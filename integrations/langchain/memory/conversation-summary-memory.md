# Conversation Summary Memory

Use Flowise database table `chat_message` as the storage mechanism for storing/retrieving conversations.

This memory type creates a brief summary of the conversation over time. This is useful for shortening information from long discussions. It updates and saves a current summary as the conversation goes on. This is especially helpful in longer chats, where saving every past message would take up too much space.

<figure><img src="../../../.gitbook/assets/image (3) (1) (1) (1) (2).png" alt="" width="296"><figcaption></figcaption></figure>

## Input

| Parameter  | Description                                                                   | Default       |
| ---------- | ----------------------------------------------------------------------------- | ------------- |
| Chat Model | LLM used to perform summarization                                             |               |
| Session Id | An ID to retrieve/store messages. If not specified, a random ID will be used. |               |
| Memory Key | A key used to format messages in prompt template                              | chat\_history |
