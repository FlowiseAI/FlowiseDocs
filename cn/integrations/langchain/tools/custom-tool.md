# 自定义工具

观看如何使用自定义工具

{% embed url="https://youtu.be/HSp9LkkTVY0" %}

## 问题

函数通常接受结构化输入数据。假设您希望 LLM 能够调用 Airtable 创建记录 [API](https://airtable.com/developers/web/api/create-records)，主体参数必须以特定方式构建。例如：

```json
"records": [
  {
    "fields": {
      "Address": "some address",
      "Name": "some name",
      "Visited": true
    }
  }
]
```

理想情况下，我们希望 LLM 返回正确的结构化数据，如下所示：

```json
{
  "Address": "some address",
  "Name": "some name",
  "Visited": true
}
```

因此我们可以提取该值并将其解析到 API 所需的主体中。然而，指示 LLM 输出准确的模式很困难。

借助新的 [OpenAI 函数调用](https://openai.com/blog/function-calling-and-other-api-updates) 模型，现在可以实现这一点。 `gpt-4-0613` 和 `gpt-3.5-turbo-0613` 经过专门训练，可返回结构化数据。该模型将智能地选择输出一个 JSON 对象，其中包含调用这些函数的参数。

## 教程

**目标**：让代理自动获取股票价格变动，检索相关股票新闻，并向 Airtable 添加新记录。

让我们开始[🚀](https://emojipedia.org/rocket/)

### 创建工具

我们需要 3 个工具来实现目标：

* 获取股价变动
* 获取股票新闻
* 添加Airtable记录

#### 获取股价变动

使用以下详细信息创建一个新工具（您可以根据需要进行更改）：

* 名称：获取\_stock\_movers
* 描述：获取价格/成交量变动最大的股票，例如积极者、获益者、受损者等

描述是一个重要的部分，因为 ChatGPT 依靠它来决定何时使用该工具。

<figure><img src="../../../.gitbook/assets/image (6) (3).png" alt=""><figcaption></figcaption></figure>

* JavaScript 函数：我们将使用 [Morning Star](https://rapidapi.com/apidojo/api/morning-star) `/market/v2/get-movers` API 来获取数据。首先，您必须单击“订阅测试”（如果还没有），然后复制代码并将其粘贴到 JavaScript 函数中。
  * 在顶部添加 `const fetch = require('node-fetch');` 以导入库。您可以导入任何内置 NodeJS [modules](https://www.w3schools.com/nodejs/ref_modules.asp) 和 [外部库](https://github.com/FlowiseAI/Flowise/blob/main/packages/components/src/utils.ts#L289)。
  * 最后返回 `result` 。

<figure><img src="../../../.gitbook/assets/Untitled (4) (1).png" alt=""><figcaption></figcaption></figure>

最终代码应该是：

```javascript
const fetch = require('node-fetch');
const url = 'https://morning-star.p.rapidapi.com/market/v2/get-movers';
const options = {
	method: 'GET',
	headers: {
		'X-RapidAPI-Key': 'replace with your api key',
		'X-RapidAPI-Host': 'morning-star.p.rapidapi.com'
	}
};

try {
	const response = await fetch(url, options);
	const result = await response.text();
	console.log(result);
	return result;
} catch (error) {
	console.error(error);
	return '';
}
```

您现在可以保存它。

#### 获取股票新闻

使用以下详细信息创建一个新工具（您可以根据需要进行更改）：

* 名称: 获取\_stock\_news
* 描述：获取股票的最新消息
* 输入模式：
  * 属性：性能ID
  * 类型：字符串
  * 描述：股票的id，在API中称为performanceID
  * 必填：真实

输入架构告诉 LLM 作为 JSON 对象返回什么。在这种情况下，我们期待一个 JSON 对象，如下所示：

<pre class="language-json"><code class="lang-json"><strong>{ "performanceId": "SOME TICKER" }
</strong></code></pre>

<figure><img src="../../../.gitbook/assets/image (4) (2).png" alt=""><figcaption></figcaption></figure>

* JavaScript 函数：我们将使用 [Morning Star](https://rapidapi.com/apidojo/api/morning-star) `/news/list` API 来获取数据。首先，您必须单击“订阅测试”（如果还没有），然后复制代码并将其粘贴到 JavaScript 函数中。
  * 在顶部添加 `const fetch = require('node-fetch');` 以导入库。您可以导入任何内置 NodeJS [modules](https://www.w3schools.com/nodejs/ref_modules.asp) 和 [外部库](https://github.com/FlowiseAI/Flowise/blob/main/packages/components/src/utils.ts#L289)。
  * 最后返回 `result` 。
* 接下来，将硬编码的url查询参数performanceId：`0P0000OQN8`替换为Input Schema中指定的属性变量：`$performanceId`
* 您可以通过在变量名称前面附加前缀 `$` 来将输入架构中指定的任何属性用作 JavaScript 函数中的变量。

<figure><img src="../../../.gitbook/assets/Untitled (2) (1) (1).png" alt=""><figcaption></figcaption></figure>

最终代码：

```javascript
const fetch = require('node-fetch');
const url = 'https://morning-star.p.rapidapi.com/news/list?performanceId=' + $performanceId;
const options = {
	method: 'GET',
	headers: {
		'X-RapidAPI-Key': 'replace with your api key',
		'X-RapidAPI-Host': 'morning-star.p.rapidapi.com'
	}
};

try {
	const response = await fetch(url, options);
	const result = await response.text();
	console.log(result);
	return result;
} catch (error) {
	console.error(error);
	return '';
}
```

您现在可以保存它。

#### 添加Airtable记录

使用以下详细信息创建一个新工具（您可以根据需要进行更改）：

* 名称: add\_airtable
* 描述：将股票、新闻摘要和价格变动添加到 Airtable
* 输入模式：
  * 财产：库存
  * 类型：字符串
  * 描述：股票行情
  * 必填：真实
  * 属性：移动
  * 类型：字符串
  * 描述：价格变动百分比
  * 必填：真实
  * 属性：新闻\_摘要
  * 类型：字符串
  * 描述：该股新闻摘要
  * 必填：真实

ChatGPT 将返回一个 JSON 对象，如下所示：

```json
{ "stock": "SOME TICKER", "move": "20%", "news_summary": "Some summary" }
```

<figure><img src="../../../.gitbook/assets/image (36).png" alt=""><figcaption></figcaption></figure>

* JavaScript 函数：我们将使用 [Airtable Create Record API](https://airtable.com/developers/web/api/create-records) 在现有表中创建新记录。您可以从[此处](https://www.highviewapps.com/kb/where-can-i-find-the-airtable-base-id-and-table-id/)找到tableId和baseId。您还需要创建个人访问令牌，请在[此处](https://www.highviewapps.com/kb/how-do-i-create-an-airtable-personal-access-token/)了解操作方法。

最终代码应如下所示。请注意我们如何传入 `$stock`、`$move` 和 `$news_summary` 作为变量：

```javascript
const fetch = require('node-fetch');
const baseId = 'your-base-id';
const tableId = 'your-table-id';
const token = 'your-token';

const body = {
	"records": [
		{
			"fields": {
				"stock": $stock,
				"move": $move,
				"news_summary": $news_summary,
			}
		}
	]
};

const options = {
	method: 'POST',
	headers: {
		'Authorization': `Bearer ${token}`,
		'Content-Type': 'application/json'
	},
	body: JSON.stringify(body)
};

const url = `https://api.airtable.com/v0/${baseId}/${tableId}`

try {
	const response = await fetch(url, options);
	const text = await response.text();
	return text;
} catch (error) {
	console.error(error);
	return '';
}
```

您现在可以保存它。

您应该会看到创建了 3 个工具：

<figure><img src="../../../.gitbook/assets/image (3) (3) (1) (1).png" alt=""><figcaption></figcaption></figure>

### 创建聊天流

您可以使用市场上的 **OpenAI Function** **Agent** 模板，并将工具替换为 **自定义工具**。选择您创建的工具。

注：OpenAI Function Agent 目前仅支持 0613 型号。

<figure><img src="../../../.gitbook/assets/image (15) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

保存聊天流并开始测试。对于初学者，您可以尝试询问：

_<mark style="color:blue;">今天价格变动最大的股票是什么？</mark>_

_<mark style="color:orange;">今天价格变动最大的股票是 Overstock.com (OSTK)，价格变动为 17.47%。</mark>_

然后，您可以跟进另一个问题来获取该特定股票的新闻：

_<mark style="color:blue;">该股票的最新消息有哪些可能导致价格变动？</mark>_

_<mark style="color:orange;">以下是有关 Overstock.com (OSTK) 的可能导致价格变动的最新消息：</mark>_

1._<mark style="color:orange;">标题：“Overstock 股价飙升，成功以 2150 万美元竞购 Bed Bath & Beyond 资产” 来源：MarketWatch 发布日期：2023 年 6 月 22 日 摘要：成功竞购价值 2150 万美元的 Bed Bath & Beyond 资产后，Overstock.com 股价大幅上涨。</mark>_
2._<mark style="color:orange;">标题：“考虑在 Meta Platforms、Overstock.com、沃尔玛、家得宝或联合包裹服务公司中交易期权或股票吗？”来源：美通社 发布日期：2023 年 6 月 22 日 摘要：这篇新闻文章讨论了投资者可能考虑的潜在交易选项和股票，包括 Overstock.com。</mark>_

_<mark style="color:orange;">请注意，这些新闻文章仅供参考，可能不是价格变动的唯一原因。始终建议在做出任何投资决定之前进行彻底的研究和分析。</mark>_

最后，您可以要求 ChatGPT 将新记录添加到 Airtable：

_<mark style="color:blue;">您可以向 Airtable 添加包含股票行情、价格变动和新闻摘要的记录吗？</mark>_

_<mark style="color:orange;">我已向 Airtable 添加了一条记录，其中包含以下详细信息：</mark>_

_<mark style="color:orange;">股票代码：OSTK 价格变动：17.47% 新闻摘要：Overstock.com 成功竞购 Bed Bath & Beyond 价值 2150 万美元资产后，股价大幅上涨。</mark>_

[🎉](https://emojipedia.org/party-popper/)[🎉](https://emojipedia.org/party-popper/)**瞧！** 这样您就可以创建自己的自定义工具并将其与 OpenAI Function Agent 一起使用！

## 附加

### 将会话 ID 传递给函数

默认情况下，自定义工具中的 Function 可以访问以下流配置：

```json5
$flow.sessionId 
$flow.chatId
$flow.chatflowId
$flow.input
```

以下是将 sessionId 发送到 Discord webhook 的示例：

{% tabs %}
{% tab title="Javascript" %}
```javascript
const fetch = require('node-fetch');
const webhookUrl = "https://discord.com/api/webhooks/1124783587267";
const content = $content; // captured from input schema
const sessionId = $flow.sessionId;

const body = {
	"content": `${mycontent} and the sessionid is ${sessionId}`
};

const options = {
	method: 'POST',
	headers: {
		'Content-Type': 'application/json'
	},
	body: JSON.stringify(body)
};

const url = `${webhookUrl}?wait=true`

try {
	const response = await fetch(url, options);
	const text = await response.text();
	return text;
} catch (error) {
	console.error(error);
	return '';
}
```
{% endtab %}
{% endtabs %}

### 将变量传递给函数

在某些情况下，您希望将变量传递给自定义工具函数。

例如，您正在创建一个使用自定义工具的聊天机器人。自定义工具正在执行 HTTP POST 调用，成功验证请求需要 API 密钥。您可以将其作为变量传递。

默认情况下，自定义工具中的函数可以访问变量：

```
$vars.<variable-name>
```

如何使用 API 和嵌入式在 Flowise 中传递变量的示例：

{% tabs %}
{% tab title="Javascript API" %}
```javascript
async function query(data) {
    const response = await fetch(
        "http://localhost:3000/api/v1/prediction/<chatflow-id>",
        {
            method: "POST",
            headers: {
                "Content-Type": "application/json"
            },
            body: JSON.stringify(data)
        }
    );
    const result = await response.json();
    return result;
}

query({
    "question": "Hey, how are you?",
    "overrideConfig": {
        "vars": {
            "apiKey": "abc"
        }
    }
}).then((response) => {
    console.log(response);
});
```
{% endtab %}

{% tab title="Embed" %}
```html
<script type="module">
    import Chatbot from 'https://cdn.jsdelivr.net/npm/flowise-embed/dist/web.js';
    Chatbot.init({
        chatflowid: 'chatflow-id',
        apiHost: 'http://localhost:3000',
        chatflowConfig: {
          vars: {
            apiKey: 'def'
          }
        }
    });
</script>
```
{% endtab %}
{% endtabs %}

如何在自定义工具中接收变量的示例：

{% tabs %}
{% tab title="Javascript" %}
```javascript
const fetch = require('node-fetch');
const webhookUrl = "https://discord.com/api/webhooks/1124783587267";
const content = $content; // captured from input schema
const sessionId = $flow.sessionId;
const apiKey = $vars.apiKey;

const body = {
	"content": `${mycontent} and the sessionid is ${sessionId}`
};

const options = {
	method: 'POST',
	headers: {
		'Content-Type': 'application/json',
		'Authorization': `Bearer ${apiKey}`
	},
	body: JSON.stringify(body)
};

const url = `${webhookUrl}?wait=true`

try {
	const response = await fetch(url, options);
	const text = await response.text();
	return text;
} catch (error) {
	console.error(error);
	return '';
}
```
{% endtab %}
{% endtabs %}

### 覆盖自定义工具

以下参数可以被覆盖

|参数|描述 |
| ---------------- | ---------------- |
|自定义工具名称 |工具名称 |
|自定义工具描述 |工具说明|
|自定义工具架构 |工具架构|
|自定义工具函数 |工具功能|

用于覆盖自定义工具参数的 API 调用示例：

{% tabs %}
{% tab title="Javascript API" %}
```javascript
async function query(data) {
    const response = await fetch(
        "http://localhost:3000/api/v1/prediction/<chatflow-id>",
        {
            method: "POST",
            headers: {
                "Content-Type": "application/json"
            },
            body: JSON.stringify(data)
        }
    );
    const result = await response.json();
    return result;
}

query({
    "question": "Hey, how are you?",
    "overrideConfig": {
        "customToolName": "example_tool",
        "customToolSchema": "z.object({title: z.string()})"
    }
}).then((response) => {
    console.log(response);
});
```
{% endtab %}
{% endtabs %}

### 导入外部依赖

您可以将任何内置 NodeJS [modules](https://www.w3schools.com/nodejs/ref_modules.asp) 和支持的 [外部库](https://github.com/FlowiseAI/Flowise/blob/main/packages/components/src/utils.ts#L289) 导入到 Function 中。

1. 要导入任何不支持的库，您可以轻松地将新的 npm 包添加到 `packages/components` 文件夹中的 `package.json` 中。

```bash
cd Flowise && cd packages && cd components
pnpm add <your-library>
cd .. && cd ..
pnpm install
pnpm build
```

2. 然后，将导入的库添加到 `TOOL_FUNCTION_EXTERNAL_DEP` 环境变量中。有关更多详细信息，请参阅[#内置和外部依赖关系](../../../configuration/environment-variables.md#builtin-and-external-dependencies "mention")。
3.启动应用程序

```bash
pnpm start
```

4. 然后，您可以在 **JavaScript Function** 中使用新添加的库，如下所示：

```javascript
const axios = require('axios')
```

观看如何添加额外的依赖项和导入库

{% embed url="https://youtu.be/0H1rrisc0ok" %}
