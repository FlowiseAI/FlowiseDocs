---
description: LangChain Memory Nodes
---

# Mémoire

***

La mémoire vous permet de discuter avec l'IA comme si l'IA avait la mémoire des conversations précédentes.

_ <Mark Style = "Color: Blue;"> humain: salut je suis bob </mark> _

_ <Mark Style = "Color: Orange;"> AI: Bonjour Bob! Ravi de vous rencontrer. Comment puis-je vous aider aujourd'hui? </mark> _

_ <Mark Style = "Color: Blue;"> Human: quel est mon nom? </mark> _

_ <Mark Style = "Color: Orange;"> AI: Votre nom est Bob, comme vous l'avez mentionné plus tôt. </mark> _

Sous le capot, ces conversations sont stockées dans des tableaux ou des bases de données et fournies comme contexte à LLM. Par exemple:

```
You are an assistant to a human, powered by a large language model trained by OpenAI.

Whether the human needs help with a specific question or just wants to have a conversation about a particular topic, you are here to assist.

Current conversation:
{history}
```

### Nœuds de mémoire:

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

## Conversations séparées pour plusieurs utilisateurs

### UI et chat embarqué

Par défaut, l'interface utilisateur et le chat embarqué séparent automatiquement les différentes conversations d'utilisateurs. Cela se fait en générant un ** unique **`chatId`** Pour chaque nouvelle interaction. Cette logique est manipulée sous le capot en fluant.

### API de prédiction

Vous pouvez séparer les conversations pour plusieurs utilisateurs en spécifiant un ** unique`sessionId`**

1. Pour chaque nœud de mémoire, vous devriez pouvoir voir un paramètre d'entrée **`Session ID`**

<gigne> <img src = "../../../. GitBook / Assets / Image (76) .png" alt = "" width = "563"> <Figcaption> </ Figcaption> </ Figure>

<gigne> <img src = "../../../. GitBook / Assets / Untitled (1) (1) (1) (1) (1) (1) .png" alt = "" width = "563"> <figcaption> </gigcaption> </ figure>

2. Dans le`/api/v1/prediction/{your-chatflowid}`Demande post-corps, spécifiez le **`sessionId`** dans **`overrideConfig`**

```json
{
    "question": "hello!",
    "overrideConfig": {
        "sessionId": "user1"
    }
}
```

### API du message

* OBTENIR`/api/v1/chatmessage/{your-chatflowid}`
* SUPPRIMER`/api/v1/chatmessage/{your-chatflowid}`

<ballage> <thead> <tr> <th> Query Param </th> <Th width = "192"> Type </ th> <th> value </ th> </tr> </ thead> <tbody> <tr> <td> sessiond </td> <td> string </td> <td> </td> </tr> <td> ASC Desc </td> </ tr> <tr> <td> startDate </td> <td> string </td> <td> </td> </tr> <tr> <td> enddate </td> <td> string </td> <td> </td> </tr> </tbody> </-table>

Toutes les conversations peuvent également être visualisées et gérées à partir de l'interface utilisateur:

<gigne> <img src = "../../../. GitBook / Assets / Image (78) .png" alt = ""> <Figcaption> </gigcaption> </gigne>

Pour l'assistant openai,[Threads](../agents/openai-assistant/threads.md)sera utilisé pour stocker des conversations.
