# Long Term Memory

Long Term Memory in Flowise refers to memory nodes that are capable of persisting past conversations, that can be later retrieved to resume the conversation. This allows the conversations for different users to be isolated.

There are 5 long term memory nodes in Flowise:

* DynamoDB Chat Memory
* Motorhead Memory
* [Redis Chat Memory](broken-reference)
* Upstash Chat Memory
* [Zep Memory](zep-memory.md)

<figure><img src="../../../../.gitbook/assets/screely-1699894602544.png" alt=""><figcaption></figcaption></figure>

## Separate conversations for multiple users

### UI & Embedded Chat

By default, UI and Embedded Chat will automatically separate different users conversations. This is done by generating a unique **`chatId`** for each new interaction. That logic is handled under the hood by Flowise.

### Prediction API

You can separate the conversations for multiple users by specifying a unique **`sessionId`**

1. Use one of the long term memory nodes on Flowise. Make sure the node has the input parameter **`Session ID`**

<figure><img src="../../../../.gitbook/assets/image (76).png" alt="" width="563"><figcaption></figcaption></figure>

<figure><img src="../../../../.gitbook/assets/Untitled (1) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

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

<figure><img src="../../../../.gitbook/assets/image (78).png" alt=""><figcaption></figcaption></figure>
