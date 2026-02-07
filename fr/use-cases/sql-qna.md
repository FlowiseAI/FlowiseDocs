---
description: Learn how to query structured data
---

# SQL QNA

***

Contrairement aux exemples précédents comme[Web Scrape QnA](web-scrape-qna.md)et[Multiple Documents QnA](multiple-documents-qna.md), interroger les données structurées ne nécessite pas de base de données vectorielle. Au niveau élevé, cela peut être réalisé avec les étapes suivantes:

1. Fournir le LLM:
   * Aperçu du schéma de base de données SQL
   * Exemples de données de lignes
2. Renvoyez une requête SQL avec peu d'incitation
3. Valider la requête SQL à l'aide d'un[If Else](../integrations/utilities/if-else.md)nœud
4. Créez une fonction personnalisée pour exécuter la requête SQL et obtenez la réponse
5. Renvoyer une réponse naturelle de la réponse SQL exécutée

<gigne> <img src = "../. GitBook / Assets / Image (113) .png" alt = ""> <Figcaption> </gigcaption> </gigust>

Dans cet exemple, nous allons créer un chatbot QNA qui peut interagir avec une base de données SQL stockée à Singlestore

<gigne> <img src = "../. GitBook / Assets / Image (116) .png" alt = ""> <Figcaption> </gigcaption> </gigust>

## Tl; dr

Vous pouvez trouver le modèle ChatFlow:

{% fichier src = "../. gitbook / actifs / sql chatflow.json"%}

## 1. Schéma de base de données SQL + Exemples de lignes

Utilisez un nœud de fonction JS personnalisé pour vous connecter à Singlestore, récupérer le schéma de base de données et les 3 premières lignes.

