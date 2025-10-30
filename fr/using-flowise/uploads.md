---
description: Learn how to use upload images, audio, and other files
---

# Téléchargements

Flowise vous permet de télécharger des images, de l'audio et d'autres fichiers à partir du chat. Dans cette section, vous apprendrez à activer et à utiliser ces fonctionnalités.

## Image

Certains modèles de chat vous permettent de saisir des images. Reportez-vous toujours à la documentation officielle du LLM pour confirmer si le modèle prend en charge l'entrée d'image.

* [ChatOpenAI](../integrations/llamaindex/chat-models/chatopenai.md)
* [AzureChatOpenAI](../integrations/llamaindex/chat-models/azurechatopenai.md)
* [ChatAnthropic](../integrations/langchain/chat-models/chatanthropic.md)
* [AWSChatBedrock](../integrations/langchain/chat-models/aws-chatbedrock.md)
* [ChatGoogleGenerativeAI](../integrations/langchain/chat-models/google-ai.md)
* [ChatOllama](../integrations/llamaindex/chat-models/chatollama.md)
* [Google Vertex AI](../integrations/langchain/llms/googlevertex-ai.md)

{% hint style = "avertissement"%}
Le traitement d'image ne fonctionne qu'avec certaines chaînes / agents dans ChatFlow.

[LLMChain](../integrations/langchain/chains/llm-chain.md), [Conversation Chain](../integrations/langchain/chains/conversation-chain.md), [ReAct Agent](../integrations/langchain/agents/react-agent-chat.md), [Conversational Agent](../integrations/langchain/agents/conversational-agent.md), [Tool Agent](../integrations/langchain/agents/tool-agent.md)
{% EndHint%}

Si vous activez ** Autoriser le téléchargement d'images **, vous pouvez télécharger des images à partir de l'interface de chat.

<div align = "Center"> <gigne> <img src = "../. gitbook / actifs / image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) .png" alt = "" width = "255"> <Figcaption> </gigcaption> </gigust> <stigué> <img src = "../. GitBook / Assets / ScreenShot 2024-02-29 011714.png" alt = "" width = "290"> </ Figcaption> </Figcaption> </Gigu

Pour télécharger des images avec l'API:

{% Tabs%}
{% tab title = "python"%}
```python
import requests
API_URL = "http://localhost:3000/api/v1/prediction/<chatflowid>"

def query(payload):
    response = requests.post(API_URL, json=payload)
    return response.json()
    
output = query({
    "question": "Can you describe the image?",
    "uploads": [
        {
            "data": "data:image/png;base64,iVBORw0KGgdM2uN0", # base64 string or url
            "type": "file", # file | url
            "name": "Flowise.png",
            "mime": "image/png"
        }
    ]
})
```
{% endtab%}

{% tab title = "javascript"%}
```javascript
async function query(data) {
    const response = await fetch(
        "http://localhost:3000/api/v1/prediction/<chatflowid>",
        {
            method: "POST",
            headers: {
                "Content-Type": "application/json"
            },
            body: JSON.stringify(data)
        }
    );
    const result = await response.json();
    return result;
}

query({
    "question": "Can you describe the image?",
    "uploads": [
        {
            "data": "data:image/png;base64,iVBORw0KGgdM2uN0", //base64 string or url
            "type": "file", // file | url
            "name": "Flowise.png",
            "mime": "image/png"
        }
    ]
}).then((response) => {
    console.log(response);
});
```
{% endtab%}
{% endtabs%}

## Audio

Dans la configuration de ChatFlow, vous pouvez sélectionner un module Speech-to-Text. Les intégrations prises en charge comprennent:

* Openai
* Assemblage
* [LocalAI](../integrations/langchain/chat-models/chatlocalai.md)

Lorsque cela est activé, les utilisateurs peuvent parler directement dans le microphone. Leur discours est transcrit en texte.

<div align = "Left"> <Figure> <img src = "../. GitBook / Assets / Image (2) (1) (1) (1) (1) (1) (1) (1) (1). <gigne> <img src = "../. GitBook / Assets / Captures 2024-02-29 012538.png" alt = "" width = "431"> <figcaption> </gigcaption> </ figure> </ div>

Pour télécharger l'audio avec l'API:

{% Tabs%}
{% tab title = "python"%}
```python
import requests
API_URL = "http://localhost:3000/api/v1/prediction/<chatflowid>"

def query(payload):
    response = requests.post(API_URL, json=payload)
    return response.json()
    
output = query({
    "uploads": [
        {
            "data": "data:audio/webm;codecs=opus;base64,GkXf", # base64 string
            "type": "audio",
            "name": "audio.wav",
            "mime": "audio/webm"
        }
    ]
})
```
{% endtab%}

{% tab title = "javascript"%}
```javascript
async function query(data) {
    const response = await fetch(
        "http://localhost:3000/api/v1/prediction/<chatflowid>",
        {
            method: "POST",
            headers: {
                "Content-Type": "application/json"
            },
            body: JSON.stringify(data)
        }
    );
    const result = await response.json();
    return result;
}

query({
    "uploads": [
        {
            "data": "data:audio/webm;codecs=opus;base64,GkXf", // base64 string
            "type": "audio",
            "name": "audio.wav",
            "mime": "audio/webm"
        }
    ]
}).then((response) => {
    console.log(response);
});
```
{% endtab%}
{% endtabs%}

