---
description: Nodos de Memory de LangChain
---

# Memory

***

Memory te permite chatear con la AI como si la AI tuviera memoria de conversaciones anteriores.

_<mark style="color:blue;">Humano: hola soy bob</mark>_

_<mark style="color:orange;">AI: ¡Hola Bob! Es un placer conocerte. ¿Cómo puedo ayudarte hoy?</mark>_

_<mark style="color:blue;">Humano: ¿cuál es mi nombre?</mark>_

_<mark style="color:orange;">AI: Tu nombre es Bob, como mencionaste anteriormente.</mark>_

Internamente, estas conversaciones se almacenan en arrays o bases de datos, y se proporcionan como contexto al LLM. Por ejemplo:

```
Eres un asistente para un humano, impulsado por un modelo de lenguaje grande entrenado por OpenAI.

Ya sea que el humano necesite ayuda con una pregunta específica o solo quiera tener una conversación sobre un tema en particular, estás aquí para ayudar.

Conversación actual:
{history}
```

### Nodos de Memory:

* [Buffer Memory](buffer-memory.md)
* [Buffer Window Memory](buffer-window-memory.md)
* [Conversation Summary Memory](conversation-summary-memory.md)
* [Conversation Summary Buffer Memory](conversation-summary-buffer-memory.md)
* [DynamoDB Chat Memory](dynamodb-chat-memory.md)
* [Mem0 Memory](mem0-memory.md)
* [MongoDB Atlas Chat Memory](mongodb-atlas-chat-memory.md)
* [Redis-Backed Chat Memory](redis-backed-chat-memory.md)
* [Upstash Redis-Backed Chat Memory](upstash-redis-backed-chat-memory.md)
* [Zep Memory](zep-memory.md)

## Conversaciones separadas para múltiples usuarios

### UI y Chat Embebido

Por defecto, la UI y el Chat Embebido separarán automáticamente las conversaciones de diferentes usuarios. Esto se hace generando un **`chatId`** único para cada nueva interacción. Esa lógica es manejada internamente por Flowise.

### API de Predicción

Puedes separar las conversaciones para múltiples usuarios especificando un **`sessionId`** único

1. Para cada nodo de memory, deberías poder ver un parámetro de entrada **`Session ID`**

<figure><img src="../../../.gitbook/assets/image (76).png" alt="" width="563"><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/Untitled (1) (1) (1) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

2. En el cuerpo de la solicitud POST `/api/v1/prediction/{your-chatflowid}`, especifica el **`sessionId`** en **`overrideConfig`**

```json
{
    "question": "¡hola!",
    "overrideConfig": {
        "sessionId": "user1"
    }
}
```

### API de Mensajes

* GET `/api/v1/chatmessage/{your-chatflowid}`
* DELETE `/api/v1/chatmessage/{your-chatflowid}`

<table><thead><tr><th>Parámetro de Consulta</th><th width="192">Tipo</th><th>Valor</th></tr></thead><tbody><tr><td>sessionId</td><td>string</td><td></td></tr><tr><td>sort</td><td>enum</td><td>ASC o DESC</td></tr><tr><td>startDate</td><td>string</td><td></td></tr><tr><td>endDate</td><td>string</td><td></td></tr></tbody></table>

Todas las conversaciones también pueden ser visualizadas y gestionadas desde la UI:

<figure><img src="../../../.gitbook/assets/image (78).png" alt=""><figcaption></figcaption></figure>

Para OpenAI Assistant, se utilizarán [Threads](../agents/openai-assistant/threads.md) para almacenar las conversaciones.
