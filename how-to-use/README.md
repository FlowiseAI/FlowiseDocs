# How to Use

Now that you have tested your chatflow with the chat interface on Flowise, you want to "export" it out to be able to use with other applications. Flowise provides 2 ways to do that:

* API
* Embed

## API

You can use the chatflow as API and connect to frontend applications.

<figure><img src="../.gitbook/assets/image (16).png" alt=""><figcaption></figcaption></figure>

Watch how to connect to [Bubble](https://bubble.io/)

{% embed url="https://youtu.be/kOwmPe8aLAA" %}

Watch how to connect to [FlutterFlow](https://flutterflow.io/)

{% embed url="https://youtu.be/iI84yym473Q" %}

## Streaming

Flowise also support streaming back to your front end application if **ConversationalRetrievalQAChain** or **OpenAI Function Agent** is being used.

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

You can also embed a chat widget to your website.&#x20;

Simply copy paste the embedded code provided to anywhere in the `<body>` tag of your html file.

<figure><img src="../.gitbook/assets/image (8) (2) (1) (1).png" alt=""><figcaption></figcaption></figure>

Watch how to do that:

{% embed url="https://github.com/FlowiseAI/Flowise/assets/26460777/c128829a-2d08-4d60-b821-1e41a9e677d0" %}

