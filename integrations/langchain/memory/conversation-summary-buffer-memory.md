# Conversation Summary Buffer Memory

Use Flowise database table `chat_message` as the storage mechanism for storing/retrieving conversations.

This memory keeps a buffer of recent interactions and compiles old ones into a summary, using both in its storage. Instead of flushing old interactions based solely on their number, it now considers the total length of tokens to decide when to clear them out.

<figure><img src="../../../.gitbook/assets/image (4) (1) (2).png" alt="" width="297"><figcaption></figcaption></figure>

## Input

| Parameter       | Description                                                                   | Default       |
| --------------- | ----------------------------------------------------------------------------- | ------------- |
| Chat Model      | LLM used to perform summarization                                             |               |
| Max Token Limit | Summarize conversations once token limit is reached                           | 2000          |
| Session Id      | An ID to retrieve/store messages. If not specified, a random ID will be used. |               |
| Memory Key      | A key used to format messages in prompt template                              | chat\_history |
