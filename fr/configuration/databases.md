---
description: Learn how to connect your Flowise instance to a database
---

# Databases

---

## Setup

Flowise supports 4 database types:

- SQLite
- MySQL
- PostgreSQL
- MariaDB

### SQLite (Default)

SQLite will be the default database. These databases can be configured with following env variables:

```sh
DATABASE_TYPE=sqlite
DATABASE_PATH=/root/.flowise #your preferred location
```

A `database.sqlite` file will be created and saved in the path specified by `DATABASE_PATH`. If not specified, the default store path will be in your home directory -> .flowise

**Note:** If none of the env variables is specified, SQLite will be the fallback database choice.

### MySQL

```sh
DATABASE_TYPE=mysql
DATABASE_PORT=3306
DATABASE_HOST=localhost
DATABASE_NAME=flowise
DATABASE_USER=user
DATABASE_PASSWORD=123
```

### PostgreSQL

```sh
DATABASE_TYPE=postgres
DATABASE_PORT=5432
DATABASE_HOST=localhost
DATABASE_NAME=flowise
DATABASE_USER=user
DATABASE_PASSWORD=123
PGSSLMODE=require
```

### MariaDB

```bash
DATABASE_TYPE="mariadb"
DATABASE_PORT="3306"
DATABASE_HOST="localhost"
DATABASE_NAME="flowise"
DATABASE_USER="flowise"
DATABASE_PASSWORD="mypassword"
```

### How to use Flowise databases SQLite and MySQL/MariaDB

{% embed url="https://youtu.be/R-6uV1Cb8I8" %}

## Backup

1. Shut down FlowiseAI application.
2. Ensure that the database connection to other applications is turned off.
3. Backup your database.
4. Test backup database.

### SQLite

1. Rename file name.

   Windows:

   ```bash
   rename "DATABASE_PATH\database.sqlite" "DATABASE_PATH\BACKUP_FILE_NAME.sqlite"
   ```

   Linux:

   ```bash
   mv DATABASE_PATH/database.sqlite DATABASE_PATH/BACKUP_FILE_NAME.sqlite
   ```

2. Backup database.

   Windows:

   ```bash
   copy DATABASE_PATH\BACKUP_FILE_NAME.sqlite DATABASE_PATH\database.sqlite
   ```

   Linux:

   ```bash
   cp DATABASE_PATH/BACKUP_FILE_NAME.sqlite DATABASE_PATH/database.sqlite
   ```

3. Test backup database by running Flowise.

### PostgreSQL

1. Backup database.

   ```bash
   pg_dump -U USERNAME -h HOST -p PORT -d DATABASE_NAME -f /PATH/TO/BACKUP_FILE_NAME.sql
   ```

2. Enter database password.
3. Create test database.
   ```bash
   psql -U USERNAME -h HOST -p PORT -d TEST_DATABASE_NAME -f /PATH/TO/BACKUP_FILE_NAME.sql
   ```
4. Test the backup database by running Flowise with the `.env` file modified to point to the backup database.

### MySQL & MariaDB

1. Backup database.

   ```bash
   mysqldump -u USERNAME -p DATABASE_NAME > BACKUP_FILE_NAME.sql
   ```

2. Enter database password.
3. Create test database.
   ```bash
   mysql -u USERNAME -p TEST_DATABASE_NAME < BACKUP_FILE_NAME.sql
   ```
4. Test the backup database by running Flowise with the `.env` file modified to point to the backup database.
