---
description: Apprenez à connecter votre instance Flowise à une base de données
---

# Bases de données

---

## Configuration

Flowise prend en charge 4 types de bases de données :

- SQLite
- MySQL
- PostgreSQL
- MariaDB

### SQLite (Par défaut)

SQLite sera la base de données par défaut. Ces bases de données peuvent être configurées avec les variables d'environnement suivantes :

```sh
DATABASE_TYPE=sqlite
DATABASE_PATH=/root/.flowise #your preferred location
```

Un fichier `database.sqlite` sera créé et enregistré dans le chemin spécifié par `DATABASE_PATH`. Si aucun chemin n'est spécifié, le chemin de stockage par défaut sera dans votre répertoire personnel -> .flowise

**Remarque :** Si aucune des variables d'environnement n'est spécifiée, SQLite sera le choix de base de données par défaut.

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

### Comment utiliser les bases de données Flowise SQLite et MySQL/MariaDB

{% embed url="https://youtu.be/R-6uV1Cb8I8" %}

## Sauvegarde

1. Fermez l'application FlowiseAI.
2. Assurez-vous que la connexion à la base de données avec d'autres applications est désactivée.
3. Sauvegardez votre base de données.
4. Testez la base de données de sauvegarde.

### SQLite

1. Renommez le nom du fichier.

   Windows:

   ```bash
   rename "DATABASE_PATH\database.sqlite" "DATABASE_PATH\BACKUP_FILE_NAME.sqlite"
   ```

Linux:

   ```bash
   mv DATABASE_PATH/database.sqlite DATABASE_PATH/BACKUP_FILE_NAME.sqlite
   ```

2. Sauvegarder la base de données.

   Windows:

   ```bash
   copy DATABASE_PATH\BACKUP_FILE_NAME.sqlite DATABASE_PATH\database.sqlite
   ```

Linux:

   ```bash
   cp DATABASE_PATH/BACKUP_FILE_NAME.sqlite DATABASE_PATH/database.sqlite
   ```

3. Tester la base de données de sauvegarde en exécutant Flowise.

### PostgreSQL

1. Sauvegarder la base de données.

   ```bash
   pg_dump -U USERNAME -h HOST -p PORT -d DATABASE_NAME -f /PATH/TO/BACKUP_FILE_NAME.sql
   ```

2. Entrez le mot de passe de la base de données.  
3. Créez une base de données de test.
   ```bash
   psql -U USERNAME -h HOST -p PORT -d TEST_DATABASE_NAME -f /PATH/TO/BACKUP_FILE_NAME.sql
   ```
4. Testez la base de données de sauvegarde en exécutant Flowise avec le fichier `.env` modifié pour pointer vers la base de données de sauvegarde.

### MySQL & MariaDB

1. Base de données de sauvegarde.

   ```bash
   mysqldump -u USERNAME -p DATABASE_NAME > BACKUP_FILE_NAME.sql
   ```

2. Entrez le mot de passe de la base de données.  
3. Créez une base de données de test.
   ```bash
   mysql -u USERNAME -p TEST_DATABASE_NAME < BACKUP_FILE_NAME.sql
   ```
4. Testez la base de données de sauvegarde en exécutant Flowise avec le fichier `.env` modifié pour pointer vers la base de données de sauvegarde.