## Fichiers

Vous pouvez télécharger des fichiers de deux manières:

* Retrouver des téléchargements de fichiers de génération augmentée (RAG)
* Téléchargements de fichiers complets

Lorsque les deux options sont activées, les téléchargements de fichiers complets ont priorité.

### Téléchargements de fichiers

Vous pouvez upser des fichiers téléchargés à la volée vers le magasin vectoriel. Pour activer les téléchargements de fichiers, assurez-vous de répondre à ces conditions préalables:

* Vous devez inclure un magasin vectoriel qui prend en charge les téléchargements de fichiers dans le ChatFlow.
  * [Pinecone](../integrations/langchain/vector-stores/pinecone.md)
  * [Milvus](../integrations/langchain/vector-stores/milvus.md)
  * [Postgres](../integrations/langchain/vector-stores/postgres.md)
  * [Qdrant](../integrations/langchain/vector-stores/qdrant.md)
  * [Upstash](../integrations/langchain/vector-stores/upstash-vector.md)
* Si vous avez plusieurs magasins vectoriels dans un ChatFlow, vous ne pouvez activer que le téléchargement de fichiers pour un magasin vectoriel à la fois.
* Vous devez connecter au moins un nœud de chargeur de documents à l'entrée de document du magasin vectoriel.
* Chargeurs de documents pris en charge:
  * [CSV File](../integrations/langchain/document-loaders/csv-file.md)
  * [Docx File](../integrations/langchain/document-loaders/docx-file.md)
  * [Json File](../integrations/langchain/document-loaders/json-file.md)
  * [Json Lines File](broken-reference)
  * [PDF File](../integrations/langchain/document-loaders/pdf-file.md)
  * [Text File](../integrations/langchain/document-loaders/text-file.md)
  * [Unstructured File](../integrations/langchain/document-loaders/unstructured-file-loader.md)

<gigne> <img src = "../. Gitbook / Assets / image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) .png" alt = ""> <figcaption> </ / Figcaption> </ Figure>

Vous pouvez télécharger un ou plusieurs fichiers dans le chat:

<div align = "Left"> <Figure> <img src = "../. Gitbook / Assets / Image (3) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) .png" alt = "" width = "380"> <figcaption> </figcontion> </ Figure> <imgggtion> <imgggtion> <imggtion> <imgggtion> </ imggtion> <imgggtion> <imggtion> <imggtion> </ figction> src = "../. GitBook / Assets / Captures 2024-08-26 170456.png" alt = ""> <figcaption> </figcaption> </gigust> </div>

Voici comment cela fonctionne:

1. Les métadonnées des fichiers téléchargées sont mises à jour avec le Chatid.
2. Cela associe le fichier au Chatid.
3. Lors de l'interrogation, un filtre ** ou ** s'applique:

* Les métadonnées contient`flowise_chatId`et la valeur est l'ID actuel de la session de chat
* Les métadonnées ne contiennent pas`flowise_chatId`

Un exemple de vecteur incorporé sur le pignon:

<gigne> <img src = "../. Gitbook / Assets / Image (4) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) .png" alt = ""> <figCaption> </gigcaption> </ Figure>

Pour ce faire avec l'API, suivez ces deux étapes:

1. Utiliser le[Vector Upsert API](broken-reference)avec`formData`et`chatId`:

{% Tabs%}
{% tab title = "python"%}
```python
import requests

API_URL = "http://localhost:3000/api/v1/vector/upsert/<chatflowid>"

# Use form data to upload files
form_data = {
    "files": ("state_of_the_union.txt", open("state_of_the_union.txt", "rb"))
}

body_data = {
    "chatId": "some-session-id"
}

def query(form_data):
    response = requests.post(API_URL, files=form_data, data=body_data)
    print(response)
    return response.json()

output = query(form_data)
print(output)
```
{% endtab%}

{% tab title = "javascript"%}
```javascript
// Use FormData to upload files
let formData = new FormData();
formData.append("files", input.files[0]);
formData.append("chatId", "some-session-id");

async function query(formData) {
    const response = await fetch(
        "http://localhost:3000/api/v1/vector/upsert/<chatflowid>",
        {
            method: "POST",
            body: formData
        }
    );
    const result = await response.json();
    return result;
}

query(formData).then((response) => {
    console.log(response);
});
```
{% endtab%}
{% endtabs%}

2. Utiliser le[Prediction API](broken-reference)avec`uploads`et le`chatId`De l'étape 1:

{% Tabs%}
{% tab title = "python"%}
```python
import requests
API_URL = "http://localhost:3000/api/v1/prediction/<chatflowid>"

def query(payload):
    response = requests.post(API_URL, json=payload)
    return response.json()
    
output = query({
    "question": "What is the speech about?",
    "chatId": "same-session-id-from-step-1",
    "uploads": [
        {
            "data": "data:text/plain;base64,TWFkYWwcy4=",
            "type": "file:rag",
            "name": "state_of_the_union.txt",
            "mime": "text/plain"
        }
    ]
})
```
{% endtab%}

