# SQL Agent

This tutorial will guide you through building an intelligent SQL Agent that can interact with databases, generate SQL queries, validate them, execute them, and self-correct when errors occur.

## Overview

The SQL Agent flow implements a robust database interaction system that:

1. Retrieves database schema information
2. Generates SQL queries based on user questions
3. Validates generated queries for common mistakes
4. Executes queries against the database
5. Checks results for errors and self-corrects when needed
6. Provides natural language responses based on query results

<figure><img src="../.gitbook/assets/image (5) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

### Step 1: Setting Up the Start Node

Begin by adding a **Start** node to your canvas. This serves as the entry point for your SQL agent.

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

#### Configuration:

* **Input Type**: Select "Chat Input" to accept user questions
* **Flow State**: Add a state variable with key "`sqlQuery`" and empty value

The Start node initializes the flow state with an empty `sqlQuery` variable that will store the generated SQL query throughout the process.

### Step 2: Retrieving Database Schema

Add a **Custom Function** node and connect it to the Start node.

<figure><img src="../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

#### Configuration:

* **Javascript Function**: This is an example function that connects to your database and retrieves the complete schema including table structures, column definitions, and sample data.

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

### Step 3: Generating SQL Queries

Add an **LLM** node connected to the "Get DB Schema" node.

<figure><img src="../.gitbook/assets/image (4) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

#### Configuration:

* **Messages**: Add a system message:

```
You are an agent designed to interact with a SQL database. Given an input question, create a syntactically correct sqlite query to run, then look at the results of the query and return the answer. Unless the user specifies a specific number of examples they wish to obtain, always limit your query to at most 5 results. You can order the results by a relevant column to return the most interesting examples in the database. Never query for all the columns from a specific table, only ask for the relevant columns given the question. DO NOT make any DML statements (INSERT, UPDATE, DELETE, DROP etc.) to the database.

Here is the relevant table info:
{{ customFunctionAgentflow_0 }}

Note:
- Only generate ONE SQL query
```

* **JSON Structured Output**: Here we instruct the model only return structured output, to prevent LLM from including other text other than the SQL query.
  * Key: "`sql_query`"
  * Type: "string"
  * Description: "SQL query"
* **Update Flow State**: Set key "`sqlQuery`" with value `{{ output.sql_query }}`

This node transforms the user's natural language question into a structured SQL query using the database schema information.

### Step 4: Validating SQL Query Syntax

Add a **Condition Agent** node connected to the "Generate SQL Query" LLM.

<figure><img src="../.gitbook/assets/image (5) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

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

* **Input**: `{{ $flow.state.sqlQuery }}`
* **Scenarios**:
  * Scenario 1: "SQL query is correct and does not contains mistakes"
  * Scenario 2: "SQL query contains mistakes"

This validation step catches common SQL errors before execution.

### Step 5: Handling Query Regeneration (Error Path)

For incorrect queries (output 1) from previous Condition Agent node, add a **Loop** node.

<figure><img src="../.gitbook/assets/image (6) (1) (1) (1) (1) (1).png" alt="" width="375"><figcaption></figcaption></figure>

#### Configuration:

<figure><img src="../.gitbook/assets/image (7) (1) (1) (1) (1) (1).png" alt="" width="526"><figcaption></figcaption></figure>

* **Loop Back To**: "Generate SQL Query"
* **Max Loop Count**: Set to 5

This creates a feedback loop that allows the system to retry query generation when validation fails.

### Step 6: Executing Valid SQL Queries

For correct queries (output 0), add a **Custom Function** node.

<figure><img src="../.gitbook/assets/image (8) (1) (1) (1) (1).png" alt="" width="375"><figcaption></figcaption></figure>

#### Configuration:

<figure><img src="../.gitbook/assets/image (9) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

* **Input Variables**: Here we pass in the generated SQL query as variable to be used in Function.
  * Variable Name: "sqlQuery"
  * Variable Value: `{{ $flow.state.sqlQuery }}`
* **Javascript Function**: This function executes the validated SQL query against the database and formats the results.

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

### Step 7: Checking Query Execution Results

Add a **Condition Agent** node connected to the "Run SQL Query" function.

<figure><img src="../.gitbook/assets/image (10) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

#### Configuration:

* **Instructions**: "You are a SQL expert. Check if the query result is correct or contains error."
* **Input**: `{{ customFunctionAgentflow_1 }}`
* **Scenarios**:
  * Scenario 1: "Result is correct and does not contains error"
  * Scenario 2: "Result query contains error"

This step validates the execution results and determines if further correction is needed.

### Step 8: Generating Final Response (Success Path)

For successful results (output 0 from Condition Agent), add an **LLM** node.

<figure><img src="../.gitbook/assets/image (11) (1) (1) (1).png" alt="" width="375"><figcaption></figcaption></figure>

#### Configuration:

* **Input Message**: `{{ customFunctionAgentflow_1 }}`

This node generates a natural language response based on the successful query results.

### Step 9: Handling Query Regeneration (Runtime Error Path)

For failed executions (output 1 from Condition Agent), add an **LLM** node.

<figure><img src="../.gitbook/assets/image (12) (1) (1) (1).png" alt="" width="375"><figcaption></figcaption></figure>

#### Configuration:

<figure><img src="../.gitbook/assets/image (13) (1) (1) (1).png" alt="" width="399"><figcaption></figcaption></figure>

* **Messages**: Add the same system message as Step 3
* **Input Message**:

```
Given the generated SQL Query: {{ $flow.state.sqlQuery }}
I have the following error: {{ customFunctionAgentflow_1 }}
Regenerate a new SQL Query that will fix the error
```

* **JSON Structured Output**: Same as Step 3
* **Update Flow State**: Set key "`sqlQuery`" with value `{{ output.sql_query }}`

This node analyzes runtime errors and generates corrected SQL queries.

### Step 10: Adding the Second Loop Back

Add a **Loop** node connected to the "Regenerate SQL Query" LLM.

<figure><img src="../.gitbook/assets/image (14) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

#### Configuration:

* **Loop Back To**: "Check SQL Query"
* **Max Loop Count**: Set to 5

This creates a second feedback loop for runtime error correction.

***

## Complete Flow Structure

{% file src="../.gitbook/assets/SQL Agent.json" %}

***

## Summary

1. Start → Get DB Schema
2. Get DB Schema → Generate SQL Query
3. Generate SQL Query → Check SQL Query
4. Check SQL Query (Correct) → Run SQL Query
5. Check SQL Query (Incorrect) → Regenerate Query (Loop back)
6. Run SQL Query → Check Result
7. Check Result (Success) → Return Response
8. Check Result (Error) → Regenerate SQL Query
9. Regenerate SQL Query → Recheck SQL Query (Loop back)

***

## Testing Your SQL Agent

Test your agent with various types of database questions:

* Simple queries: "Show me all customers"
* Complex queries: "What are the top 5 products by sales?"
* Analytical queries: "Calculate the average order value by month"

<figure><img src="../.gitbook/assets/image (15) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

This SQL Agent flow provides a robust, self-correcting system for database interactions that can handle SQL queries in natural language.
