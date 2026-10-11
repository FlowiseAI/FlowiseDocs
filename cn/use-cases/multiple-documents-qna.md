---
description: 了解如何正确查询多个文档
---

# 多个文档 QnA

***

从上一个 [Web Scrape QnA](web-scrape-qna.md) 示例中，我们仅写入更新和查询 1 个网站。如果我们有多个网站或多个文档怎么办？让我们看一下如何实现这一目标。

在此示例中，我们将对 2 个 PDF 执行 QnA，即 APPLE 和 TESLA 的 FORM-10K。

<div align="left" data-full-width="false"><figure><img src="../.gitbook/assets/image (93).png" alt="" width="375"><figcaption></figcaption></figure> <figure><img src="../.gitbook/assets/image (94).png" alt="" width="375"><figcaption></figcaption></figure></div>

## 写入更新

1. 从市场模板中找到名为 - **Conversational Retrieval QA Chain** 的示例流程。
2. 我们将使用[PDF 文件加载器](../integrations/langchain/document-loaders/pdf-file.md)，并上传相应的文件：

<figure><img src="../.gitbook/assets/multi-docs-upload.png" alt=""><figcaption></figcaption></figure>

3. 单击PDF File Loader 的**附加参数**，并指定元数据对象。例如，上传 Apple FORM-10K 的 PDF 文件可以具有元数据对象 `{source: apple}` ，而上传 Tesla FORM-10K 的 PDF 文件可以具有 `{source: tesla}` 。这样做是为了在检索期间隔离文档。

<div align="left"><figure><img src="../.gitbook/assets/multi-docs-apple.png" alt="" width="563"><figcaption></figcaption></figure> <figure><img src="../.gitbook/assets/multi-docs-tesla.png" alt="" width="563"><figcaption></figcaption></figure></div>

4. 填写 Pinecone 的凭据后，单击 Upsert：

<figure><img src="../.gitbook/assets/multi-docs-upsert.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (98).png" alt=""><figcaption></figcaption></figure>

