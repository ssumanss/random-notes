---
layout: default
title: Postgresql
nav_order: 5
---
## Installing Postgresql

Starting postgres

```bash
sudo -u postgres psql
```

Creating username and database

```sql
CREATE USER username; 
CREATE ROLE username superuser;
ALTER ROLE username WITH LOGIN;
```

Creating DATABASE

```sql
postgres=# create database mydb;  
postgres=# create user myuser with encrypted password 'mypass';  
postgres=# grant all privileges on database mydb to myuser;
```

Change encrption on the file

```bash
nano /etc/postgresql/14/main/postgresql.conf
nano /etc/postgresql/14/main/hba_file;
```

```

```

Connecting with python install the package

```bash
pip install psycopg2
```
