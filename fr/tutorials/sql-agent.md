# Agent SQL

Ce didacticiel vous guidera à travers la création d'un agent SQL intelligent qui peut interagir avec les bases de données, générer des requêtes SQL, les valider, les exécuter et s'auto-correction lorsque des erreurs se produisent.

## Aperçu

Le flux d'agent SQL implémente un système d'interaction de base de données robuste qui:

1. Récupération des informations sur le schéma de la base de données
2. Génère des requêtes SQL en fonction des questions des utilisateurs
3. Valide les requêtes générées pour les erreurs courantes
4. Exécute des requêtes contre la base de données
5. Vérifie les résultats des erreurs et des auto-corrects en cas de besoin
6. Fournit des réponses en langage naturel en fonction des résultats de la requête

<gigne> <img src = "../. Gitbook / Assets / Image (5) (1) (1) (1) (1) (1) (1) .png" alt = ""> <figcaption> </gigcaption> </stigne>

### Étape 1: Configuration du nœud de démarrage

Commencez par ajouter un nœud ** start ** à votre toile. Cela sert de point d'entrée pour votre agent SQL.

<gigne> <img src = "../. Gitbook / Assets / image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) .png" alt = "" width = "563"> <figcaption> </ Figcaption> </gigne>

#### Configuration:

* ** Type d'entrée **: Sélectionnez "CHAT ENTRE" pour accepter les questions des utilisateurs
* ** État de flux **: Ajoutez une variable d'état avec la clé "`sqlQuery`"Et une valeur vide

Le nœud de démarrage initialise l'état de débit avec un vide`sqlQuery`variable qui stockera la requête SQL générée tout au long du processus.

### Étape 2: Récupération du schéma de base de données

Ajoutez un nœud de fonction ** personnalisé ** et connectez-le au nœud de démarrage.

<gigne> <img src = "../. Gitbook / Assets / image (3) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) .png" alt = "" width = "563"> <figcaption>

#### Configuration:

* ** Fonction JavaScript **: Il s'agit d'un exemple de fonction qui se connecte à votre base de données et récupère le schéma complet, y compris les structures de table, les définitions de colonnes et les données d'échantillons.

```javascript
const { DataSource } = require('typeorm');

const HOST = 'localhost';
const USER = 'testuser';
const PASSWORD = 'testpwd';
const DATABASE = 'testdatabase';
const PORT = 5432;

let sqlSchemaPrompt = '';

const AppDataSource = new DataSource({
  type: 'postgres',
  host: HOST,
  port: PORT,
  username: USER,
  password: PASSWORD,
  database: DATABASE,
  synchronize: false,
  logging: false,
});

async function getSQLPrompt() {
  try {
    await AppDataSource.initialize();
    const queryRunner = AppDataSource.createQueryRunner();

    // Get all user-defined tables
    const tablesResult = await queryRunner.query(`
      SELECT table_name
      FROM information_schema.tables
      WHERE table_schema = 'public' AND table_type = 'BASE TABLE'
    `);

    for (const tableRow of tablesResult) {
      const tableName = tableRow.table_name;
      const schemaInfo = await queryRunner.query(`
        SELECT column_name, data_type, is_nullable
        FROM information_schema.columns
        WHERE table_name = '${tableName}'
      `);

      const createColumns = [];
      const columnNames = [];

      for (const column of schemaInfo) {
        const name = column.column_name;
        const type = column.data_type.toUpperCase();
        const notNull = column.is_nullable === 'NO' ? 'NOT NULL' : '';
        columnNames.push(name);
        createColumns.push(`${name} ${type} ${notNull}`);
      }

      const sqlCreateTableQuery = `CREATE TABLE ${tableName} (${createColumns.join(', ')})`;
      const sqlSelectTableQuery = `SELECT * FROM ${tableName} LIMIT 3`;

      let allValues = [];
      try {
        const rows = await queryRunner.query(sqlSelectTableQuery);
        allValues = rows.map(row =>
          columnNames.map(col => row[col]).join(' ')
        );
      } catch (err) {
        allValues.push('[ERROR FETCHING ROWS]');
      }

      sqlSchemaPrompt +=
        sqlCreateTableQuery + '\n' +
        sqlSelectTableQuery + '\n' +
        columnNames.join(' ') + '\n' +
        allValues.join('\n') + '\n\n';
    }

    await queryRunner.release();
  } catch (err) {
    console.error(err);
    throw err;
  }
}

await getSQLPrompt();
return sqlSchemaPrompt;
```

### Étape 3: Génération de requêtes SQL

Ajoutez un nœud ** llm ** connecté au nœud "get db schéma".

<gigne> <img src = "../. Gitbook / Assets / image (4) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) .png" alt = "" width = "563"> <figcaption>

#### Configuration:

* ** Messages **: Ajouter un message système:

