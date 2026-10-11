---
description: 了解如何查询结构化数据
---

# SQL QnA

***

与前面的示例（例如 [Web Scrape QnA](web-scrape-qna.md) 和 [多文档 QnA](multiple-documents-qna.md)）不同，查询结构化数据不需要矢量数据库。在高层，这可以通过以下步骤来实现：

1. 提供 LLM：
   * SQL 数据库模式概述
   * 示例行数据
2. 返回 SQL 查询，并带有很少的镜头提示词
3. 使用 [If Else](../integrations/utilities/if-else.md) 节点验证 SQL 查询
4. 创建自定义函数来执行 SQL 查询，并获取响应
5. 从执行的 SQL 响应返回自然响应

<figure><img src="../.gitbook/assets/image (113).png" alt=""><figcaption></figcaption></figure>

在此示例中，我们将创建一个 QnA 聊天机器人，它可以与存储在 SingleStore 中的 SQL 数据库进行交互

<figure><img src="../.gitbook/assets/image (116).png" alt=""><figcaption></figcaption></figure>

## 长篇大论；博士

您可以找到聊天流模板：

{% file src="../.gitbook/assets/SQL Chatflow.json" %}

## 1. SQL 数据库架构 + 示例行

使用自定义 JS 函数节点连接到 SingleStore，检索数据库架构和前 3 行。

