# Buffer Window Memory

Use Flowise database table `chat_message` as the storage mechanism for storing/retrieving conversations.

Difference being it only fetches the last K interactions. This approach is beneficial for preserving a sliding window of the most recent interactions, ensuring the buffer remains manageable in size.

<figure><img src="../../../.gitbook/assets/image (1) (1) (3) (1).png" alt="" width="298"><figcaption></figcaption></figure>

## Input

| Parameter  | Description                                                                   | Default       |
| ---------- | ----------------------------------------------------------------------------- | ------------- |
| Size       | Last K messages to fetch                                                      | 4             |
| Session Id | An ID to retrieve/store messages. If not specified, a random ID will be used. |               |
| Memory Key | A key used to format messages in prompt template                              | chat\_history |
