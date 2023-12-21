# Streaming

Flowise supports streaming back to your front end application when the final node is a **Chain** or **OpenAI Function Agent.**

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
