# 结构化输出

在许多用例中，例如聊天机器人，模型预计会以自然语言回复用户。然而，在某些情况下，自然语言响应并不理想。例如，如果我们需要获取模型的输出，将其作为 HTTP 请求的主体传递，或存储到数据库中，则输出必须与预定义的模式保持一致。这一要求产生了“结构化输出”的概念，其中模型被引导以特定的结构化格式生成响应。

在本教程中，我们将了解如何从 LLM 生成结构化输出，并将其作为 HTTP 请求的正文传递。

## 先决条件

我们将针对 HTTP 请求使用相同的 [事件管理服务器](interacting-with-api.md#prerequisite)。

绝对地！这是针对您的**结构化输出流**的教程，其格式与您的“代理作为工具”文档一致，包括分步说明和图像占位符。

***

## 概述

1. 通过 Start 节点接收用户输入。
2. 使用 LLM 生成结构化 JSON 数组。
3. 循环遍历数组中的每一项。
4. 通过 HTTP 将每个项目发送到外部端点。

<figure><img src="../.gitbook/assets/image (306).png" alt=""><figcaption></figcaption></figure>

### 第 1 步：设置起始节点

首先向画布添加 **Start** 节点。

<figure><img src="../.gitbook/assets/image (307).png" alt="" width="417"><figcaption></figcaption></figure>

**关键输入参数：**

* **输入类型：**
  * `chatInput`（默认）：流程以来自用户的聊天消息开始。
  * `formInput`：流程以表单开始（如果您想从用户收集结构化数据）。
* **短暂记忆：**
  * （可选）如果启用，流程不会保留运行之间的聊天历史记录。
* **流程状态：**
  *（可选）预填充状态变量。
  * 示例：

      ```json
      [
        { "key": "answers", "value": "" }
      ]
      ```
* **持续状态：**
  * （可选）如果启用，状态将在同一会话中保留。

### 步骤 2：使用 LLM 生成结构化输出

添加 LLM 节点并将其连接到 Start 节点。

<figure><img src="../.gitbook/assets/image (308).png" alt="" width="563"><figcaption></figcaption></figure>

**用途：** 使用语言模型分析输入并生成结构化的 JSON 数组。

**关键输入参数：**

* **JSON 结构化输出：**
  * **关键：** `answers`
  * **类型：** `JSON Array`
  * **JSON 架构：**

      ```json
      {
        "name": { "type": "string", "required": true, "description": "Name of the event" },
        "date": { "type": "string", "required": true, "description": "Date of the event" },
        "location": { "type": "string", "required": true, "description": "Location of the event" }
      }
      ```
  * **描述：**“回答用户查询”
* **更新流程状态：**
  * 使用生成的 JSON 输出更新流状态。
  * 示例：

      ```json
      [
        {
          "key": "answers",
          "value": "{{ output.answers }}"
        }
      ]
      ```

### 步骤 3：循环遍历 JSON 数组

添加迭代节点并将其连接到 LLM 节点的输出。

<figure><img src="../.gitbook/assets/image (309).png" alt="" width="563"><figcaption></figcaption></figure>

**用途：** 迭代从 LLM 节点生成的 JSON 数组中的每一项。

**关键输入参数：**

* **数组输入：**

    * 要迭代的数组。设置为已保存状态的答案：

    ```html
    {{ $flow.state.answers }}
    ```

    * 这意味着节点将循环遍历answers 数组中的每个事件。

### 第 4 步：通过 HTTP 发送每件物品

在循环内部，添加 **HTTP** 节点。

<figure><img src="../.gitbook/assets/image (311).png" alt="" width="563"><figcaption></figcaption></figure>

**用途：** 对于数组中的每个项目，向指定端点（例如 `http://localhost:5566/events`）发送 HTTP POST 请求。

**关键输入参数：**

* **方法：**
  * `POST`（此用例的默认值）。
* **URL:**
  * 发送数据的端点。
  * 示例：

      ```
      http://localhost:5566/events
      ```
* **标题：**
  * （可选）添加任何必需的 HTTP 标头（例如，用于身份验证）。
* **查询参数：**
  * （可选）如果需要，添加任何查询参数。
* **体型：**
  * `json`（默认）：将正文作为 JSON 发送。
* **身体：**
  * 在请求正文中发送的数据。
  * 设置为循环中的当前项：

      ```html
      {{ $iteration }}
      ```
* **响应类型：**
* `json`（默认）：期望 JSON 响应。

***

## 交互示例

**用户输入：**

```
create 2 events:
1. JS Conference on next Sat in Netherlands
2. GenAI meetup, Sept 19, in Dublin
```

**流量：**

* 起始节点接收输入。
* LLM 节点生成 JSON 事件数组。
* 循环节点迭代每个事件。
* HTTP 节点通过 API 创建每个事件。

<figure><img src="../.gitbook/assets/image (304).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (305).png" alt=""><figcaption></figcaption></figure>

***

## 完整的流程结构

{% file src="../.gitbook/assets/Structured Output.json" %}

***

## 最佳实践

**设计指南：**

1. **明确输出架构：** 定义 LLM 输出的预期结构，以确保可靠的下游处理。

**常见用例：**

* **事件处理：** 收集事件数据并将其发送到日历或事件管理系统。
* **批量数据输入：** 生成多条记录并将其提交到数据库或 API。
* **自动通知：** 为列表中的每个项目发送个性化消息或警报。
