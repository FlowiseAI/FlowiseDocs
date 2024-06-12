---
description: Learn how to connect your Flowise instance to a database
---

# Databases

***

Flowise supports 4 database types:

* SQLite
* MySQL
* PostgreSQL
* MariaDB

## SQLite (Default)

SQLite will be the default database. These databases can be configured with following env variables:

```sh
DATABASE_TYPE=sqlite
DATABASE_PATH=/root/.flowise #your preferred location
```

A `database.sqlite` file will be created and saved in the path specified by `DATABASE_PATH`. If not specified, the default store path will be in your home directory -> .flowise

**Note:** If none of the env variables is specified, SQLite will be the fallback database choice.

## MySQL

```sh
DATABASE_TYPE=mysql
DATABASE_PORT=3306
DATABASE_HOST=localhost
DATABASE_NAME=flowise
DATABASE_USER=user
DATABASE_PASSWORD=123
```

## PostgreSQL

```sh
DATABASE_TYPE=postgres
DATABASE_PORT=5432
DATABASE_HOST=localhost
DATABASE_NAME=flowise
DATABASE_USER=user
DATABASE_PASSWORD=123
PGSSLMODE=require
```

## MariaDB

```bash
DATABASE_TYPE="mariadb"
DATABASE_PORT="3306"
DATABASE_HOST="localhost"
DATABASE_NAME="flowise"
DATABASE_USER="flowise"
DATABASE_PASSWORD="mypassword"
```

## Synchronize in Production

Flowise uses [Typeorm](https://typeorm.io/data-source-options#common-data-source-options) to configure database connection. By default, synchronize is set to true. This indicates if database schema should be auto created on every application launch.

However, we have to be careful with this option and don't use this in production - otherwise you can <mark style="color:red;">**lose**</mark> production data. This option is useful during debug and development.

To override the value, set the following env variable

```sh
OVERRIDE_DATABASE=false
```

## How to use Flowise databases SQLite and MySQL/MariaDB

{% embed url="https://youtu.be/R-6uV1Cb8I8" %}
