---
description: LangChain内存节点
---

# 内存

***

记忆可以让你与人工智能聊天，就好像人工智能拥有之前对话的记忆一样。

_<mark style="color:blue;">人类：嗨，我是鲍勃</mark>_

_<mark style="color:orange;">AI：你好，鲍勃！很高兴认识你。今天我能为您提供什么帮助？</mark>_

_<mark style="color:blue;">人类：我叫什么名字？</mark>_

_<mark style="color:orange;">AI：正如您之前提到的，您的名字是鲍勃。</mark>_

在底层，这些对话存储在数组或数据库中，并作为上下文提供给 LLM。例如：

```
You are an assistant to a human, powered by a large language model trained by OpenAI.

Whether the human needs help with a specific question or just wants to have a conversation about a particular topic, you are here to assist.

Current conversation:
{history}
```

### 内存节点：

* [缓冲内存](buffer-memory.md)
* [缓冲窗口内存](buffer-window-memory.md)
* [对话摘要记忆](conversation-summary-memory.md)
* [对话摘要缓冲区内存](conversation-summary-buffer-memory.md)
* [DynamoDB 聊天内存](dynamodb-chat-memory.md)
* [Mem0 内存](mem0-memory.md)
* [MongoDB Atlas 聊天内存](mongodb-atlas-chat-memory.md)
* [Redis 支持的聊天内存](redis-backed-chat-memory.md)
* [Upstash Redis 支持的聊天内存](upstash-redis-backed-chat-memory.md)
* [Zep 内存](zep-memory.md)

## 多个用户的单独对话

### UI 和嵌入式聊天

默认情况下，UI 和嵌入式聊天会自动分隔不同用户的对话。这是通过为每个新交互生成唯一的 **`chatId`** 来完成的。该逻辑由 Flowise 在幕后处理。

### 预测 API

您可以通过指定唯一的 **`sessionId`** 来分隔多个用户的对话

1. 对于每个内存节点，您应该能够看到一个输入参数 **`Session ID`**

<figure><img src="../../../.gitbook/assets/image (76).png" alt="" width="563"><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/Untitled (1) (1) (1) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

2. 在 `/api/v1/prediction/{your-chatflowid}` POST 正文请求中，指定 **`overrideConfig`** 中的 **`sessionId`**

```json
{
    "question": "hello!",
    "overrideConfig": {
        "sessionId": "user1"
    }
}
```

### 消息 API

* GET `/api/v1/chatmessage/{your-chatflowid}`
* DELETE `/api/v1/chatmessage/{your-chatflowid}`

<table><thead><tr><th>查询参数</th><th width="192">类型</th><th>价值</th></tr></thead><tbody><tr><td>会话ID</td><td>字符串</td><td></td></tr><tr><td>排序</td><td>枚举</td><td>ASC 或 DESC</td></tr><tr><td>开始日期</td><td>字符串</td><td></td></tr><tr><td>结束日期</td><td>字符串</td><td></td></tr></tbody></table>

所有对话也可以通过 UI 进行可视化和管理：

<figure><img src="../../../.gitbook/assets/image (78).png" alt=""><figcaption></figcaption></figure>

对于 OpenAI 助手，[Threads](../agents/openai-assistant/threads.md) 将用于存储对话。
