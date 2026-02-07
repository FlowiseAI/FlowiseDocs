# Threads

[Threads](https://platform.openai.com/docs/assistants/how-it-works/managing-threads-and-messages) n'est utilisé que lorsqu'un Assistant OpenAI est en cours d'utilisation. Il s'agit d'une session de conversation entre un Assistant et un utilisateur. Les threads stockent les messages et gèrent automatiquement la troncature pour adapter le contenu au contexte d'un modèle.

<figure><img src="../../../../.gitbook/assets/screely-1699896158130.png" alt=""><figcaption></figcaption></figure>

## Conversations séparées pour plusieurs utilisateurs

### UI & Chat intégré

Par défaut, l'UI et le Chat intégré sépareront automatiquement les threads pour les conversations de plusieurs utilisateurs. Cela se fait en générant un **`chatId`** unique pour chaque nouvelle interaction. Cette logique est gérée en arrière-plan par Flowise.

### API de prédiction

POST /`api/v1/prediction/{your-chatflowid}`, spécifiez le **`chatId`**. Le même thread sera utilisé pour le même chatId.

```json
{
    "question": "hello!",
    "chatId": "user1"
}
```

### Message API

* GET `/api/v1/chatmessage/{your-chatflowid}`
* DELETE `/api/v1/chatmessage/{your-chatflowid}`

Vous pouvez également filtrer via **`chatId` -** `/api/v1/chatmessage/{your-chatflowid}?chatId={your-chatid}`

Toutes les conversations peuvent également être visualisées et gérées depuis l'interface utilisateur :

<figure><img src="../../../../.gitbook/assets/image (77).png" alt=""><figcaption></figcaption></figure>
