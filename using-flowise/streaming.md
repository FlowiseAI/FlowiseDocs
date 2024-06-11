---
description: Learn when you can stream back to your front end
---

# Streaming

***

Flowise supports streaming back to your front end application when the final node is a **Chain** or **Tool Agent.**

<figure><img src="../.gitbook/assets/streaming-1.webp" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/screely-1687030924019.png" alt=""><figcaption></figcaption></figure>

## Setup

1. Install socket.io-client to your front-end application

{% tabs %}
{% tab title="Yarn" %}
```bash
yarn add socket.io-client
```
{% endtab %}

{% tab title="Npm" %}
```bash
npm install socket.io-client
```
{% endtab %}

{% tab title="Pnpm" %}
```bash
pnpm add socket.io-client
```
{% endtab %}
{% endtabs %}

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
// When LLM start streaming
socket.on('start', () => {
  console.log('start');
});

// The delta token/word when streaming
socket.on('token', (token) => {
  console.log('token:', token);
});

// Source documents returned from the chatflow
socket.on('sourceDocuments', (sourceDocuments) => {
  console.log('sourceDocuments:', sourceDocuments);
});

// Tools used during execution
socket.on('usedTools', (usedTools) => {
  console.log('usedTools:', usedTools);
});

// When LLM finished streaming
socket.on('end', () => {
  console.log('end');
});

//------------------- For Multi Agents ----------------------

// The next agent in line
socket.on('nextAgent', (nextAgent) => {
  console.log('nextAgent:', nextAgent);
});

// The whole multi agents thoughts, reasoning and output
socket.on('agentReasoning', (agentReasoning) => {
  console.log('agentReasoning:', agentReasoning);
});

// When execution is aborted/interrupted
socket.on('abort', () => {
  console.log('abort');
});
```

6. Disconnect connection

```javascript
socket.disconnect();
```
