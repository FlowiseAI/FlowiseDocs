# How to Use

Now that you have tested your chatflow with the chat interface on Flowise, you want to "export" it out to be able to use with other applications. Flowise provides 2 ways to do that:

* API
* Embed

## API

You can use the chatflow as API and connect to frontend applications.

<figure><img src="../.gitbook/assets/image (16).png" alt=""><figcaption></figcaption></figure>

You also have the flexibility to override input configuration with **overrideConfig** property.

<figure><img src="../.gitbook/assets/image (45).png" alt=""><figcaption></figcaption></figure>

An example of API call using Postman:

<figure><img src="../.gitbook/assets/image (9) (2).png" alt=""><figcaption></figcaption></figure>

<table><thead><tr><th width="161">Key</th><th>Description</th><th>Type</th><th>Required</th></tr></thead><tbody><tr><td>question</td><td>User's question</td><td>string</td><td>Yes</td></tr><tr><td>overrideConfig</td><td>Override existing flow configuration</td><td>object</td><td>No</td></tr><tr><td>history</td><td>Provide list of history messages to the flow</td><td>array</td><td>No</td></tr></tbody></table>

```json
// Example body request
{
    "question": "What's my name?",
    "history": [
        {
            "message": "Hello, how can I assist you?",
            "type": "apiMessage"
        },
        {
            "type": "userMessage",
            "message": "Hello I am Bob"
        },
        {
            "type": "apiMessage",
            "message": "Hello Bob! how can I assist you?"
        }
    ],
    "overrideConfig": {
        "returnSourceDocuments": true
    }
}
```

#### Flow with Document Loaders

If the flow contains [Document Loaders](../document-loaders/), the API looks slightly different. Instead of passing as **JSON** body, **form-data** is being used. This allows you to upload any files to the API.

Note: It is user's responsibility to make sure the file type is compatible with the expected file type from document loader. For example, if a Text File Loader is being used, you should only upload file with `.txt` extension.

<figure><img src="../.gitbook/assets/image (41).png" alt=""><figcaption></figcaption></figure>

An example of API call with `form-data` using Postman:

<figure><img src="../.gitbook/assets/image (1) (4).png" alt=""><figcaption></figcaption></figure>

An example of API call using Python

```python
import requests

API_URL = "your-flowise-api"

# use form data to upload files
form_data = {
    "files": ('state_of_the_union.txt', open('state_of_the_union.txt', 'rb'))
}

body_data = {
    "question": "what is this document about?",
    "returnSourceDocuments": True
}

def query(form_data):
    response = requests.post(API_URL, files=form_data, data=body_data)
    print(response)
    return response.json()

output = query(form_data)
print(output)
```

Watch how to use Flowise API

{% embed url="https://youtu.be/LhN560DhlzU" %}

Watch how to connect to [Bubble](https://bubble.io/)

{% embed url="https://youtu.be/kOwmPe8aLAA" %}

## Streaming

Flowise also support streaming back to your front end application when the final node is a **Chain** or **OpenAI Function Agent.**

<figure><img src="../.gitbook/assets/screely-1687030897806.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/screely-1687030924019.png" alt=""><figcaption></figcaption></figure>

1. Install socket.io-client to your front-end application

```bash
yarn add socket.io-client
```

or using npm

```bash
npm install socket.io-client
```

Refer [official docs](https://socket.io/docs/v4/client-api/) for more installation options.

2. Import it

```javascript
import socketIOClient from 'socket.io-client'
```

3. Establish connection

```javascript
const socket = socketIOClient("http://localhost:3000") //flowise url
```

4. Listen to connection

```javascript
import { useState } from 'react'

const [socketIOClientId, setSocketIOClientId] = useState('');

socket.on('connect', () => {
  setSocketIOClientId(socket.id)
});
```

4. Send query with `socketIOClientId`

```javascript
async function query(data) {
    const response = await fetch(
        "http://localhost:3000/api/v1/prediction/<chatflow-id>",
        {
            method: "POST",
            body: data
        }
    );
    const result = await response.json();
    return result;
}

query({
  "question": "Hey, how are you?",
  "socketIOClientId": socketIOClientId
}).then((response) => {
    console.log(response);
});
```

5. Listen to token stream

```javascript
socket.on('start', () => {
  console.log('start');
});

socket.on('token', (token) => {
  console.log('token:', token);
});

socket.on('sourceDocuments', (sourceDocuments) => {
  console.log('sourceDocuments:', sourceDocuments);
});

socket.on('end', () => {
  console.log('end');
});
```

6. Disconnect connection

```javascript
socket.disconnect();
```

## Embed

You can also embed a chat widget to your website.

Simply copy paste the embedded code provided to anywhere in the `<body>` tag of your html file.

<figure><img src="../.gitbook/assets/image (8) (2) (1) (1).png" alt=""><figcaption></figcaption></figure>

Watch how to do that:

{% embed url="https://github.com/FlowiseAI/Flowise/assets/26460777/c128829a-2d08-4d60-b821-1e41a9e677d0" %}

You can also customize your own embedded chat widget UI and pass **chatflowConfig** JSON object to override existing config. See [configuration list](https://github.com/FlowiseAI/FlowiseChatEmbed#configuration).

To modify the full source code of embedded chat widget, follow these steps:

1. Fork the [Flowise Chat Embed](https://github.com/FlowiseAI/FlowiseChatEmbed) repository
2. Then you can make any code changes. One of the popular ask is to remove Flowise [branding](https://github.com/HenryHengZJ/FlowiseChatEmbed-Test/blob/main/src/components/Bot.tsx#L337).
3. Run `yarn build`
4. Push changes to the forked repo
5. You can then use it as embedded chat like so:

Replace `username` to your Github username, and `forked-repo` to your forked repo.

<pre class="language-html"><code class="lang-html"><strong>&#x3C;script type="module">
</strong>      import Chatbot from "https://cdn.jsdelivr.net/gh/username/forked-repo/dist/web.js"
      Chatbot.init({
          chatflowid: "chatflow-id",
          apiHost: "http://localhost:3000",
      })
&#x3C;/script>
</code></pre>

<figure><img src="../.gitbook/assets/image (1) (1) (2).png" alt="" width="563"><figcaption></figcaption></figure>

```html
<script type="module">
      import Chatbot from "https://cdn.jsdelivr.net/gh/HenryHengZJ/FlowiseChatEmbed-Test/dist/web.js"
      Chatbot.init({
          chatflowid: "chatflow-id",
          apiHost: "http://localhost:3000",
      })
</script>
```

Watch how to embed Flowise in a Bootstrap 5 website

{% embed url="https://youtu.be/4paQ2wObDQ4" %}

Watch how to add chatbot to website

{% embed url="https://youtu.be/XOeCV1xyN48" %}