{% tab title = "javascript"%}
```javascript
async function query(data) {
    const response = await fetch(
        "http://localhost:3000/api/v1/prediction/<chatflowid>",
        {
            method: "POST",
            headers: {
                "Content-Type": "application/json"
            },
            body: JSON.stringify(data)
        }
    );
    const result = await response.json();
    return result;
}

query({
    "question": "What is the speech about?",
    "chatId": "same-session-id-from-step-1",
    "uploads": [
        {
            "data": "data:text/plain;base64,TWFkYWwcy4=",
            "type": "file:rag",
            "name": "state_of_the_union.txt",
            "mime": "text/plain"
        }
    ]
}).then((response) => {
    console.log(response);
});
```
{% endtab%}
{% endtabs%}

### Téléchargements de fichiers complets

Avec les téléchargements de fichiers de chiffon, vous ne pouvez pas travailler avec des données structurées comme des feuilles de calcul ou des tables, et vous ne pouvez pas effectuer une résumé complet en raison du manque de contexte complet. Dans certains cas, vous voudrez peut-être inclure tout le contenu de fichier directement dans l'invite pour un LLM, en particulier avec des modèles comme Gemini et Claude qui ont des fenêtres de contexte plus longues.[This research paper](https://arxiv.org/html/2407.16833v1)est l'un des nombreux qui comparent le chiffon avec des fenêtres de contexte plus long.

Pour activer les téléchargements de fichiers complets, accédez à ** CHATFLOW Configuration **, ouvrez l'onglet ** Fichier Upload **, puis cliquez sur le commutateur:

<gigne> <img src = "../. Gitbook / Assets / image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (2) .png" alt = ""> <figure> </1/figcaption>

Vous pouvez voir le bouton ** File Pièce ** dans le chat, où vous pouvez télécharger un ou plusieurs fichiers. Sous le capot, le[File Loader](../integrations/langchain/document-loaders/file-loader.md)traite chaque fichier et le convertit en texte.

<gigne> <img src = "../. Gitbook / Assets / image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (2) (1) .png" alt = ""> <Figcaption> </ / Figcaption>

Notez que si votre ChatFlow utilise un nœud de modèle d'invite de chat, une entrée doit être créée à partir des valeurs d'invite ** de format ** pour passer les données du fichier. Le nom d'entrée spécifié (par exemple {fichier}) doit être inclus dans le champ ** Message humain **.

<gigne> <img src = "../. GitBook / Assets / Chat-Pompt-Template-File-Aattachment.jpg" alt = ""> <Figcaption> </gigcaption> </gigust>

Pour télécharger des fichiers avec l'API:

{% Tabs%}
{% tab title = "python"%}
```python
import requests
API_URL = "http://localhost:3000/api/v1/prediction/<chatflowid>"

def query(payload):
    response = requests.post(API_URL, json=payload)
    return response.json()
    
output = query({
    "question": "What is the data about?",
    "chatId": "some-session-id",
    "uploads": [
        {
            "data": "data:text/plain;base64,TWFkYWwcy4=",
            "type": "file:full",
            "name": "state_of_the_union.txt",
            "mime": "text/plain"
        }
    ]
})
```
{% endtab%}

{% tab title = "javascript"%}
```javascript
async function query(data) {
    const response = await fetch(
        "http://localhost:3000/api/v1/prediction/<chatflowid>",
        {
            method: "POST",
            headers: {
                "Content-Type": "application/json"
            },
            body: JSON.stringify(data)
        }
    );
    const result = await response.json();
    return result;
}

query({
    "question": "What is the data about?",
    "chatId": "some-session-id",
    "uploads": [
        {
            "data": "data:text/plain;base64,TWFkYWwcy4=",
            "type": "file:full",
            "name": "state_of_the_union.txt",
            "mime": "text/plain"
        }
    ]
}).then((response) => {
    console.log(response);
});
```
{% endtab%}
{% endtabs%}

Comme vous pouvez le voir dans les exemples, les téléchargements nécessitent une chaîne Base64. Pour obtenir une chaîne Base64 pour un fichier, utilisez le[Create Attachments API](../api-reference/attachments.md).

### Différence entre les téléchargements complets et de chiffon

Les téléchargements de fichiers Full et RAG (RETRIEVAL-AUGMED) servent des objectifs différents.

* ** Téléchargement complet du fichier **: Cette méthode analyse l'ensemble du fichier dans une chaîne et l'envoie au LLM (modèle de grande langue). Il est bénéfique pour résumer le document ou extraire des informations clés. Cependant, avec des fichiers très volumineux, le modèle peut produire des résultats inexacts ou des «hallucinations» en raison de limitations de jetons.
* ** Rag Fichier Upload **: Recommandé si vous visez à réduire les coûts de jetons en n'envoyant pas l'intégralité du texte au LLM. Cette approche convient aux tâches Q \ & A sur les documents, mais n'est pas idéale pour le résumé car il n'a pas le contexte complet du document. Cette approche peut prendre plus de temps en raison du processus Upsert.
