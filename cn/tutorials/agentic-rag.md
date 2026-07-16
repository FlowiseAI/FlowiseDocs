# 代理 RAG

代理 RAG 是一种基于代理的方法，以精心策划的方式执行 [RAG](rag.md)。它可能涉及从各种文档源检索数据、比较摘要以及实施自动自我更正机制。

在本教程中，我们将探索如何构建一个自校正 RAG 系统，该系统检查检索到的数据的相关性，并在结果不相关时自动重新生成查询。

## 概述

Agentic RAG 流程实现了一个多步骤过程：

1. 验证传入查询并对其进行分类
2. 为矢量数据库检索生成优化的搜索查询
3. 评估检索到的文档的相关性
4. 当结果不相关时通过重新生成查询进行自我纠正
5. 根据检索到的信息提供上下文响应

<figure><img src="../.gitbook/assets/image (16) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

### 第 1 步：设置起始节点

首先向画布添加 **Start** 节点。这是您的智能体流程的入口点。

<figure><img src="../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

#### 配置：

* **输入类型**：选择“聊天输入”接受用户问题
* **流状态**：添加一个带有键“`query`”和空值的状态变量

Start 节点使用空的 `query` 变量初始化流状态，该变量将在整个过程中更新。

### 步骤 2：添加查询验证

添加 **Condition Agent** 节点并将其连接到 Start 节点。

<figure><img src="../.gitbook/assets/image (5) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

#### 配置：

* **说明**：“检查用户是否询问人工智能相关主题，或者只是一般查询”
* **输入**：`{{ question }}`（引用用户的输入）
* **场景**：
  * 场景1：“AI相关”
  * 场景2：“一般”

该节点充当路由器，确定查询是否需要专门的人工智能知识或可以普遍回答。

### 步骤 3：创建一般响应分支

对于非 AI 相关查询，添加连接到条件代理的输出 1 的 **LLM** 节点。

<figure><img src="../.gitbook/assets/image (6) (1) (1) (1) (1) (1) (1).png" alt="" width="375"><figcaption></figcaption></figure>

这为一般查询提供了直接响应，无需检索文档。您还可以替换为直接回复节点以返回预定义的答案。

<figure><img src="../.gitbook/assets/image (8) (1) (1) (1) (1) (1).png" alt="" width="375"><figcaption></figcaption></figure>

### 步骤 4：设置查询生成

对于 AI 相关查询，添加一个连接到条件代理的输出 0 的 **LLM** 节点 - 这是“AI 相关”的场景。

<figure><img src="../.gitbook/assets/image (9) (1) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

#### 配置：

* **消息**：添加系统消息：

    ```
    Given the user question and history, construct a short string that can be used for searching vector database. Only generate the query, no meta comments, no explanation

    Example:
    Question: what are the events happening today?
    Query: today's event

    Example:
    Question: how about the address?
    Query: business address of the shop

    Question: {{ question }}
    Query:
    ```
* **更新流状态**：将键“query”设置为值 `{{ output }}`。这会将“query”的值更新为此 LLM 节点的输出。

该节点将用户的自然语言问题转换为矢量数据库的优化搜索查询。

### 步骤 5：配置矢量数据库检索器

添加 **检索器** 节点并将其连接到“生成查询”LLM。

<figure><img src="../.gitbook/assets/image (10) (1) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

#### 配置：

* **知识（文档存储）**：选择您预先配置的文档存储（例如“ai paper”）
* **检索器查询**：`{{ $flow.state.query }}`（使用共享状态中的“查询”值）

该节点使用优化的查询搜索您的矢量数据库并返回相关文档。

### 步骤 6：添加文档相关性检查

添加另一个连接到检索器的 **条件代理** 节点。

<figure><img src="../.gitbook/assets/image (11) (1) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

#### 配置：

* **说明**：“确定文档是否与用户问题相关。用户问题为 \{{ question \}}”
* **输入**：`{{ retrieverAgentflow_0 }}`（引用步骤 5 中检索到的文档）
* **场景**：
  * 场景 1：“相关”
  * 场景 2：“无关紧要”

这评估检索到的文档是否实际上包含与用户问题相关的信息。

### 步骤 7：创建最终响应生成器

对于相关文档，添加一个连接到相关性检查器的输出 0 的 **LLM** 节点 - 这是匹配场景“相关”时的情况。

<figure><img src="../.gitbook/assets/image (12) (1) (1) (1) (1).png" alt="" width="373"><figcaption></figcaption></figure>

#### 配置：

* **输入消息**：

    ```
    Given the question: {{ question }}
    And the findings: {{ retrieverAgentflow_0 }}
    Output the final response
    ```

该节点通过将用户的问题与相关检索到的文档相结合来创建最终答案。

### 步骤 8：实施自我纠正

对于不相关的文档，添加一个连接到相关性检查器的输出 1 的 **LLM** 节点 - 对于第二种情况 - “不相关”。

<figure><img src="../.gitbook/assets/image (13) (1) (1) (1) (1).png" alt="" width="375"><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (14) (1) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

#### 配置：

* **消息**：添加系统消息：“您是一个有用的助手，可以转换查询以产生更好的问题。”
* **输入消息**：

    ```
    Look at the input and try to reason about the underlying semantic intent / meaning.
    Here is the initial question: {{ $flow.state.query }}
    Formulate an improved question:
    ```
* **更新流状态**：将键“query”设置为值 `{{ output }}`

<figure><img src="../.gitbook/assets/image (15) (1) (1) (1) (1).png" alt="" width="520"><figcaption></figcaption></figure>

该节点分析初始查询未返回相关结果的原因并生成改进版本。

### 步骤9：添加环回机制

添加连接到“重新生成问题”LLM 的 **Loop** 节点。

<figure><img src="../.gitbook/assets/image (16) (1) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

#### 配置：

* **循环返回**：选择“retrieverAgentflow\_0-检索器 Vector DB”
* **最大循环计数**：设置为 5（防止无限循环）

这会创建一个反馈循环，允许系统在初始结果不令人满意时重试改进的查询。

## 完整的流程结构

{% file src="../.gitbook/assets/Agentic RAG V2.json" %}

## 总结

1. 开始→检查查询是否有效
2. 检查查询是否有效（AI相关）→ 生成查询
3. 检查查询是否有效（一般）→一般答案
4. 生成查询→检索器向量数据库
5. 检索器 Vector DB → 检查文档是否相关
6. 检查文档是否相关（Relevant）→ 生成响应
7. 检查文档是否相关（不相关）→ 重新生成问题
8. 重新生成问题 → 循环回到检索器

## 测试你的流程

用各种类型的问题测试你的流程：

* 人工智能相关查询：“机器学习的最新进展是什么？”
* 一般查询：“今天天气怎么样？”
* 可能需要细化的复杂查询：“这项新技术是如何工作的？”

<figure><img src="../.gitbook/assets/image (17) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

这个 Agentic RAG 为基于文档的问答提供了一个强大的、自我改进的系统，可以处理简单和复杂的查询，同时通过迭代细化保持高精度。
