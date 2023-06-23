# Memory

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

There are different type of ways to store memory:

* Buffer Memory
* Buffer Window Memory
* Conversation Summary Memory
* [Zep Memory](zep-memory.md)