De[research paper](https://arxiv.org/abs/2204.00498), il est recommandé de générer une invite avec le format d'exemple suivant:

```
CREATE TABLE samples (firstName varchar NOT NULL, lastName varchar)
SELECT * FROM samples LIMIT 3
firstName lastName
Stephen Tyler
Jack McGinnis
Steven Repici
```

<gigne> <img src = "../. GitBook / Assets / Image (114) .png" alt = ""> <figcaption> </gigcaption> </gigust>

<Dettots>

<summary> Code JavaScript complet </summary>

```javascript
const HOST = 'singlestore-host.com';
const USER = 'admin';
const PASSWORD = 'mypassword';
const DATABASE = 'mydb';
const TABLE = 'samples';
const mysql = require('mysql2/promise');

let sqlSchemaPrompt;

function getSQLPrompt() {
  return new Promise(async (resolve, reject) => {
    try {
      const singleStoreConnection = mysql.createPool({
        host: HOST,
        user: USER,
        password: PASSWORD,
        database: DATABASE,
      });
  
      // Get schema info
      const [schemaInfo] = await singleStoreConnection.execute(
        `SELECT * FROM INFORMATION_SCHEMA.COLUMNS WHERE table_name = "${TABLE}"`
      );
  
      const createColumns = [];
      const columnNames = [];
  
      for (const schemaData of schemaInfo) {
        columnNames.push(`${schemaData['COLUMN_NAME']}`);
        createColumns.push(`${schemaData['COLUMN_NAME']} ${schemaData['COLUMN_TYPE']} ${schemaData['IS_NULLABLE'] === 'NO' ? 'NOT NULL' : ''}`);
      }
  
      const sqlCreateTableQuery = `CREATE TABLE samples (${createColumns.join(', ')})`;
      const sqlSelectTableQuery = `SELECT * FROM samples LIMIT 3`;
  
      // Get first 3 rows
      const [rows] = await singleStoreConnection.execute(
          sqlSelectTableQuery,
      );
      
      const allValues = [];
      for (const row of rows) {
          const rowValues = [];
          for (const colName in row) {
              rowValues.push(row[colName]);
          }
          allValues.push(rowValues.join(' '));
      }
  
      sqlSchemaPrompt = sqlCreateTableQuery + '\n' + sqlSelectTableQuery + '\n' + columnNames.join(' ') + '\n' + allValues.join('\n');
      
      resolve();
    } catch (e) {
      console.error(e);
      return reject(e);
    }
  });
}

async function main() {
    await getSQLPrompt();
}

await main();

return sqlSchemaPrompt;
```

</fords>

Vous pouvez en savoir plus sur la façon d'obtenir le`HOST`, `USER`, `PASSWORD`de cette[guide](broken-reference/). Une fois terminé, cliquez sur Exécuter:

<gigne> <img src = "../. GitBook / Assets / Image (117) .png" alt = ""> <Figcaption> </gigcaption> </gigust>

Nous pouvons maintenant voir que le bon format a été généré. La prochaine étape consiste à mettre cela dans le modèle invite.

## 2. Renvoyez une requête SQL avec peu d'incitation

Créer un nouveau modèle de chat + modèle d'invite + llmchain

<Figure> <img src = "../. GitBook / Assets / Image (118) .png" alt = ""> <Figcaption> </gigcaption> </gigust>

Spécifiez l'invite suivante dans le modèle d'invite:

```
Based on the provided SQL table schema and question below, return a SQL SELECT ALL query that would answer the user's question. For example: SELECT * FROM table WHERE id = '1'.
------------
SCHEMA: {schema}
------------
QUESTION: {question}
------------
SQL QUERY:
```

Puisque nous utilisons 2 variables: {schéma} et {question}, spécifiez leurs valeurs dans les valeurs d'invite de format ** **:

<gigne> <img src = "../. GitBook / Assets / Image (122) .png" alt = "" width = "563"> <Figcaption> </gigcaption> </ Figure>

{% hint style = "info"%}
Vous pouvez fournir plus d'exemples à l'invite (c'est-à-dire une invitation à quelques coups) pour permettre que le LLM apprenne mieux. Ou faire référence à[dialect-specific prompting](https://js.langchain.com/docs/use\_cases/sql/prompting#dialect-specific-prompting)
{% EndHint%}

## 3. Valider la requête SQL en utilisant[If Else](../integrations/utilities/if-else.md)nœud

Parfois, la requête SQL n'est pas valide, et nous ne voulons pas gaspiller les ressources de l'exécution d'une requête SQL non valide. Par exemple, si un utilisateur pose une question générale qui n'est pas pertinente pour la base de données SQL. Nous pouvons utiliser un`If Else`Node pour se rendre à un chemin différent.

Par exemple, nous pouvons effectuer une vérification de base pour voir si Sélectionner et où sont incluses dans la requête SQL donnée par le LLM.

{% Tabs%}
{% tab title = "if function"%}
```javascript
const sqlQuery = $sqlQuery.trim();

const regex = /SELECT\s.*?(?:\n|$)/gi;

// Extracting the SQL part
const matches = sqlQuery.match(regex);
const cleanSql = matches ? matches[0].trim() : "";

if (cleanSql.includes("SELECT") && cleanSql.includes("WHERE")) {
    return cleanSql;
}
```
{% endtab%}

{% tab title = "else function"%}
```javascript
return $sqlQuery;
```
{% endtab%}
{% endtabs%}

<gigne> <img src = "../. GitBook / Assets / Image (119) .png" alt = "" width = "327"> <Figcaption> </ Figcaption> </gigust>

Dans la fonction ELSE, nous irons vers un modèle rapide + llmchain qui indique essentiellement à LLM qu'il n'est pas en mesure de répondre à la requête utilisateur:

<gigne> <img src = "../. GitBook / Assets / Image (120) .png" alt = ""> <Figcaption> </gigcaption> </gigust>

## 4. Fonction personnalisée pour exécuter la requête SQL et obtenir la réponse

S'il s'agit d'une requête SQL valide, nous devons exécuter la requête. Connectez la sortie _ ** true ** _ de ** nœud if else ** à une fonction ** js personnalisée ** nœud:

<gigne> <img src = "../. GitBook / Assets / Image (123) .png" alt = "" width = "563"> <Figcaption> </gigcaption> </gigust>

<Dettots>

<summary> Code JavaScript complet </summary>

```javascript
const HOST = 'singlestore-host.com';
const USER = 'admin';
const PASSWORD = 'mypassword';
const DATABASE = 'mydb';
const TABLE = 'samples';
const mysql = require('mysql2/promise');

let result;

function getSQLResult() {
  return new Promise(async (resolve, reject) => {
    try {
      const singleStoreConnection = mysql.createPool({
        host: HOST,
        user: USER,
        password: PASSWORD,
        database: DATABASE,
      });
     
      const [rows] = await singleStoreConnection.execute(
        $sqlQuery
      );
  
      result = JSON.stringify(rows)
      
      resolve();
    } catch (e) {
      console.error(e);
      return reject(e);
    }
  });
}

async function main() {
    await getSQLResult();
}

await main();

return result;
```

</fords>

## 5. Renvoie une réponse naturelle de la réponse SQL exécutée

Créer un nouveau modèle de chat + modèle d'invite + llmchain

<gigne> <img src = "../. GitBook / Assets / Image (124) .png" alt = ""> <Figcaption> </gigcaption> </gigne>

Écrivez l'invite suivante dans le modèle d'invite:

```
Based on the question, and SQL response, write a natural language response, be details as possible:
------------
QUESTION: {question}
------------
SQL RESPONSE: {sqlResponse}
------------
NATURAL LANGUAGE RESPONSE:
```

Spécifiez les variables dans les valeurs d'invite de format ** **:

<gigne> <img src = "../. GitBook / Assets / Image (125) .png" alt = "" width = "563"> <Figcaption> </gigcaption> </gigust>

Le tour est joué! Votre chatbot SQL est maintenant prêt pour les tests!

## Requête

Tout d'abord, demandons quelque chose lié à la base de données.

<gigne> <img src = "../. GitBook / Assets / Image (128) .png" alt = "" width = "434"> <Figcaption> </ Figcaption> </gigust>

En regardant les journaux, nous pouvons voir que le premier LLMCHAIN ​​est capable de nous donner une requête SQL:

**Saisir:**

{% code overflow = "wrap"%}
```
Based on the provided SQL table schema and question below, return a SQL SELECT ALL query that would answer the user's question. For example: SELECT * FROM table WHERE id = '1'.\n------------\nSCHEMA: CREATE TABLE samples (id bigint(20) NOT NULL, firstName varchar(300) NOT NULL, lastName varchar(300) NOT NULL, userAddress varchar(300) NOT NULL, userState varchar(300) NOT NULL, userCode varchar(300) NOT NULL, userPostal varchar(300) NOT NULL, createdate timestamp(6) NOT NULL)\nSELECT * FROM samples LIMIT 3\nid firstName lastName userAddress userState userCode userPostal createdate\n1125899906842627 Steven Repici 14 Kingston St. Oregon NJ 5578 Thu Dec 14 2023 13:06:17 GMT+0800 (Singapore Standard Time)\n1125899906842625 John Doe 120 jefferson st. Riverside NJ 8075 Thu Dec 14 2023 13:04:32 GMT+0800 (Singapore Standard Time)\n1125899906842629 Bert Jet 9th, at Terrace plc Desert City CO 8576 Thu Dec 14 2023 13:07:11 GMT+0800 (Singapore Standard Time)\n------------\nQUESTION: what is the address of John\n------------\nSQL QUERY:
```
{% Endcode%}

**Sortir**

<pre class = "Language-SQL"> <Code class = "Lang-SQL"> <strong> Sélectionnez UserAddress à partir d'échantillons où FirstName = 'John'
</strong> </code> </pre>

Après avoir exécuté la requête SQL, le résultat est transmis au 2e llmchain:

**Saisir**

{% code overflow = "wrap"%}
```
Based on the question, and SQL response, write a natural language response, be details as possible:\n------------\nQUESTION: what is the address of John\n------------\nSQL RESPONSE: [{\"userAddress\":\"120 jefferson st.\"}]\n------------\nNATURAL LANGUAGE RESPONSE:
```
{% Endcode%}

**Sortir**

```
The address of John is 120 Jefferson St.
```

Maintenant, si nous demandons quelque chose qui n'est pas pertinent pour la base de données SQL, l'itinéraire ELSE est emprunté.

<gigne> <img src = "../. GitBook / Assets / image (132) .png" alt = "" width = "428"> <Figcaption> </gigcaption> </gigust>

Pour le premier llmchain, une requête SQL est générée comme ci-dessous:

```sql
SELECT * FROM samples LIMIT 3
```

Cependant, il échoue`If Else`Vérifiez car il ne contient pas les deux`SELECT`et`WHERE`, donc entrant sur l'itinéraire des autres qui a une invite qui dit:

```
Politely say "I'm not able to answer query"
```

Et la sortie finale est:

```
I apologize, but I'm not able to answer your query at the moment.
```

## Conclusion

Dans cet exemple, nous avons créé avec succès un chatbot SQL qui peut interagir avec votre base de données et est également en mesure de gérer des questions qui ne sont pas pertinentes pour la base de données. Une amélioration supplémentaire comprend l'ajout de mémoire pour fournir l'historique des conversations.

Vous pouvez trouver le Chatflow ci-dessous:

{% fichier src = "../. gitbook / actifs / sql chatflow (1) .json"%}
