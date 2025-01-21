---
description: Aprende cómo consultar datos estructurados
---

# SQL QnA

***

A diferencia de ejemplos anteriores como [QnA con Web Scraping](web-scrape-qna.md) y [QnA con Múltiples Documentos](multiple-documents-qna.md), consultar datos estructurados no requiere una base de datos vectorial. A alto nivel, esto se puede lograr con los siguientes pasos:

1. Proporcionar al LLM:
   * visión general del esquema de la base de datos SQL
   * datos de ejemplo de filas
2. Devolver una consulta SQL con few shot prompting
3. Validar la consulta SQL usando un nodo [If Else](../integrations/utilities/if-else.md)
4. Crear una función personalizada para ejecutar la consulta SQL y obtener la respuesta
5. Devolver una respuesta natural de la respuesta SQL ejecutada

<figure><img src="../.gitbook/assets/image (113).png" alt=""><figcaption></figcaption></figure>

En este ejemplo, vamos a crear un chatbot QnA que pueda interactuar con una base de datos SQL almacenada en SingleStore

<figure><img src="../.gitbook/assets/image (116).png" alt=""><figcaption></figcaption></figure>

## TL;DR

Puedes encontrar la plantilla del chatflow:

{% file src="../.gitbook/assets/SQL Chatflow.json" %}

## 1. Esquema de Base de Datos SQL + Filas de Ejemplo

Usa un nodo Custom JS Function para conectarte a SingleStore, recuperar el esquema de la base de datos y las 3 primeras filas.