```
You are an agent designed to interact with a SQL database. Given an input question, create a syntactically correct sqlite query to run, then look at the results of the query and return the answer. Unless the user specifies a specific number of examples they wish to obtain, always limit your query to at most 5 results. You can order the results by a relevant column to return the most interesting examples in the database. Never query for all the columns from a specific table, only ask for the relevant columns given the question. DO NOT make any DML statements (INSERT, UPDATE, DELETE, DROP etc.) to the database.

Here is the relevant table info:
{{ customFunctionAgentflow_0 }}

Note:
- Only generate ONE SQL query
```

* ** Sortie structurée JSON **: Ici, nous instructons le modèle uniquement la sortie structurée, pour empêcher LLM d'inclure d'autres texte autres que la requête SQL.
  * Clé: "`sql_query`"
  * Type: "String"
  * Description: "Query SQL"
* ** Mettre à jour l'état de flux **: Set Key "`sqlQuery`"Avec valeur`{{ output.sql_query }}`

Ce nœud transforme la question du langage naturel de l'utilisateur en une requête SQL structurée à l'aide des informations de schéma de base de données.

### Étape 4: validation de la syntaxe de requête SQL

Ajoutez un nœud d'agent ** de condition ** connecté à la "Generate SQL Query" LLM.

<gigne> <img src = "../. GitBook / Assets / Image (5) (1) (1) (1) (1) (1) (1) (1) .png" alt = "" width = "563"> <figcaption> </gigcaption> </pigucial>

#### Configuration:

* **Instructions**:

```
You are a SQL expert with a strong attention to detail. Double check the SQL query for common mistakes, including:
- Using NOT IN with NULL values
- Using UNION when UNION ALL should have been used
- Using BETWEEN for exclusive ranges
- Data type mismatch in predicates
- Properly quoting identifiers
- Using the correct number of arguments for functions
- Casting to the correct data type
- Using the proper columns for joins
```

* **Saisir**:`{{ $flow.state.sqlQuery }}`
* ** Scénarios **:
  * Scénario 1: "La requête SQL est correcte et ne contient pas d'erreurs"
  * Scénario 2: "La requête SQL contient des erreurs"

Cette étape de validation attrape les erreurs SQL courantes avant l'exécution.

### Étape 5: Gestion de la régénération des requêtes (chemin d'erreur)

Pour les requêtes incorrectes (sortie 1) du nœud de l'agent de condition précédente, ajoutez un nœud ** boucle **.

<gigne> <img src = "../. Gitbook / Assets / image (6) (1) (1) (1) (1) (1) .png" alt = "" width = "375"> <figCaption> </gigcaption> </gigust>

#### Configuration:

<gigne> <img src = "../. GitBook / Assets / image (7) (1) (1) (1) (1) (1) .png" alt = "" width = "526"> <figCaption> </gigcaption> </gigust>

* ** Loop Retour à **: "Générer la requête SQL"
* ** Count de boucle maximale **: réglé sur 5

Cela crée une boucle de rétroaction qui permet au système de réessayer la génération de requête lorsque la validation échoue.

### Étape 6: Exécution des requêtes SQL valides

Pour les requêtes correctes (sortie 0), ajoutez un nœud de fonction ** personnalisé **.

<gigne> <img src = "../. GitBook / Assets / Image (8) (1) (1) (1) (1) .png" alt = "" width = "375"> <Figcaption> </ Figcaption> </gigu

#### Configuration:

<gigne> <img src = "../. GitBook / Assets / Image (9) (1) (1) (1) .png" alt = "" width = "563"> <figcaption> </gigcaption> </stigne>

* ** Variables d'entrée **: Ici, nous passons dans la requête SQL générée comme variable à utiliser dans la fonction.
  * Nom de la variable: "SqlQuery"
  * Valeur variable:`{{ $flow.state.sqlQuery }}`
* ** Fonction JavaScript **: Cette fonction exécute la requête SQL validée par rapport à la base de données et formate les résultats.

```javascript
const { DataSource } = require('typeorm');

const HOST = 'localhost';
const USER = 'testuser';
const PASSWORD = 'testpwd';
const DATABASE = 'testdatabase';
const PORT = 5432;

const sqlQuery = $sqlQuery;

const AppDataSource = new DataSource({
  type: 'postgres',
  host: HOST,
  port: PORT,
  username: USER,
  password: PASSWORD,
  database: DATABASE,
  synchronize: false,
  logging: false,
});

let formattedResult = '';

async function runSQLQuery(query) {
  try {
    await AppDataSource.initialize();
    const queryRunner = AppDataSource.createQueryRunner();

    const rows = await queryRunner.query(query);
    console.log('rows =', rows);

    if (rows.length === 0) {
      formattedResult = '[No results returned]';
    } else {
      const columnNames = Object.keys(rows[0]);
      const header = columnNames.join(' ');
      const values = rows.map(row =>
        columnNames.map(col => row[col]).join(' ')
      );

      formattedResult = query + '\n' + header + '\n' + values.join('\n');
    }

    await queryRunner.release();
  } catch (err) {
    console.error('[ERROR]', err);
    formattedResult = `[Error executing query]: ${err}`;
  }

  return formattedResult;
}

await runSQLQuery(sqlQuery);
return formattedResult;
```

