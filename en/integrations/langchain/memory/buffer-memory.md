# Buffer Memory

Use Flowise database table `chat_message` as the storage mechanism for storing/retrieving conversations.

<figure><img src="../../../.gitbook/assets/image (1) (1) (3).png" alt="" width="299"><figcaption></figcaption></figure>

## Input

| Parameter  | Description                                                                   | Default       |
| ---------- | ----------------------------------------------------------------------------- | ------------- |
| Session Id | An ID to retrieve/store messages. If not specified, a random ID will be used. |               |
| Memory Key | A key used to format messages in prompt template                              | chat\_history |