Según el [paper de investigación](https://arxiv.org/abs/2204.00498), se recomienda generar un prompt con el siguiente formato de ejemplo:

```
CREATE TABLE samples (firstName varchar NOT NULL, lastName varchar)
SELECT * FROM samples LIMIT 3
firstName lastName
Stephen Tyler
Jack McGinnis
Steven Repici
```

<figure><img src="../.gitbook/assets/image (114).png" alt=""><figcaption></figcaption></figure>

<details>

<summary>Código Javascript Completo</summary>

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

</details>

Puedes encontrar más información sobre cómo obtener el `HOST`, `USER`, `PASSWORD` en esta [guía](broken-reference/). Una vez terminado, haz clic en Execute:

<figure><img src="../.gitbook/assets/image (117).png" alt=""><figcaption></figcaption></figure>

Ahora podemos ver que se ha generado el formato correcto. El siguiente paso es llevarlo al Prompt Template.

## 2. Devolver una consulta SQL con few shot prompting

Crea un nuevo Chat Model + Prompt Template + LLMChain

<figure><img src="../.gitbook/assets/image (118).png" alt=""><figcaption></figcaption></figure>

Especifica el siguiente prompt en el Prompt Template:

```
Basado en el esquema de tabla SQL proporcionado y la pregunta a continuación, devuelve una consulta SQL SELECT ALL que respondería la pregunta del usuario. Por ejemplo: SELECT * FROM table WHERE id = '1'.
------------
SCHEMA: {schema}
------------
QUESTION: {question}
------------
SQL QUERY:
```

Como estamos usando 2 variables: {schema} y {question}, especifica sus valores en **Format Prompt Values**:

<figure><img src="../.gitbook/assets/image (122).png" alt="" width="563"><figcaption></figcaption></figure>

{% hint style="info" %}
Puedes proporcionar más ejemplos al prompt (es decir, few-shot prompting) para que el LLM aprenda mejor. O toma referencia del [dialect-specific prompting](https://js.langchain.com/docs/use_cases/sql/prompting#dialect-specific-prompting)
{% endhint %}

## 3. Validar la consulta SQL usando el nodo [If Else](../integrations/utilities/if-else.md)

A veces la consulta SQL es inválida, y no queremos desperdiciar recursos ejecutando una consulta SQL inválida. Por ejemplo, si un usuario está haciendo una pregunta general que es irrelevante para la base de datos SQL. Podemos usar un nodo `If Else` para enrutar a diferentes caminos.

Por ejemplo, podemos realizar una verificación básica para ver si SELECT y WHERE están incluidos en la consulta SQL dada por el LLM.

{% tabs %}
{% tab title="If Function" %}
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
{% endtab %}

{% tab title="Else Function" %}
```javascript
return $sqlQuery;
```
{% endtab %}
{% endtabs %}

<figure><img src="../.gitbook/assets/image (119).png" alt="" width="327"><figcaption></figcaption></figure>

En la Else Function, enrutaremos a un Prompt Template + LLMChain que básicamente le dice al LLM que no puede responder la consulta del usuario:

<figure><img src="../.gitbook/assets/image (120).png" alt=""><figcaption></figcaption></figure>

## 4. Función personalizada para ejecutar la consulta SQL y obtener la respuesta

Si es una consulta SQL válida, necesitamos ejecutarla. Conecta la salida _**True**_ del nodo **If Else** a un nodo **Custom JS Function**:

<figure><img src="../.gitbook/assets/image (123).png" alt="" width="563"><figcaption></figcaption></figure>

<details>

<summary>Código Javascript Completo</summary>

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

</details>

## 5. Devolver una respuesta natural de la respuesta SQL ejecutada

Crea un nuevo Chat Model + Prompt Template + LLMChain

<figure><img src="../.gitbook/assets/image (124).png" alt=""><figcaption></figcaption></figure>

Write the following prompt in the Prompt Template:

```
Based on the question, and SQL response, write a natural language response, be details as possible:
------------
QUESTION: {question}
------------
SQL RESPONSE: {sqlResponse}
------------
NATURAL LANGUAGE RESPONSE:
```

Specify the variables in **Format Prompt Values**:

<figure><img src="../.gitbook/assets/image (125).png" alt="" width="563"><figcaption></figcaption></figure>

Voila! Your SQL chatbot is now ready for testing!

## Query

First, let's ask something related to the database.

<figure><img src="../.gitbook/assets/image (128).png" alt="" width="434"><figcaption></figcaption></figure>

Looking at the logs, we can see the first LLMChain is able to give us a SQL query:

**Input:**

{% code overflow="wrap" %}
```
Based on the provided SQL table schema and question below, return a SQL SELECT ALL query that would answer the user's question. For example: SELECT * FROM table WHERE id = '1'.\n------------\nSCHEMA: CREATE TABLE samples (id bigint(20) NOT NULL, firstName varchar(300) NOT NULL, lastName varchar(300) NOT NULL, userAddress varchar(300) NOT NULL, userState varchar(300) NOT NULL, userCode varchar(300) NOT NULL, userPostal varchar(300) NOT NULL, createdate timestamp(6) NOT NULL)\nSELECT * FROM samples LIMIT 3\nid firstName lastName userAddress userState userCode userPostal createdate\n1125899906842627 Steven Repici 14 Kingston St. Oregon NJ 5578 Thu Dec 14 2023 13:06:17 GMT+0800 (Singapore Standard Time)\n1125899906842625 John Doe 120 jefferson st. Riverside NJ 8075 Thu Dec 14 2023 13:04:32 GMT+0800 (Singapore Standard Time)\n1125899906842629 Bert Jet 9th, at Terrace plc Desert City CO 8576 Thu Dec 14 2023 13:07:11 GMT+0800 (Singapore Standard Time)\n------------\nQUESTION: what is the address of John\n------------\nSQL QUERY:
```
{% endcode %}

**Output**

<pre class="language-sql"><code class="lang-sql"><strong>SELECT userAddress FROM samples WHERE firstName = 'John'
</strong></code></pre>

After executing the SQL query, the result is passed to the 2nd LLMChain:

**Input**

{% code overflow="wrap" %}
```
Based on the question, and SQL response, write a natural language response, be details as possible:\n------------\nQUESTION: what is the address of John\n------------\nSQL RESPONSE: [{\"userAddress\":\"120 jefferson st.\"}]\n------------\nNATURAL LANGUAGE RESPONSE:
```
{% endcode %}

**Output**

```
The address of John is 120 Jefferson St.
```

Now, we if ask something that is irrelevant to the SQL database, the Else route is taken.

<figure><img src="../.gitbook/assets/image (132).png" alt="" width="428"><figcaption></figcaption></figure>

For first LLMChain, a SQL query is generated as below:

```sql
SELECT * FROM samples LIMIT 3
```

However, it fails the `If Else` check because it doesn't contains both `SELECT` and `WHERE`, hence entering the Else route that has a prompt that says:

```
Politely say "I'm not able to answer query"
```

And the final output is:

```
I apologize, but I'm not able to answer your query at the moment.
```

## Conclusion

In this example, we have successfully created a SQL chatbot that can interact with your database, and is also able to handle questions that are irrelevant to database. Further improvement includes adding memory to provide conversation history.

You can find the chatflow below:

{% file src="../.gitbook/assets/SQL Chatflow (1).json" %}
