# Overview

# Prerequisites

- Python `>= 3.9.0`


# Run

## Locally

```bash
git clone https://github.com/airbytehq/airbyte.git
cd airbyte
docker-compose up
# open http://localhost:8000
```

# Setup Source

## Postgres Source

1. Install postgres

2. Open `psql` using `postgres` linux user

```bash
$ sudo -u postgres psql
```

Just check if things works fine

```sql
postgres=# \conninfo
```

3. Create a source role & db

<!-- ```bash -->
<!-- $ sudo -u postgres createuser --interactive  -->
<!-- # name it as `postgres_airbyte_src`, with no superuser access -->

<!-- $ sudo -u postgres createdb postgres_airbyte_src -->
<!-- # postgres usually maps a role with same named database -->
<!-- ``` -->

```sql
postgres=# CREATE USER postgres_airbyte_src PASSWORD 'postgres_airbyte_src';
postgres=# ALTER ROLE postgres_airbyte WITH LOGIN;
postgres=# GRANT USAGE ON SCHEMA public TO postgres_airbyte_src;
postgres=# GRANT SELECT ON ALL TABLES IN SCHEMA public TO postgres_airbyte_src;
```

4. Validate if able to access

```bash
$ psql -h 127.0.0.1 -p 5432 -U postgres_airbyte_src --password
```

5. Fillup on Airbyte UI

```
Name=Postgres Source
Host=127.0.0.1
Port=5432
DB Name=postgres_airbyte_src
Schemas=public  # is a list
User=postgres_airbyte_src
Password=postgres_airbyte_src
```



# Setup Destination

## Postgres Destination

1. Open `psql` using `postgres` linux user

```bash
$ sudo -u postgres psql
```


2. Create destination role & db

<!-- ```bash -->
<!-- $ sudo -i -u postgres createuser --interactive -->
<!-- Enter name of role to add: postgres_airbyte_dest -->
<!-- Shall the new role be a superuser? (y/n) n -->
<!-- Shall the new role be allowed to create databases? (y/n) y -->
<!-- Shall the new role be allowed to create more new roles? (y/n) n -->
<!-- ``` -->


```sql
postgres=# CREATE DATABASE postgres_airbyte_dest;
postgres=# CREATE ROLE postgres_airbyte_dest PASSWORD 'postgres_airbyte_dest';
postgres=# ALTER ROLE postgres_airbyte_dest WITH LOGIN;
postgres=# GRANT ALL ON SCHEMA public TO postgres_airbyte_dest;
postgres=# GRANT ALL ON ALL TABLES IN SCHEMA public TO postgres_airbyte_dest;
postgres=# GRANT CREATE, TEMPORARY ON DATABASE postgres_airbyte_dest TO postgres_airbyte_dest;
```

3. Validate

```bash
$ psql -h 127.0.0.1 -p 5432 -U postgres_airbyte_dest
Password for user postgres_airbyte_dest:
```

```sql
postgres_airbyte_dest=> \conninfo
You are connected to database "postgres_airbyte_dest" as user "postgres_airbyte_dest" on host "127.0.0.1" at port "5432".
SSL connection (protocol: TLSv1.3, cipher: TLS_AES_256_GCM_SHA384, bits: 256, compression: off)
```


4. Fillup on Airbyte UI

```
Name=Postgres Destination
Host=127.0.0.1
Port=5432
DB Name=postgres_airbyte_dest
Schemas=public  # is a list
User=postgres_airbyte_dest
Password=postgres_airbyte_dest
```