根据[研究论文](https://arxiv.org/abs/2204.00498)，建议生成具有以下示例格式的提示词：

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

<summary>完整的 JavaScript 代码</summary>

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

您可以从此[指南](broken-reference/)中找到有关如何获取`HOST`、`USER`、`PASSWORD`的更多信息。完成后，点击执行：

<figure><img src="../.gitbook/assets/image (117).png" alt=""><figcaption></figcaption></figure>

我们现在可以看到已经生成了正确的格式。下一步是将其带入提示词模板。

## 2. 返回一个带有很少镜头提示词的 SQL 查询

创建新的聊天模型+提示词模板+LLMChain

<figure><img src="../.gitbook/assets/image (118).png" alt=""><figcaption></figcaption></figure>

在提示词模板中指定以下提示词：

```
Based on the provided SQL table schema and question below, return a SQL SELECT ALL query that would answer the user's question. For example: SELECT * FROM table WHERE id = '1'.
------------
SCHEMA: {schema}
------------
QUESTION: {question}
------------
SQL QUERY:
```

由于我们使用 2 个变量：{schema} 和 {question}，请在 **Format Prompt Values** 中指定它们的值：

<figure><img src="../.gitbook/assets/image (122).png" alt="" width="563"><figcaption></figcaption></figure>

{% hint style="info" %}
您可以为提示词提供更多示例（即少量提示词），让 LLM 学得更好。或者参考[方言特定提示词](https://js.langchain.com/docs/use\_cases/sql/prompting#dialect-specific-prompting)
{% endhint %}

## 3. 使用 [If Else](../integrations/utilities/if-else.md) 节点验证 SQL 查询

有时 SQL 查询无效，我们不想浪费资源来执行无效的 SQL 查询。例如，如果用户询问与 SQL 数据库无关的一般性问题。我们可以使用 `If Else` 节点来路由到不同的路径。

例如，我们可以执行基本检查以查看 SELECT 和 WHERE 是否包含在 LLM 给出的 SQL 查询中。

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

在 Else 函数中，我们将路由到提示词模板 + LLMChain，它基本上告诉 LLM 它无法回答用户查询：

<figure><img src="../.gitbook/assets/image (120).png" alt=""><figcaption></figcaption></figure>

## 4. 自定义函数执行 SQL 查询，并获取响应

如果它是有效的 SQL 查询，我们需要执行该查询。将 **If Else** 节点的 _**True**_ 输出连接到 **Custom JS Function** 节点：

<figure><img src="../.gitbook/assets/image (123).png" alt="" width="563"><figcaption></figcaption></figure>

<details>

<summary>完整的 JavaScript 代码</summary>

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

## 5. 从执行的 SQL 响应返回自然响应

创建新的聊天模型+提示词模板+LLMChain

<figure><img src="../.gitbook/assets/image (124).png" alt=""><figcaption></figcaption></figure>

在提示词模板中写入以下提示词：

```
Based on the question, and SQL response, write a natural language response, be details as possible:
------------
QUESTION: {question}
------------
SQL RESPONSE: {sqlResponse}
------------
NATURAL LANGUAGE RESPONSE:
```

在**格式提示词值**中指定变量：

<figure><img src="../.gitbook/assets/image (125).png" alt="" width="563"><figcaption></figcaption></figure>

瞧！您的 SQL 聊天机器人现已准备好进行测试！

## 查询

首先我们先问一些与数据库相关的事情。

<figure><img src="../.gitbook/assets/image (128).png" alt="" width="434"><figcaption></figcaption></figure>

查看日志，我们可以看到第一个 LLMChain 能够为我们提供 SQL 查询：

**输入：**

{% code overflow="wrap" %}
```
Based on the provided SQL table schema and question below, return a SQL SELECT ALL query that would answer the user's question. For example: SELECT * FROM table WHERE id = '1'.\n------------\nSCHEMA: CREATE TABLE samples (id bigint(20) NOT NULL, firstName varchar(300) NOT NULL, lastName varchar(300) NOT NULL, userAddress varchar(300) NOT NULL, userState varchar(300) NOT NULL, userCode varchar(300) NOT NULL, userPostal varchar(300) NOT NULL, createdate timestamp(6) NOT NULL)\nSELECT * FROM samples LIMIT 3\nid firstName lastName userAddress userState userCode userPostal createdate\n1125899906842627 Steven Repici 14 Kingston St. Oregon NJ 5578 Thu Dec 14 2023 13:06:17 GMT+0800 (Singapore Standard Time)\n1125899906842625 John Doe 120 jefferson st. Riverside NJ 8075 Thu Dec 14 2023 13:04:32 GMT+0800 (Singapore Standard Time)\n1125899906842629 Bert Jet 9th, at Terrace plc Desert City CO 8576 Thu Dec 14 2023 13:07:11 GMT+0800 (Singapore Standard Time)\n------------\nQUESTION: what is the address of John\n------------\nSQL QUERY:
```
{% endcode %}

**输出**

<pre class="language-sql"><code class="lang-sql"><strong>SELECT userAddress FROM samples WHERE firstName = 'John'
</strong></code></pre>

执行 SQL 查询后，结果被传递到第二个 LLMChain：

**输入**

{% code overflow="wrap" %}
```
Based on the question, and SQL response, write a natural language response, be details as possible:\n------------\nQUESTION: what is the address of John\n------------\nSQL RESPONSE: [{\"userAddress\":\"120 jefferson st.\"}]\n------------\nNATURAL LANGUAGE RESPONSE:
```
{% endcode %}

**输出**

```
The address of John is 120 Jefferson St.
```

现在，如果我们询问与 SQL 数据库无关的问题，则采用 Else 路线。

<figure><img src="../.gitbook/assets/image (132).png" alt="" width="428"><figcaption></figcaption></figure>

对于第一个 LLMChain，生成 SQL 查询，如下所示：

```sql
SELECT * FROM samples LIMIT 3
```

但是，它未通过 `If Else` 检查，因为它不包含 `SELECT` 和 `WHERE`，因此进入 Else 路由，提示词如下：

```
Politely say "I'm not able to answer query"
```

最终的输出是：

```
I apologize, but I'm not able to answer your query at the moment.
```

## 结论

在此示例中，我们成功创建了一个 SQL 聊天机器人，它可以与您的数据库交互，并且还能够处理与数据库无关的问题。进一步的改进包括添加内存以提供对话历史记录。

您可以在下面找到聊天流：

{% file src="../.gitbook/assets/SQL Chatflow (1).json" %}