5. 在[Pinecone 控制台](https://app.pinecone.io) 上，您将能够看到添加的新向量。

<figure><img src="../.gitbook/assets/multi-docs-console.png" alt=""><figcaption></figcaption></figure>

## 查询

1. 验证数据已更新到 Pinecone 后，我们现在可以开始在聊天中提问！

<figure><img src="../.gitbook/assets/image (100).png" alt=""><figcaption></figcaption></figure>

2. 但是，用于返回答案的检索上下文是 APPLE 和 TESLA 文档的混合。从源文件中可以看出：

<div align="left"><figure><img src="../.gitbook/assets/Untitled (7).png" alt="" width="563"><figcaption></figcaption></figure> <figure><img src="../.gitbook/assets/Untitled (8).png" alt="" width="563"><figcaption></figcaption></figure></div>

3. 我们可以通过从 Pinecone 节点指定元数据过滤器来解决此问题。例如，如果我们只想从 APPLE FORM-10K 检索上下文，我们可以回顾一下之前在 [#upsert](multiple-documents-qna.md#upsert "mention") 步骤中指定的元数据，然后在下面的元数据过滤器中使用相同的元数据：

<figure><img src="../.gitbook/assets/image (102).png" alt=""><figcaption></figcaption></figure>

4. 让我们再次问同样的问题，我们现在应该看到检索到的所有上下文确实来自 APPLE FORM-10K：

<figure><img src="../.gitbook/assets/image (103).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
每个矢量数据库提供商都有不同格式的过滤语法，建议阅读各自的矢量数据库文档
{% endhint %}

5. 然而，这样做的问题是元数据过滤有点_**“硬编码”**_。理想情况下，我们应该让 LLM 根据问题决定检索哪个文档。

## 工具代理

我们可以使用[工具代理](../integrations/langchain/agents/tool-agent.md)解决_**“硬编码”**_元数据过滤器问题。

通过向智能体提供工具，我们可以让智能体根据问题决定适合使用哪种工具。

1. 使用以下名称和说明创建 [检索器 工具](../integrations/langchain/tools/retriever-tool.md)：

<table><thead><tr><th width="178">名称</th><th>描述</th></tr></thead><tbody><tr><td>搜索_苹果</td><td>使用此功能回答用户有关 Apple Inc (APPL) 的问题。它包含一份 SEC 10K 表格文件，描述了 Apple Inc (APPL) 2022 年期间的财务状况。</td></tr></tbody></table>

2. 使用元数据过滤器 `{source: apple}` 连接到 Pinecone 节点

<figure><img src="../.gitbook/assets/image (104).png" alt="" width="563"><figcaption></figcaption></figure>

3. 对 Tesla 重复相同的操作：

<table><thead><tr><th width="175">名称</th><th width="322">描述</th><th>松果元数据过滤器</th></tr></thead><tbody><tr><td>搜索_tsla</td><td>使用此函数回答用户有关 Tesla Inc (TSLA) 的问题。它包含一份 SEC 10K 表格文件，描述特斯拉公司 (TSLA) 2022 年期间的财务状况。</td><td><code>{source: tesla}</code></td></tr></tbody></table>

{% hint style="info" %}
指定清晰简洁的描述非常重要。这允许 LLM 更好地决定何时使用哪个工具
{% endhint %}

您的流程应如下所示：

<figure><img src="../.gitbook/assets/image (154).png" alt=""><figcaption></figcaption></figure>

4. 现在，我们需要为工具代理创建一个通用指令。单击节点的“**其他参数**”，并指定**系统消息**。例如：

```
You are an expert financial analyst that always answers questions with the most relevant information using the tools at your disposal.
These tools have information regarding companies that the user has expressed interest in.
Here are some guidelines that you must follow:
* For financial questions, you must use the tools to find the answer and then write a response.
* Even if it seems like your tools won't be able to answer the question, you must still use them to find the most relevant information and insights. Not using them will appear as if you are not doing your job.
* You may assume that the users financial questions are related to the documents they've selected.
* For any user message that isn't related to financial analysis, respectfully decline to respond and suggest that the user ask a relevant question.
* If your tools are unable to find an answer, you should say that you haven't found an answer but still relay any useful information the tools found.
* Dont ask clarifying questions, just return answer.

The tools at your disposal have access to the following SEC documents that the user has selected to discuss with you:
- Apple Inc (APPL) FORM 10K 2022
- Tesla Inc (TSLA) FORM 10K 2022

The current date is: 2024-01-28
```

5. 保存聊天流，然后开始提问！

<figure><img src="../.gitbook/assets/image (110).png" alt=""><figcaption></figcaption></figure>

<div align="left"><figure><img src="../.gitbook/assets/Untitled (9).png" alt="" width="375"><figcaption></figcaption></figure> <figure><img src="../.gitbook/assets/Untitled (10).png" alt="" width="375"><figcaption></figcaption></figure></div>

6. 跟进特斯拉：

<figure><img src="../.gitbook/assets/image (111).png" alt=""><figcaption></figcaption></figure>

7. 现在，我们可以询问有关我们之前写入更新到矢量数据库的任何文档的问题，而无需使用工具+代理对元数据过滤进行“硬编码”。

## 元数据检索器

使用工具代理方法，用户必须创建多个检索器工具来从不同来源检索文档。如果存在大量具有不同元数据的文档源，这可能会成为问题。仅使用上面的示例（仅针对苹果和特斯拉），我们可能会扩展到其他公司，例如迪士尼、亚马逊等。为每家公司创建一个检索工具将是一项繁琐的任务。

元数据检索器开始发挥作用。这个想法是让 LLM 从用户问题中提取元数据，然后在搜索矢量数据库时将其用作过滤器。

例如，如果用户询问与 Apple 相关的问题，元数据过滤器 `{source: apple}` 将自动应用于矢量数据库搜索。

<div align="left"><figure><img src="../.gitbook/assets/image (235).png" alt="" width="297"><figcaption></figcaption></figure> <figure><img src="../.gitbook/assets/Screenshot 2024-11-29 155926.png" alt="" width="526"><figcaption></figcaption></figure></div>

在这种情况下，我们可以有一个检索器工具，并将**元数据检索器**放在矢量数据库和检索器工具之间。

<figure><img src="../.gitbook/assets/image (236).png" alt=""><figcaption></figcaption></figure>

## XML 代理

对于某些 LLM，不支持函数调用功能。在这种情况下，我们可以使用 XML Agent 以更结构化的格式/语法提示词 LLM，目的是使用提供的工具。

它有底层提示词：

```xml
You are a helpful assistant. Help the user answer any questions.

You have access to the following tools:

{tools}

In order to use a tool, you can use <tool></tool> and <tool_input></tool_input> tags. You will then get back a response in the form <observation></observation>
For example, if you have a tool called 'search' that could run a google search, in order to search for the weather in SF you would respond:

<tool>search</tool><tool_input>weather in SF</tool_input>
<observation>64 degrees</observation>

When you are done, respond with a final answer between <final_answer></final_answer>. For example:

<final_answer>The weather in SF is 64 degrees</final_answer>

Begin!

Previous Conversation:
{chat_history}

Question: {input}
{agent_scratchpad}
```

<figure><img src="../.gitbook/assets/image (20) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

## 结论

我们介绍了会话检索 QA 链的使用及其在查询多个文档时的局限性。我们通过使用 OpenAI Function Agent/XML Agent + 工具 解决了这个问题。您可以找到以下模板：

{% file src="../.gitbook/assets/ToolAgent Chatflow.json" %}

{% file src="../.gitbook/assets/XMLAgent Chatflow.json" %}
