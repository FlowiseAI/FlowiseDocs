# SQL 代理

本教程将指导您构建一个智能 SQL 代理，该代理可以与数据库交互、生成 SQL 查询、验证它们、执行它们，并在发生错误时进行自我纠正。

## 概述

SQL 智能体流程实现了强大的数据库交互系统，该系统：

1. 检索数据库模式信息
2.根据用户问题生成SQL查询
3. 验证生成的查询是否存在常见错误
4. 对数据库执行查询
5. 检查结果是否有错误并在需要时进行自我更正
6.根据查询结果提供自然语言响应

<figure><img src="../.gitbook/assets/image (5) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

### 第 1 步：设置起始节点

首先向画布添加 **Start** 节点。这是您的 SQL 代理的入口点。

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

#### 配置：

* **输入类型**：选择“聊天输入”接受用户问题
* **流状态**：添加一个带有键“`sqlQuery`”和空值的状态变量

Start 节点使用空的 `sqlQuery` 变量初始化流状态，该变量将在整个过程中存储生成的 SQL 查询。

### 步骤 2：检索数据库架构

添加 **Custom Function** 节点并将其连接到 Start 节点。

<figure><img src="../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

#### 配置：

* **Javascript 函数**：这是一个示例函数，用于连接到数据库并检索完整的架构，包括表结构、列定义和示例数据。

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

### 步骤 3：生成 SQL 查询

添加连接到“获取数据库架构”节点的 **LLM** 节点。

<figure><img src="../.gitbook/assets/image (4) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

#### 配置：

* **消息**：添加系统消息：

```
You are an agent designed to interact with a SQL database. Given an input question, create a syntactically correct sqlite query to run, then look at the results of the query and return the answer. Unless the user specifies a specific number of examples they wish to obtain, always limit your query to at most 5 results. You can order the results by a relevant column to return the most interesting examples in the database. Never query for all the columns from a specific table, only ask for the relevant columns given the question. DO NOT make any DML statements (INSERT, UPDATE, DELETE, DROP etc.) to the database.

Here is the relevant table info:
{{ customFunctionAgentflow_0 }}

Note:
- Only generate ONE SQL query
```

* **JSON 结构化输出**：这里我们指示模型仅返回结构化输出，以防止 LLM 包含除 SQL 查询之外的其他文本。
  * 键：“`sql_query`”
  * 类型：“字符串”
  * 描述：“SQL查询”
* **更新流状态**：将键“`sqlQuery`”设置为值 `{{ output.sql_query }}`

该节点使用数据库架构信息将用户的自然语言问题转换为结构化的 SQL 查询。

### 步骤 4：验证 SQL 查询语法

添加连接到“生成 SQL 查询”LLM 的 **条件代理** 节点。

<figure><img src="../.gitbook/assets/image (5) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

#### 配置：

* **说明**：

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

* **输入**：`{{ $flow.state.sqlQuery }}`
* **场景**：
  * 场景1：“SQL查询正确且不包含错误”
  * 场景 2：“SQL 查询包含错误”

此验证步骤在执行前捕获常见的 SQL 错误。

### 步骤 5：处理查询重新生成（错误路径）

对于来自先前条件代理节点的错误查询（输出 1），请添加 **Loop** 节点。

<figure><img src="../.gitbook/assets/image (6) (1) (1) (1) (1) (1).png" alt="" width="375"><figcaption></figcaption></figure>

#### 配置：

<figure><img src="../.gitbook/assets/image (7) (1) (1) (1) (1) (1).png" alt="" width="526"><figcaption></figcaption></figure>

* **循环回到**：“生成 SQL 查询”
* **最大循环计数**：设置为 5

这会创建一个反馈循环，允许系统在验证失败时重试查询生成。

### 步骤 6：执行有效的 SQL 查询

为了正确查询（输出 0），请添加 **自定义函数** 节点。

<figure><img src="../.gitbook/assets/image (8) (1) (1) (1) (1).png" alt="" width="375"><figcaption></figcaption></figure>

#### 配置：

<figure><img src="../.gitbook/assets/image (9) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

* **输入变量**：这里我们传入生成的 SQL 查询作为要在函数中使用的变量。
  * 变量名称：“sqlQuery”
  * 变量值：`{{ $flow.state.sqlQuery }}`
* **Javascript 函数**：此函数对数据库执行经过验证的 SQL 查询并格式化结果。

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

### 步骤7：检查查询执行结果

添加连接到“运行 SQL 查询”功能的 **条件代理** 节点。

<figure><img src="../.gitbook/assets/image (10) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

#### 配置：

* **说明**：“您是SQL专家。请检查查询结果是否正确或包含错误。”
* **输入**：`{{ customFunctionAgentflow_1 }}`
* **场景**：
  * 场景1：“结果正确且不包含错误”
  * 场景2：“结果查询包含错误”

此步骤验证执行结果并确定是否需要进一步修正。

### 步骤 8：生成最终响应（成功路径）

要获得成功结果（条件代理输出 0），请添加 **LLM** 节点。

<figure><img src="../.gitbook/assets/image (11) (1) (1) (1).png" alt="" width="375"><figcaption></figcaption></figure>

#### 配置：

* **输入消息**：`{{ customFunctionAgentflow_1 }}`

该节点根据成功的查询结果生成自然语言响应。

### 步骤 9：处理查询重新生成（运行时错误路径）

对于失败的执行（条件代理的输出 1），添加 **LLM** 节点。

<figure><img src="../.gitbook/assets/image (12) (1) (1) (1).png" alt="" width="375"><figcaption></figcaption></figure>

#### 配置：

<figure><img src="../.gitbook/assets/image (13) (1) (1) (1).png" alt="" width="399"><figcaption></figcaption></figure>

* **消息**：添加与步骤3相同的系统消息
* **输入消息**：

```
Given the generated SQL Query: {{ $flow.state.sqlQuery }}
I have the following error: {{ customFunctionAgentflow_1 }}
Regenerate a new SQL Query that will fix the error
```

* **JSON 结构化输出**：与步骤 3 相同
* **更新流状态**：将键“`sqlQuery`”设置为值 `{{ output.sql_query }}`

该节点分析运行时错误并生成更正的 SQL 查询。

### 步骤 10：添加第二个循环

添加连接到“重新生成 SQL 查询”LLM 的 **Loop** 节点。

<figure><img src="../.gitbook/assets/image (14) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

#### 配置：

* **循环回到**：“检查SQL查询”
* **最大循环计数**：设置为 5

这创建了第二个反馈循环，用于运行时纠错。

***

## 完整的流程结构

{% file src="../.gitbook/assets/SQL Agent.json" %}

***

## 总结

1. 开始 → 获取数据库架构
2. 获取数据库架构 → 生成 SQL 查询
3. 生成 SQL 查询 → 检查 SQL 查询
4. 检查 SQL 查询（正确）→ 运行 SQL 查询
5.检查SQL查询（错误）→重新生成查询（环回）
6. 运行SQL查询→检查结果
7. 检查结果（成功）→返回响应
8. 检查结果（错误）→重新生成SQL查询
9. 重新生成 SQL 查询 → 重新检查 SQL 查询（环回）

***

## 测试您的 SQL 代理

使用各种类型的数据库问题测试您的代理：

* 简单查询：“显示所有客户”
* 复杂查询：“销量排名前 5 名的产品是什么？”
* 分析查询：“按月计算平均订单价值”

<figure><img src="../.gitbook/assets/image (15) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

此 SQL 智能体流程为数据库交互提供了一个强大的、自我纠正的系统，可以处理自然语言的 SQL 查询。
