---
description: 了解如何将 Flowise 实例连接到数据库
---

# 数据库

---

## 设置

Flowise 支持 4 种数据库类型：

- SQLite
- MySQL
- PostgreSQL
- 玛丽亚数据库

### SQLite（默认）

SQLite 将是默认数据库。这些数据库可以使用以下环境变量进行配置：

```sh
DATABASE_TYPE=sqlite
DATABASE_PATH=/root/.flowise #your preferred location
```

将创建 `database.sqlite` 文件并将其保存在 `DATABASE_PATH` 指定的路径中。如果未指定，默认存储路径将位于您的主目录 -> .flowise

**注意：** 如果未指定任何环境变量，SQLite 将作为后备数据库选择。

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

### 玛丽亚数据库

```bash
DATABASE_TYPE="mariadb"
DATABASE_PORT="3306"
DATABASE_HOST="localhost"
DATABASE_NAME="flowise"
DATABASE_USER="flowise"
DATABASE_PASSWORD="mypassword"
```

### 如何使用 Flowise 数据库 SQLite 和 MySQL/MariaDB

{% embed url="https://youtu.be/R-6uV1Cb8I8" %}

## 备份

1. 关闭 FlowiseAI 应用程序。
2. 确保与其他应用程序的数据库连接已关闭。
3. 备份数据库。
4. 测试备份数据库。

### SQLite

1. 重命名文件名。

   窗户：

   ```bash
   rename "DATABASE_PATH\database.sqlite" "DATABASE_PATH\BACKUP_FILE_NAME.sqlite"
   ```

   Linux：

   ```bash
   mv DATABASE_PATH/database.sqlite DATABASE_PATH/BACKUP_FILE_NAME.sqlite
   ```

2.备份数据库。

   窗户：

   ```bash
   copy DATABASE_PATH\BACKUP_FILE_NAME.sqlite DATABASE_PATH\database.sqlite
   ```

   Linux：

   ```bash
   cp DATABASE_PATH/BACKUP_FILE_NAME.sqlite DATABASE_PATH/database.sqlite
   ```

3. 通过运行 Flowise 测试备份数据库。

### PostgreSQL

1.备份数据库。

   ```bash
   pg_dump -U USERNAME -h HOST -p PORT -d DATABASE_NAME -f /PATH/TO/BACKUP_FILE_NAME.sql
   ```

2. 输入数据库密码。
3. 创建测试数据库。
   ```bash
   psql -U USERNAME -h HOST -p PORT -d TEST_DATABASE_NAME -f /PATH/TO/BACKUP_FILE_NAME.sql
   ```
4. 通过运行 Flowise 来测试备份数据库，并将 `.env` 文件修改为指向备份数据库。

### MySQL 和 MariaDB

1.备份数据库。

   ```bash
   mysqldump -u USERNAME -p DATABASE_NAME > BACKUP_FILE_NAME.sql
   ```

2. 输入数据库密码。
3. 创建测试数据库。
   ```bash
   mysql -u USERNAME -p TEST_DATABASE_NAME < BACKUP_FILE_NAME.sql
   ```
4. 通过运行 Flowise 来测试备份数据库，并将 `.env` 文件修改为指向备份数据库。
