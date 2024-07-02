---
description: LangChain Memory Nodes
---

# Memory

***

Memory allow you to chat with AI as if AI has the memory of previous conversations.

_<mark style="color:blue;">Human: hi i am bob</mark>_

_<mark style="color:orange;">AI: Hello Bob! It's nice to meet you. How can I assist you today?</mark>_

_<mark style="color:blue;">Human: what's my name?</mark>_

_<mark style="color:orange;">AI: Your name is Bob, as you mentioned earlier.</mark>_

Under the hood, these conversations are stored in arrays or databases, and provided as context to LLM. For example:

```
You are an assistant to a human, powered by a large language model trained by OpenAI.

Whether the human needs help with a specific question or just wants to have a conversation about a particular topic, you are here to assist.

Current conversation:
{history}
```

### Memory Nodes:

* [Buffer Memory](buffer-memory.md)
* [Buffer Window Memory](buffer-window-memory.md)
* [Conversation Summary Memory](conversation-summary-memory.md)
* [Conversation Summary Buffer Memory](conversation-summary-buffer-memory.md)
* [DynamoDB Chat Memory](dynamodb-chat-memory.md)
* [MongoDB Atlas Chat Memory](mongodb-atlas-chat-memory.md)
* [Redis-Backed Chat Memory](redis-backed-chat-memory.md)
* [Upstash Redis-Backed Chat Memory](upstash-redis-backed-chat-memory.md)
* [Zep Memory](zep-memory.md)

## Separate conversations for multiple users

### UI & Embedded Chat

By default, UI and Embedded Chat will automatically separate different users conversations. This is done by generating a unique **`chatId`** for each new interaction. That logic is handled under the hood by Flowise.

### Prediction API

You can separate the conversations for multiple users by specifying a unique **`sessionId`**

1. For every memory node, you should be able to see a input parameter **`Session ID`**

<figure><img src="../../../.gitbook/assets/image (76).png" alt="" width="563"><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/Untitled (1) (1) (1) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

2. In the `/api/v1/prediction/{your-chatflowid}` POST body request, specify the **`sessionId`** in **`overrideConfig`**

```json
{
    "question": "hello!",
    "overrideConfig": {
        "sessionId": "user1"
    }
}
```

### Message API

* GET `/api/v1/chatmessage/{your-chatflowid}`
* DELETE `/api/v1/chatmessage/{your-chatflowid}`

<table><thead><tr><th>Query Param</th><th width="192">Type</th><th>Value</th></tr></thead><tbody><tr><td>sessionId</td><td>string</td><td></td></tr><tr><td>sort</td><td>enum</td><td>ASC or DESC</td></tr><tr><td>startDate</td><td>string</td><td></td></tr><tr><td>endDate</td><td>string</td><td></td></tr></tbody></table>

All conversations can be visualized and managed from UI as well:

<figure><img src="../../../.gitbook/assets/image (78).png" alt=""><figcaption></figcaption></figure>

For OpenAI Assistant, [Threads](../agents/openai-assistant/threads.md) will be used to store conversations.