### Étape 7: Vérification des résultats de l'exécution des requêtes

Ajoutez un nœud d'agent ** de condition ** connecté à la fonction "Run SQL Query".

<gigne> <img src = "../. GitBook / Assets / Image (10) (1) (1) (1) .png" alt = "" width = "563"> <figcaption> </gigcaption> </gigust>

#### Configuration:

* ** Instructions **: "Vous êtes un expert SQL. Vérifiez si le résultat de la requête est correct ou contient une erreur."
* **Saisir**:`{{ customFunctionAgentflow_1 }}`
* ** Scénarios **:
  * Scénario 1: "Le résultat est correct et ne contient pas d'erreur"
  * Scénario 2: "La requête du résultat contient une erreur"

Cette étape valide les résultats de l'exécution et détermine si une correction supplémentaire est nécessaire.

### Étape 8: Génération de réponse finale (chemin de réussite)

Pour des résultats réussis (sortie 0 de Condition Agent), ajoutez un nœud ** llm **.

<gigne> <img src = "../. GitBook / Assets / image (11) (1) (1) (1) .png" alt = "" width = "375"> <figcaption> </gigcaption> </ figure>

#### Configuration:

* ** Message d'entrée **:`{{ customFunctionAgentflow_1 }}`

Ce nœud génère une réponse en langage naturel basé sur les résultats de la requête réussis.

### Étape 9: Gestion de la régénération des requêtes (chemin d'erreur d'exécution)

Pour les exécutions défaillantes (sortie 1 de l'agent de condition), ajoutez un nœud ** llm **.

<gigne> <img src = "../. GitBook / Assets / image (12) (1) (1) (1) .png" alt = "" width = "375"> <figcaption> </gigcaption> </gigust>

#### Configuration:

<gigne> <img src = "../. Gitbook / Assets / image (13) (1) (1) (1) .png" alt = "" width = "399"> <figcaption> </gigcaption> </gigust>

* ** Messages **: Ajoutez le même message système que l'étape 3
* ** Message d'entrée **:

```
Given the generated SQL Query: {{ $flow.state.sqlQuery }}
I have the following error: {{ customFunctionAgentflow_1 }}
Regenerate a new SQL Query that will fix the error
```

* ** Sortie structurée JSON **: Identique à l'étape 3
* ** Mettre à jour l'état de flux **: Set Key "`sqlQuery`"Avec valeur`{{ output.sql_query }}`

Ce nœud analyse les erreurs d'exécution et génère des requêtes SQL corrigées.

### Étape 10: ajout de la deuxième boucle

Ajoutez un nœud ** LOOP ** connecté au "Regenerate SQL Query" LLM.

<gigne> <img src = "../. GitBook / Assets / image (14) (1) (1) (1) .png" alt = "" width = "563"> <figcaption> </gigcaption> </ figure>

#### Configuration:

* ** Boucle de retour à **: "Vérifiez la requête SQL"
* ** Count de boucle maximale **: réglé sur 5

Cela crée une deuxième boucle de rétroaction pour la correction d'erreur d'exécution.

***

## Structure d'écoulement complète

{% fichier src = "../. gitbook / actifs / sql agent.json"%}

***

## Résumé

1. Démarrer → Obtenir le schéma DB
2. Obtenez un schéma DB → Générer une requête SQL
3. Générer la requête SQL → Vérifiez la requête SQL
4. Vérifiez la requête SQL (correcte) → Exécuter la requête SQL
5. Vérifiez la requête SQL (incorrecte) → Regérer la requête (boucle arrière)
6. Exécuter la requête SQL → Vérifier le résultat
7. Vérifier le résultat (succès) → Retour Response
8. Vérifier le résultat (erreur) → Regérer la requête SQL
9. Regenerate SQL Query → ReCheck SQL Query (boucle arrière)

***

## Tester votre agent SQL

Testez votre agent avec différents types de questions de base de données:

* Requêtes simples: "Montrez-moi tous les clients"
* Requêtes complexes: "Quels sont les 5 meilleurs produits par vente?"
* Requêtes analytiques: "Calculez la valeur moyenne de l'ordre par mois"

<gigne> <img src = "../. GitBook / Assets / Image (15) (1) (1) (1) .png" alt = "" width = "563"> <figcaption> </gigcaption> </gigust>

Ce flux d'agent SQL fournit un système robuste et auto-corrigé pour les interactions de base de données qui peuvent gérer les requêtes SQL en langage naturel.
