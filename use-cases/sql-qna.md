# SQL QnA

Unlike previous examples like [Web Scrape QnA](web-scrape-qna.md) and [Multiple Documents QnA](multiple-documents-qna.md), querying structured data does not require vector database. On the high-level, this can be achieved with following steps:

1. Providing LLM:
   * overview of SQL database schema&#x20;
   * example rows data
2. Ask LLM to now returns a SQL query with few shot prompting
3. Validate the SQL query using [If Else](../integrations/utilities/if-else.md) node
4. Custom function to execute the SQL query, and get the response
5. Ask LLM to return a natural response from the executed SQL response

<figure><img src="../.gitbook/assets/image (113).png" alt=""><figcaption></figcaption></figure>

In this example, we are going to create a QnA chatbot that can interact with a SQL database stored in SingleStore

<figure><img src="../.gitbook/assets/image (116).png" alt=""><figcaption></figcaption></figure>

## 1) SQL Database Schema + Example Rows

Use a Custom JS Function node to connect to SingleStore, retrieve database schema and top 3 rows.

From the [research paper](https://arxiv.org/abs/2204.00498), it is recommended to generate a prompt with following example format:

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

<summary>Full Javascript Code</summary>

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

You can find more on how to get the `HOST`, `USER`, `PASSWORD` from this [guide](../integrations/langchain/vector-stores/singlestore.md). Once finished, click Execute:

<figure><img src="../.gitbook/assets/image (117).png" alt=""><figcaption></figcaption></figure>

We can now see the correct format has been generated. Next step is to bring this into Prompt Template.

## 2) Ask LLM to now returns a SQL query with few shot prompting

Create a new Chat Model + Prompt Template + LLMChain

<figure><img src="../.gitbook/assets/image (118).png" alt=""><figcaption></figcaption></figure>

```
Based on the provided SQL table schema and question below, return a SQL SELECT ALL query that would answer the user's question. For example: SELECT * FROM table WHERE id = '1'.
------------
SCHEMA: {schema}
------------
QUESTION: {question}
------------
SQL QUERY:
```

{% hint style="info" %}
You can provide more examples to the prompt (i.e few-shot prompting) to let the LLM learns better. Or take reference from [dialect-specific prompting](https://js.langchain.com/docs/use\_cases/sql/prompting#dialect-specific-prompting)
{% endhint %}

## 3) Validate the SQL query using [If Else](../integrations/utilities/if-else.md) node

Sometimes the SQL query is invalid, and we do not want to waste resources the execute invalid SQL query. For example, if user is asking general question that is irrelevant to the SQL database. We can use If Else node to route to different path.

For instance, we can perform a basic check to see if SELECT and WHERE are included in the SQL query given by LLM:

<figure><img src="../.gitbook/assets/image (119).png" alt="" width="327"><figcaption></figcaption></figure>

In the Else Function, we will route to a Prompt Template + LLMChain that basically tells LLM that it is unable to answer user query:

<figure><img src="../.gitbook/assets/image (120).png" alt=""><figcaption></figcaption></figure>
