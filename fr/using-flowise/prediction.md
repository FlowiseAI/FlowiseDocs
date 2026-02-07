# Prédiction

L'API de prédiction est le principal critère d'évaluation pour interagir avec vos flux et assistants fluide. Il vous permet d'envoyer des messages à votre flux sélectionné et de recevoir des réponses. Cette API gère la fonctionnalité de chat de base, notamment:

* ** Interactions de chat **: Envoyez des questions ou des messages à votre flux et recevez des réponses générées par l'AI-AI
* ** Réponses en streaming **: Obtenez des réponses en streaming en temps réel pour une meilleure expérience utilisateur
* ** Mémoire de conversation **: Maintenir le contexte sur plusieurs messages d'une session
* ** Traitement de fichiers **: Télécharger et traiter les images, l'audio et autres fichiers dans le cadre de vos requêtes
* ** Configuration dynamique **: remplacer les paramètres de chat et les variables de passes à l'exécution

Pour plus de détails, voir le[Prediction Endpoint API Reference](../api-reference/prediction.md).

## URL de base et authentification

** URL de base **:`http://localhost:3000`(ou votre URL d'instance fluide)

** Point de terminaison **:`POST /api/v1/prediction/:id`

** Authentification **: se référer[Authentication for Flows](../configuration/authorization/chatflow-level.md)

## Demander le format

#### Structure de base de la demande

```json
{
    "question": "Your message here",
    "streaming": false,
    "overrideConfig": {},
    "history": [],
    "uploads": [],
    "form": {}
}
```

#### Paramètres

| Paramètre | Type | Requis | Description |
| ---------------- | ------- | --------------------------- | ------------------------------------------- |
| `question`| String | Oui | Le message / question à envoyer au flux |
| `form`| Objet | Soit`question`ou`form`| L'objet formulaire à envoyer à l'écoulement |
| `streaming`| booléen | Non | Activer les réponses en streaming (par défaut: false) |
| `overrideConfig`| Objet | Non | Remplacez la configuration du flux |
| `history`| tableau | Non | Messages de conversation précédents |
| `uploads`| tableau | Non | Fichiers à télécharger (images, audio, etc.) |
| `humanInput`| Objet | Non | Renvoie les commentaires humains et le curriculum vitae |

## Bibliothèques SDK

Flowise fournit des SDK officiels pour Python et TypeScript / JavaScript:

#### Installation

**Python**:`pip install flowise`

** TypeScript / JavaScript **:`npm install flowise-sdk`

#### Utilisation du SDK Python

{% Tabs%}
{% tab title = "USAGE BASIC"%}
```python
from flowise import Flowise, PredictionData

# Initialize client
client = Flowise(base_url="http://localhost:3000")

# Non-streaming prediction
try:
    response = client.create_prediction(
        PredictionData(
            chatflowId="your-chatflow-id",
            question="What is machine learning?",
            streaming=False
        )
    )
    
    # Handle response
    for result in response:
        print("Response:", result)
        
except Exception as e:
    print(f"Error: {e}")
```
{% endtab%}

{% tab title = "streaming"%}
```python
from flowise import Flowise, PredictionData

client = Flowise(base_url="http://localhost:3000")

# Streaming prediction
try:
    response = client.create_prediction(
        PredictionData(
            chatflowId="your-chatflow-id",
            question="Tell me a long story about AI",
            streaming=True
        )
    )
    
    # Process streaming chunks
    print("Streaming response:")
    for chunk in response:
        print(chunk, end="", flush=True)
        
except Exception as e:
    print(f"Error: {e}")
```
{% endtab%}

{% tab title = "avec configuration"%}
```python
from flowise import Flowise, PredictionData

client = Flowise(base_url="http://localhost:3000")

# Advanced configuration
try:
    response = client.create_prediction(
        PredictionData(
            chatflowId="your-chatflow-id",
            question="Analyze this data",
            streaming=False,
            overrideConfig={
                "sessionId": "user-session-123",
                "temperature": 0.7,
                "maxTokens": 500,
                "returnSourceDocuments": True
            }
        )
    )
    
    for result in response:
        print("Response:", result)
        
except Exception as e:
    print(f"Error: {e}")
```
{% endtab%}
{% endtabs%}

#### Utilisation du SDK TypeScript / JavaScript

{% Tabs%}
{% tab title = "USAGE BASIC"%}
```typescript
import { FlowiseClient } from 'flowise-sdk';

// Initialize client
const client = new FlowiseClient({ 
    baseUrl: 'http://localhost:3000' 
});

// Non-streaming prediction
async function chatWithFlow() {
    try {
        const response = await client.createPrediction({
            chatflowId: 'your-chatflow-id',
            question: 'What is machine learning?',
            streaming: false
        });
        
        console.log('Response:', response);
        
    } catch (error) {
        console.error('Error:', error);
    }
}

chatWithFlow();
```
{% endtab%}

{% tab title = "streaming"%}
```typescript
import { FlowiseClient } from 'flowise-sdk';

const client = new FlowiseClient({ 
    baseUrl: 'http://localhost:3000' 
});

// Streaming prediction
async function streamingChat() {
    try {
        const stream = await client.createPrediction({
            chatflowId: 'your-chatflow-id',
            question: 'Tell me a long story about AI',
            streaming: true
        });
        
        console.log('Streaming response:');
        for await (const chunk of stream) {
            process.stdout.write(chunk);
        }
        
    } catch (error) {
        console.error('Error:', error);
    }
}

streamingChat();
```
{% endtab%}

{% tab title = "avec configuration"%}
```typescript
import { FlowiseClient } from 'flowise-sdk';

const client = new FlowiseClient({ 
    baseUrl: 'http://localhost:3000' 
});

// Advanced configuration
async function advancedChat() {
    try {
        const response = await client.createPrediction({
            chatflowId: 'your-chatflow-id',
            question: 'Analyze this data',
            streaming: false,
            overrideConfig: {
                sessionId: 'user-session-123',
                temperature: 0.7,
                maxTokens: 500,
                returnSourceDocuments: true
            }
        });
        
        console.log('Response:', response);
        
    } catch (error) {
        console.error('Error:', error);
    }
}

advancedChat();
```
{% endtab%}
{% endtabs%}

## Utilisation directe de l'API HTTP

Si vous préférez utiliser l'API REST directement sans SDKS:

#### Demande de base

{% Tabs%}
{% tab title = "python (requêtes)"%}
```python
import requests
import json

def send_message(chatflow_id, question, streaming=False):
    url = f"http://localhost:3000/api/v1/prediction/{chatflow_id}"
    
    payload = {
        "question": question,
        "streaming": streaming
    }
    
    headers = {
        "Content-Type": "application/json"
    }
    
    try:
        response = requests.post(url, json=payload, headers=headers)
        response.raise_for_status()  # Raise exception for bad status codes
        
        return response.json()
        
    except requests.exceptions.RequestException as e:
        print(f"Request failed: {e}")
        return None

# Usage
result = send_message(
    chatflow_id="your-chatflow-id",
    question="What is artificial intelligence?",
    streaming=False
)

if result:
    print("Response:", result)
```
{% endtab%}

{% tab title = "javascript (fetch)"%}
```javascript
async function sendMessage(chatflowId, question, streaming = false) {
    const url = `http://localhost:3000/api/v1/prediction/${chatflowId}`;
    
    const payload = {
        question: question,
        streaming: streaming
    };
    
    try {
        const response = await fetch(url, {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json',
            },
            body: JSON.stringify(payload)
        });
        
        if (!response.ok) {
            throw new Error(`HTTP error! status: ${response.status}`);
        }
        
        const result = await response.json();
        return result;
        
    } catch (error) {
        console.error('Request failed:', error);
        return null;
    }
}

// Usage
sendMessage(
    'your-chatflow-id',
    'What is artificial intelligence?',
    false
).then(result => {
    if (result) {
        console.log('Response:', result);
    }
});
```
{% endtab%}

{% tab title = "curl"%}
```bash
curl -X POST "http://localhost:3000/api/v1/prediction/your-chatflow-id" \
  -H "Content-Type: application/json" \
  -d '{
    "question": "What is artificial intelligence?",
    "streaming": false
  }'
```
{% endtab%}
{% endtabs%}

## Fonctionnalités avancées

### Entrée de formulaire

Dans AgentFlow v2, vous pouvez sélectionner`form`comme type d'entrée.

<gigne> <img src = "../. GitBook / Assets / Image (333) .png" alt = "" width = "418"> <Figcaption> </ Figcaption> </gigne>

Vous pouvez remplacer la valeur par nom de variable de l'entrée de formulaire

```json
{
    "form": {
        "title": "Example",
        "count": 1,
        ...
    }
}
```

{% Tabs%}
{% tab title = "python"%}
```python
import requests

def prediction(flow_id, form):
    url = f"http://localhost:3000/api/v1/prediction/{flow_id}"
    
    payload = {
        "form": form
    }
    
    try:
        response = requests.post(url, json=payload)
        response.raise_for_status()
        return response.json()
    except requests.exceptions.RequestException as e:
        print(f"Error: {e}")
        return None

result = prediction(
    flow_id="your-flow-id",
    form={
        "title": "ABC",
        "choices": "A"
    }
)

print(result)
```
{% endtab%}

{% tab title = "javascript"%}
```javascript
async function prediction(flowId, form) {
    const url = `http://localhost:3000/api/v1/prediction/${flowId}`;
    
    const payload = {
        form: form
    };
    
    try {
        const response = await fetch(url, {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json',
            },
            body: JSON.stringify(payload)
        });
        
        if (!response.ok) {
            throw new Error(`HTTP error! status: ${response.status}`);
        }
        
        return await response.json();
        
    } catch (error) {
        console.error('Error:', error);
        return null;
    }
}

prediction(
    'your-flow-id',
    {
        "title": "ABC",
        "choices": "A"
    }
).then(result => {
    console.log(result);
});
```
{% endtab%}
{% endtabs%}

### Remplacement de la configuration

Remplacez dynamiquement les paramètres de ChatFlow.

La configuration de remplacement est ** désactivée ** Par défaut pour des raisons de sécurité. Activez-le en haut à droite: ** Paramètres ** → ** Configuration ** → ** Sécurité ** Tab:

<div align = "droite" data-ull-width = "false"> <gigne> <img src = "../. gitbook / actifs / image (21) .png" alt = ""> <figcaption> </gigcaption> </ fig> </div>

{% Tabs%}
{% tab title = "python"%}
```python
import requests

def query_with_config(flow_id, question, config):
    url = f"http://localhost:3000/api/v1/prediction/{flow_id}"
    
    payload = {
        "question": question,
        "overrideConfig": config
    }
    
    try:
        response = requests.post(url, json=payload)
        response.raise_for_status()
        return response.json()
    except requests.exceptions.RequestException as e:
        print(f"Error: {e}")
        return None

# Example: Override session and return source documents
result = query_with_config(
    flow_id="your-flow-id",
    question="How does machine learning work?",
    config={
        "sessionId": "user-123",
        "temperature": 0.5,
        "maxTokens": 1000
    }
)

print(result)
```
{% endtab%}

{% tab title = "javascript"%}
```javascript
async function queryWithConfig(flowId, question, config) {
    const url = `http://localhost:3000/api/v1/prediction/${flowId}`;
    
    const payload = {
        question: question,
        overrideConfig: config
    };
    
    try {
        const response = await fetch(url, {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json',
            },
            body: JSON.stringify(payload)
        });
        
        if (!response.ok) {
            throw new Error(`HTTP error! status: ${response.status}`);
        }
        
        return await response.json();
        
    } catch (error) {
        console.error('Error:', error);
        return null;
    }
}

// Example: Override session and return source documents
queryWithConfig(
    'your-flow-id',
    'How does machine learning work?',
    {
        sessionId: 'user-123',
        temperature: 0.5,
        maxTokens: 1000
    }
).then(result => {
    console.log(result);
});
```
{% endtab%}
{% endtabs%}

Pour`array`Type, survol de l'icône Info affichera le schéma qui peut être remplacé.

La valeur du tableau de OverRideConfig se concatera avec les valeurs de tableau existantes. Par exemple, si existant`startState`a:

```json
{
  "key": "key1",
  "value": "value1"
}
```

Et si nous permettons de remplacer:

<gigne> <img src = "../. GitBook / Assets / Image (337) .png" alt = ""> <figcaption> </gigcaption> </gigust>

```json
"overrideConfig": {
    "startState": [
        {
            "key": "foo",
            "value": "bar"
        }
    ],
    "llmMessages": [
        {
            "role": "system",
            "content": "You are helpful assistant"
        }
    ]
}
```

La finale`startState`sera:

```json
[
  {
    "key": "key1",
    "value": "value1"
  },
  {
    "key": "foo",
    "value": "bar"
  },
]
```

### Overrifier le nœud spécifique

Par défaut, si plusieurs nœuds partagent le même type et qu'aucun ID de nœud n'est spécifié, le remplacement d'une propriété mettra à jour cette propriété sur tous les nœuds correspondants.

Par exemple, il y a 2 nœuds LLM où je souhaite remplacer le message système:

<gigne> <img src = "../. GitBook / Assets / Image (3) .png" alt = ""> <Figcaption> </gigcaption> </gigust>

Après avoir permis la possibilité de remplacer:

<gigne> <img src = "../. GitBook / Assets / Image (1) .png" alt = ""> <Figcaption> </gigcaption> </gigne>

Je peux remplacer le message système pour les deux LLM comme ainsi:

```json
"overrideConfig": {
    "llmMessages": [
        {
            "role": "system",
            "content": "You are sarcastic"
        }
    ]
}
```

Depuis l'exécution, vous pouvez voir le message du système Overriden:

<gigne> <img src = "../. GitBook / Assets / Image (4) .png" alt = ""> <Figcaption> </gigcaption> </gigust>

<gigne> <img src = "../. GitBook / Assets / Image (5) .png" alt = ""> <Figcaption> </gigcaption> </gigust>

Dans certains cas, vous voudrez peut-être simplement remplacer la configuration pour un nœud spécifique. Vous pouvez le faire en spécifiant l'ID de nœud ** à l'intérieur ** la propriété que vous souhaitez remplacer.

Par exemple:

```json
"overrideConfig": {
    "llmMessages": {
        "llmAgentflow_0": [
            {
                "role": "system",
                "content": "You are sweet"
            } 
        ],
        "llmAgentflow_1": [
            {
                "role": "system",
                "content": "You are smart"
            } 
        ]
    }
}
```

Si vous retournez à l'exécution, vous pouvez voir que chaque LLM a la valeur de dépassement correcte:

<gigne> <img src = "../. GitBook / Assets / Image (6) .png" alt = ""> <Figcaption> </gigcaption> </gigust>

<gigne> <img src = "../. GitBook / Assets / Image (7) .png" alt = ""> <Figcaption> </gigcaption> </gigust>

### Histoire de la conversation

Fournir un contexte de conversation en incluant des messages précédents dans le tableau d'historique.

** Format de message d'histoire **

```json
{
    "role": "apiMessage" | "userMessage",
    "content": "Message content"
}
```

{% Tabs%}
{% tab title = "python"%}
```python
import requests

def chat_with_history(flow_id, question, history):
    url = f"http://localhost:3000/api/v1/prediction/{flow_id}"
    
    payload = {
        "question": question,
        "history": history
    }
    
    try:
        response = requests.post(url, json=payload)
        response.raise_for_status()
        return response.json()
    except requests.exceptions.RequestException as e:
        print(f"Error: {e}")
        return None

# Example conversation with context
conversation_history = [
    {
        "role": "apiMessage",
        "content": "Hello! I'm an AI assistant. How can I help you today?"
    },
    {
        "role": "userMessage", 
        "content": "Hi, my name is Sarah and I'm learning about AI"
    },
    {
        "role": "apiMessage",
        "content": "Nice to meet you, Sarah! I'd be happy to help you learn about AI. What specific aspects interest you?"
    }
]

result = chat_with_history(
    flow_id="your-flow-id",
    question="Can you explain neural networks in simple terms?",
    history=conversation_history
)

print(result)
```
{% endtab%}

{% tab title = "javascript"%}
```javascript
async function chatWithHistory(flowId, question, history) {
    const url = `http://localhost:3000/api/v1/prediction/${flowId}`;
    
    const payload = {
        question: question,
        history: history
    };
    
    try {
        const response = await fetch(url, {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json',
            },
            body: JSON.stringify(payload)
        });
        
        if (!response.ok) {
            throw new Error(`HTTP error! status: ${response.status}`);
        }
        
        return await response.json();
        
    } catch (error) {
        console.error('Error:', error);
        return null;
    }
}

// Example conversation with context
const conversationHistory = [
    {
        role: "apiMessage",
        content: "Hello! I'm an AI assistant. How can I help you today?"
    },
    {
        role: "userMessage", 
        content: "Hi, my name is Sarah and I'm learning about AI"
    },
    {
        role: "apiMessage",
        content: "Nice to meet you, Sarah! I'd be happy to help you learn about AI. What specific aspects interest you?"
    }
];

chatWithHistory(
    'your-flow-id',
    'Can you explain neural networks in simple terms?',
    conversationHistory
).then(result => {
    console.log(result);
});
```
{% endtab%}
{% endtabs%}

### Gestion de session

Utiliser`sessionId`Pour maintenir l'état de conversation sur plusieurs appels d'API. Chaque session maintient son propre contexte de conversation et sa mémoire.

{% Tabs%}
{% tab title = "python"%}
```python
import requests

class FlowiseSession:
    def __init__(self, flow_id, session_id, base_url="http://localhost:3000"):
        self.flow_id= flow_id
        self.session_id = session_id
        self.base_url = base_url
        self.url = f"{base_url}/api/v1/prediction/{flow_id}"
    
    def send_message(self, question, **kwargs):
        payload = {
            "question": question,
            "overrideConfig": {
                "sessionId": self.session_id,
                **kwargs.get("overrideConfig", {})
            }
        }
        
        # Add any additional parameters
        for key, value in kwargs.items():
            if key != "overrideConfig":
                payload[key] = value
        
        try:
            response = requests.post(self.url, json=payload)
            response.raise_for_status()
            return response.json()
        except requests.exceptions.RequestException as e:
            print(f"Error: {e}")
            return None

# Usage
session = FlowiseSession(
    flow_id="your-flow-id",
    session_id="user-session-123"
)

# First message
response1 = session.send_message("Hello, my name is John")
print("Response 1:", response1)

# Second message - will remember the previous context
response2 = session.send_message("What's my name?")
print("Response 2:", response2)
```
{% endtab%}

{% tab title = "javascript"%}
```javascript
class FlowiseSession {
    constructor(flowId, sessionId, baseUrl = 'http://localhost:3000') {
        this.flowId= flowId;
        this.sessionId = sessionId;
        this.baseUrl = baseUrl;
        this.url = `${baseUrl}/api/v1/prediction/${flowId}`;
    }
    
    async sendMessage(question) {
        const payload = {
            question: question,
            overrideConfig: {
                sessionId: this.sessionId
            }
        };
  
        try {
            const response = await fetch(this.url, {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json',
                },
                body: JSON.stringify(payload)
            });
            
            if (!response.ok) {
                throw new Error(`HTTP error! status: ${response.status}`);
            }
            
            return await response.json();
            
        } catch (error) {
            console.error('Error:', error);
            return null;
        }
    }
}

// Usage
const session = new FlowiseSession(
    'your-flow-id',
    'user-session-123'
);

async function conversationExample() {
    // First message
    const response1 = await session.sendMessage("Hello, my name is John");
    console.log("Response 1:", response1);
    
    // Second message - will remember the previous context
    const response2 = await session.sendMessage("What's my name?");
    console.log("Response 2:", response2);
}

conversationExample();
```
{% endtab%}
{% endtabs%}

### Variables

Passer des variables dynamiques à votre flux en utilisant le`vars`propriété`overrideConfig`. Les variables peuvent être utilisées dans votre flux pour injecter un contenu dynamique.

{% hint style = "avertissement"%}
Les variables doivent être créées avant de pouvoir la remplacer. Se référer à[Variables](variables.md)
{% EndHint%}

{% Tabs%}
{% tab title = "python"%}
```python
import requests

def send_with_variables(flow_id, question, variables):
    url = f"http://localhost:3000/api/v1/prediction/{flow_id}"
    
    payload = {
        "question": question,
        "overrideConfig": {
            "vars": variables
        }
    }
    
    try:
        response = requests.post(url, json=payload)
        response.raise_for_status()
        return response.json()
    except requests.exceptions.RequestException as e:
        print(f"Error: {e}")
        return None

# Example: Pass user information and preferences
result = send_with_variables(
    flow_id="your-flow-id",
    question="Create a personalized workout plan",
    variables={
        "user_name": "Alice",
        "fitness_level": "intermediate",
        "preferred_duration": "30 minutes",
        "equipment": "dumbbells, resistance bands",
        "goals": "strength training, flexibility"
    }
)

print(result)
```
{% endtab%}

{% tab title = "javascript"%}
```javascript
async function sendWithVariables(flowId, question, variables) {
    const url = `http://localhost:3000/api/v1/prediction/${flowId}`;
    
    const payload = {
        question: question,
        overrideConfig: {
            vars: variables
        }
    };
    
    try {
        const response = await fetch(url, {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json',
            },
            body: JSON.stringify(payload)
        });
        
        if (!response.ok) {
            throw new Error(`HTTP error! status: ${response.status}`);
        }
        
        return await response.json();
        
    } catch (error) {
        console.error('Error:', error);
        return null;
    }
}

// Example: Pass user information and preferences
sendWithVariables(
    'your-flow-id',
    'Create a personalized workout plan',
    {
        user_name: 'Alice',
        fitness_level: 'intermediate',
        preferred_duration: '30 minutes',
        equipment: 'dumbbells, resistance bands',
        goals: 'strength training, flexibility'
    }
).then(result => {
    console.log(result);
});
```
{% endtab%}
{% endtabs%}

### Téléchargements d'images

Téléchargez des images pour une analyse visuelle lorsque votre flux prend en charge le traitement d'image. Se référer à[Image](uploads.md#image)Pour plus de référence.

** Structure de téléchargement: **

```json
{
    "data": "", 
    "type": "",
    "name": ",
    "mime": "
}
```

** Données: ** base64 ou URL d'une image

**Taper**:`url`ou`file`

** Nom: ** Nom de l'image

**Mime**:`image/png`, `image/jpeg`, `image/jpg`

{% Tabs%}
{% Tab Title = "Python (base64)"%}
```python
import requests
import base64
import os

def upload_image(flow_id, question, image_path):
    # Read and encode image
    with open(image_path, 'rb') as image_file:
        encoded_image = base64.b64encode(image_file.read()).decode('utf-8')
    
    # Determine MIME type based on file extension
    mime_types = {
        '.png': 'image/png',
        '.jpg': 'image/jpeg',
        '.jpeg': 'image/jpeg',
        '.gif': 'image/gif',
        '.webp': 'image/webp'
    }

    file_ext = os.path.splitext(image_path)[1].lower()
    mime_type = mime_types.get(file_ext, 'image/png')
    
    url = f"http://localhost:3000/api/v1/prediction/{flow_id}"
    
    payload = {
        "question": question,
        "uploads": [
            {
                "data": f"data:{mime_type};base64,{encoded_image}",
                "type": "file",
                "name": os.path.basename(image_path),
                "mime": mime_type
            }
        ]
    }
    
    try:
        response = requests.post(url, json=payload)
        response.raise_for_status()
        return response.json()
    except requests.exceptions.RequestException as e:
        print(f"Error: {e}")
        return None

# Example usage
result = upload_image(
    flow_id="your-flow-id",
    question="Can you describe what you see in this image?",
    image_path="path/to/your/image.png"
)

print(result)
```
{% endtab%}

{% Tab Title = "Python (URL)"%}
```python
import requests
import os

def upload_image_url(flow_id, question, image_url, image_name=None):
    """
    Upload an image using a URL instead of base64 encoding.
    This is more efficient for images that are already hosted online.
    """
    url = f"http://localhost:3000/api/v1/prediction/{flow_id}"
    
    # Extract filename from URL if not provided
    if not image_name:
        image_name = image_url.split('/')[-1]
        if '?' in image_name:
            image_name = image_name.split('?')[0]
    
    # Determine MIME type from URL extension
    mime_types = {
        '.png': 'image/png',
        '.jpg': 'image/jpeg',
        '.jpeg': 'image/jpeg',
        '.gif': 'image/gif',
        '.webp': 'image/webp'
    }
    
    file_ext = os.path.splitext(image_name)[1].lower()
    mime_type = mime_types.get(file_ext, 'image/jpeg')
    
    payload = {
        "question": question,
        "uploads": [
            {
                "data": image_url,
                "type": "url",
                "name": image_name,
                "mime": mime_type
            }
        ]
    }
    
    try:
        response = requests.post(url, json=payload)
        response.raise_for_status()
        return response.json()
    except requests.exceptions.RequestException as e:
        print(f"Error: {e}")
        return None

# Example usage with public image URL
result = upload_image_url(
    flow_id="your-flow-id",
    question="What's in this image? Analyze it in detail.",
    image_url="https://example.com/path/to/image.jpg",
    image_name="example_image.jpg"
)

print(result)

# Example with direct URL (no custom name)
result2 = upload_image_url(
    chatflow_id="your-chatflow-id",
    question="Describe this screenshot",
    image_url="https://i.imgur.com/sample.png"
)

print(result2)
```
{% endtab%}

{% tab title = "JavaScript (Fichier Téléchargement)"%}
```javascript
async function uploadImage(flowId, question, imageFile) {
    return new Promise((resolve, reject) => {
        const reader = new FileReader();
        
        reader.onload = async function(e) {
            const base64Data = e.target.result;
            
            const payload = {
                question: question,
                uploads: [
                    {
                        data: base64Data,
                        type: 'file',
                        name: imageFile.name,
                        mime: imageFile.type
                    }
                ]
            };
            
            try {
                const response = await fetch(`http://localhost:3000/api/v1/prediction/${flowId}`, {
                    method: 'POST',
                    headers: {
                        'Content-Type': 'application/json',
                    },
                    body: JSON.stringify(payload)
                });
                
                if (!response.ok) {
                    throw new Error(`HTTP error! status: ${response.status}`);
                }
                
                const result = await response.json();
                resolve(result);
                
            } catch (error) {
                reject(error);
            }
        };
        
        reader.onerror = function() {
            reject(new Error('Failed to read file'));
        };
        
        reader.readAsDataURL(imageFile);
    });
}

// Example usage in browser
document.getElementById('imageInput').addEventListener('change', async function(e) {
    const file = e.target.files[0];
    if (file) {
        try {
            const result = await uploadImage(
                'your-flow-id',
                'Can you describe what you see in this image?',
                file
            );
            console.log('Analysis result:', result);
        } catch (error) {
            console.error('Upload failed:', error);
        }
    }
});
```
{% endtab%}

{% tab title = "javascript (url)"%}
```javascript
async function uploadImageUrl(flowId, question, imageUrl, imageName = null) {
    /**
     * Upload an image using a URL instead of base64 encoding.
     * This is more efficient for images that are already hosted online.
     */
    
    // Extract filename from URL if not provided
    if (!imageName) {
        imageName = imageUrl.split('/').pop();
        if (imageName.includes('?')) {
            imageName = imageName.split('?')[0];
        }
    }
    
    // Determine MIME type from URL extension
    const mimeTypes = {
        '.png': 'image/png',
        '.jpg': 'image/jpeg',
        '.jpeg': 'image/jpeg',
        '.gif': 'image/gif',
        '.webp': 'image/webp'
    };
    
    const fileExt = imageName.toLowerCase().substring(imageName.lastIndexOf('.'));
    const mimeType = mimeTypes[fileExt] || 'image/jpeg';
    
    const payload = {
        question: question,
        uploads: [
            {
                data: imageUrl,
                type: 'url',
                name: imageName,
                mime: mimeType
            }
        ]
    };
    
    try {
        const response = await fetch(`http://localhost:3000/api/v1/prediction/${flowId}`, {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json',
            },
            body: JSON.stringify(payload)
        });
        
        if (!response.ok) {
            throw new Error(`HTTP error! status: ${response.status}`);
        }
        
        return await response.json();
        
    } catch (error) {
        console.error('Error:', error);
        return null;
    }
}

// Example usage with public image URL
async function analyzeImageFromUrl() {
    try {
        const result = await uploadImageUrl(
            'your-flow-id',
            'What is in this image? Analyze it in detail.',
            'https://example.com/path/to/image.jpg',
            'example_image.jpg'
        );
        
        console.log('Analysis result:', result);
    } catch (error) {
        console.error('Upload failed:', error);
    }
}

// Example with direct URL (no custom name)
uploadImageUrl(
    'your-flow-id',
    'Describe this screenshot',
    'https://i.imgur.com/sample.png'
).then(result => {
    if (result) {
        console.log('Analysis result:', result);
    }
});

// Example with multiple image URLs
async function analyzeMultipleImages() {
    const imageUrls = [
        'https://example.com/image1.jpg',
        'https://example.com/image2.png',
        'https://example.com/image3.gif'
    ];
    
    const results = await Promise.all(
        imageUrls.map(url => 
            uploadImageUrl(
                'your-flow-id',
                `Analyze this image: ${url}`,
                url
            )
        )
    );
    
    results.forEach((result, index) => {
        console.log(`Image ${index + 1} analysis:`, result);
    });
}
```
{% endtab%}

{% tab title = "javascript (node.js)"%}
```javascript
const fs = require('fs');
const path = require('path');

async function uploadImage(flowId, question, imagePath) {
    // Read image file
    const imageBuffer = fs.readFileSync(imagePath);
    const base64Image = imageBuffer.toString('base64');
    
    // Determine MIME type
    const ext = path.extname(imagePath).toLowerCase();
    const mimeTypes = {
        '.png': 'image/png',
        '.jpg': 'image/jpeg',
        '.jpeg': 'image/jpeg',
        '.gif': 'image/gif',
        '.webp': 'image/webp'
    };
    const mimeType = mimeTypes[ext] || 'image/png';
    
    const payload = {
        question: question,
        uploads: [
            {
                data: `data:${mimeType};base64,${base64Image}`,
                type: 'file',
                name: path.basename(imagePath),
                mime: mimeType
            }
        ]
    };
    
    try {
        const response = await fetch(`http://localhost:3000/api/v1/prediction/${flowId}`, {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json',
            },
            body: JSON.stringify(payload)
        });
        
        if (!response.ok) {
            throw new Error(`HTTP error! status: ${response.status}`);
        }
        
        return await response.json();
        
    } catch (error) {
        console.error('Error:', error);
        return null;
    }
}

// Example usage
uploadImage(
    'your-flow-id',
    'Can you describe what you see in this image?',
    'path/to/your/image.png'
).then(result => {
    console.log('Analysis result:', result);
});
```
{% endtab%}
{% endtabs%}

### Téléchargements audio (discours au texte)

Téléchargez des fichiers audio pour le traitement de la parole à texte. Se référer à[Audio](uploads.md#audio)Pour plus de référence.

** Structure de téléchargement: **

```json
{
    "data": "", 
    "type": "",
    "name": ",
    "mime": "
}
```

** Données: ** base64 ou URL d'un audio

**Taper**:`url`ou`file`

** Nom: ** Nom de l'audio

**Mime**:`audio/mp4`, `audio/webm`, `audio/wav`, `audio/mpeg`

{% Tabs%}
{% Tab Title = "Python (base64)"%}
```python
import requests
import base64
import os

def upload_audio(flow_id, audio_path, question=None):
    # Read and encode audio
    with open(audio_path, 'rb') as audio_file:
        encoded_audio = base64.b64encode(audio_file.read()).decode('utf-8')
    
    # Determine MIME type based on file extension
    mime_types = {
        '.webm': 'audio/webm',
        '.wav': 'audio/wav',
        '.mp3': 'audio/mpeg',
        '.m4a': 'audio/mp4'
    }
 
    file_ext = os.path.splitext(audio_path)[1].lower()
    mime_type = mime_types.get(file_ext, 'audio/webm')
    
    url = f"http://localhost:3000/api/v1/prediction/{flow_id}"
    
    payload = {
        "uploads": [
            {
                "data": f"data:{mime_type};base64,{encoded_audio}",
                "type": "audio",
                "name": os.path.basename(audio_path),
                "mime": mime_type
            }
        ]
    }
    
    # Add question if provided
    if question:
        payload["question"] = question
    
    try:
        response = requests.post(url, json=payload)
        response.raise_for_status()
        return response.json()
    except requests.exceptions.RequestException as e:
        print(f"Error: {e}")
        return None

# Example usage
result = upload_audio(
    flow_id="your-flow-id",
    audio_path="path/to/your/audio.wav",
    question="Please transcribe this audio and summarize the content"
)

print(result)
```
{% endtab%}

{% Tab Title = "Python (URL)"%}
```python
import requests
import os

def upload_audio_url(flow_id, audio_url, question=None, audio_name=None):
    """
    Upload an audio file using a URL instead of base64 encoding.
    This is more efficient for audio files that are already hosted online.
    """
    url = f"http://localhost:3000/api/v1/prediction/{flow_id}"
    
    # Extract filename from URL if not provided
    if not audio_name:
        audio_name = audio_url.split('/')[-1]
        if '?' in audio_name:
            audio_name = audio_name.split('?')[0]
    
    # Determine MIME type from URL extension
    mime_types = {
        '.webm': 'audio/webm',
        '.wav': 'audio/wav',
        '.mp3': 'audio/mpeg',
        '.m4a': 'audio/mp4',
        '.ogg': 'audio/ogg',
        '.aac': 'audio/aac'
    }

    file_ext = os.path.splitext(audio_name)[1].lower()
    mime_type = mime_types.get(file_ext, 'audio/wav')
    
    payload = {
        "uploads": [
            {
                "data": audio_url,
                "type": "url",
                "name": audio_name,
                "mime": mime_type
            }
        ]
    }
    
    # Add question if provided
    if question:
        payload["question"] = question
    
    try:
        response = requests.post(url, json=payload)
        response.raise_for_status()
        return response.json()
    except requests.exceptions.RequestException as e:
        print(f"Error: {e}")
        return None

# Example usage with public audio URL
result = upload_audio_url(
    flow_id="your-flow-id",
    audio_url="https://example.com/path/to/speech.mp3",
    question="Please transcribe this audio and provide a summary",
    audio_name="speech_recording.mp3"
)

print(result)

# Example with direct URL (no custom name or question)
result2 = upload_audio_url(
    flow_id="your-flow-id",
    audio_url="https://storage.googleapis.com/sample-audio/speech.wav"
)

print(result2)

# Example for meeting transcription
result3 = upload_audio_url(
    flow_id="your-flow-id",
    audio_url="https://meetings.example.com/recording-123.m4a",
    question="Transcribe this meeting recording and extract key action items and decisions made",
    audio_name="team_meeting_jan15.m4a"
)

print(result3)
```
{% endtab%}

{% tab title = "JavaScript (Fichier Téléchargement)"%}
```javascript
async function uploadAudio(flowId, audioFile, question = null) {
    return new Promise((resolve, reject) => {
        const reader = new FileReader();
        
        reader.onload = async function(e) {
            const base64Data = e.target.result;
            
            const payload = {
                uploads: [
                    {
                        data: base64Data,
                        type: 'audio',
                        name: audioFile.name,
                        mime: audioFile.type
                    }
                ]
            };
            
            // Add question if provided
            if (question) {
                payload.question = question;
            }
            
            try {
                const response = await fetch(`http://localhost:3000/api/v1/prediction/${flowId}`, {
                    method: 'POST',
                    headers: {
                        'Content-Type': 'application/json',
                    },
                    body: JSON.stringify(payload)
                });
                
                if (!response.ok) {
                    throw new Error(`HTTP error! status: ${response.status}`);
                }
                
                const result = await response.json();
                resolve(result);
                
            } catch (error) {
                reject(error);
            }
        };
        
        reader.onerror = function() {
            reject(new Error('Failed to read file'));
        };
        
        reader.readAsDataURL(audioFile);
    });
}

// Example usage with file input
document.getElementById('audioInput').addEventListener('change', async function(e) {
    const file = e.target.files[0];
    if (file) {
        try {
            const result = await uploadAudio(
                'your-flow-id',
                file,
                'Please transcribe this audio and summarize the content'
            );
            console.log('Transcription result:', result);
        } catch (error) {
            console.error('Upload failed:', error);
        }
    }
});
```
{% endtab%}

{% tab title = "javascript (url)"%}
```javascript
async function uploadAudioUrl(flowId, audioUrl, question = null, audioName = null) {
    /**
     * Upload an audio file using a URL instead of base64 encoding.
     * This is more efficient for audio files that are already hosted online.
     */
    
    // Extract filename from URL if not provided
    if (!audioName) {
        audioName = audioUrl.split('/').pop();
        if (audioName.includes('?')) {
            audioName = audioName.split('?')[0];
        }
    }
    
    // Determine MIME type from URL extension
    const mimeTypes = {
        '.webm': 'audio/webm',
        '.wav': 'audio/wav',
        '.mp3': 'audio/mpeg',
        '.m4a': 'audio/mp4',
        '.ogg': 'audio/ogg',
        '.aac': 'audio/aac'
    };
    
    const fileExt = audioName.toLowerCase().substring(audioName.lastIndexOf('.'));
    const mimeType = mimeTypes[fileExt] || 'audio/wav';
    
    const payload = {
        uploads: [
            {
                data: audioUrl,
                type: 'url',
                name: audioName,
                mime: mimeType
            }
        ]
    };
    
    // Add question if provided
    if (question) {
        payload.question = question;
    }
    
    try {
        const response = await fetch(`http://localhost:3000/api/v1/prediction/${flowId}`, {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json',
            },
            body: JSON.stringify(payload)
        });
        
        if (!response.ok) {
            throw new Error(`HTTP error! status: ${response.status}`);
        }
        
        return await response.json();
        
    } catch (error) {
        console.error('Error:', error);
        return null;
    }
}

// Example usage with public audio URL
async function transcribeAudioFromUrl() {
    try {
        const result = await uploadAudioUrl(
            'your-flow-id',
            'https://example.com/path/to/speech.mp3',
            'Please transcribe this audio and provide a summary',
            'speech_recording.mp3'
        );
        
        console.log('Transcription result:', result);
    } catch (error) {
        console.error('Upload failed:', error);
    }
}

// Example with direct URL (no custom name or question)
uploadAudioUrl(
    'your-flow-id',
    'https://storage.googleapis.com/sample-audio/speech.wav'
).then(result => {
    if (result) {
        console.log('Transcription result:', result);
    }
});

// Example for meeting transcription
uploadAudioUrl(
    'your-flow-id',
    'https://meetings.example.com/recording-123.m4a',
    'Transcribe this meeting recording and extract key action items and decisions made',
    'team_meeting_jan15.m4a'
).then(result => {
    if (result) {
        console.log('Meeting analysis:', result);
    }
});

// Example with multiple audio URLs for batch processing
async function transcribeMultipleAudios() {
    const audioUrls = [
        {
            url: 'https://example.com/interview1.wav',
            question: 'Transcribe this interview and summarize key points',
            name: 'interview_candidate_1.wav'
        },
        {
            url: 'https://example.com/interview2.mp3',
            question: 'Transcribe this interview and summarize key points',
            name: 'interview_candidate_2.mp3'
        },
        {
            url: 'https://example.com/lecture.m4a',
            question: 'Transcribe this lecture and create bullet-point notes',
            name: 'cs101_lecture.m4a'
        }
    ];
    
    const results = await Promise.all(
        audioUrls.map(audio => 
            uploadAudioUrl(
                'your-flow-id',
                audio.url,
                audio.question,
                audio.name
            )
        )
    );
    
    results.forEach((result, index) => {
        console.log(`Audio ${index + 1} transcription:`, result);
    });
}
```
{% endtab%}
{% endtabs%}

### Téléchargements de fichiers

Téléchargez des fichiers pour faire en sorte que LLM traite les fichiers et répond à la requête liée aux fichiers. Se référer à[Files](uploads.md#files)Pour plus de référence.

### Entrée humaine

Pour reprendre l'exécution à partir d'un point de contrôle précédemment arrêté,`humanInput`doit être fourni. Référer[Human In The Loop](../tutorials/human-in-the-loop.md)pour plus de détails.

** Structure d'entrée humaine **

```json
{
    "type": "",
    "feedback": ""
}
```

* ** Type **: Soit`proceed`ou`reject`
* ** Feedback **: commentaires à la dernière sortie

{% hint style = "avertissement"%}
Doit spécifier la même chose`sessionId`où l'exécution a été arrêtée
{% EndHint%}

```json
{
    "humanInput": {
        "type": "reject",
        "feedback": "Include more emoji"
    },
    "overrideConfig": {
        "sessionId": "abc"
    }
}
```

## Dépannage

1. ** 404 introuvable **: Vérifiez que l'ID de flux est correct et le flux existe
2. ** 401 Accès non autorisé **: Vérifiez si le flux est protégé par la clé API
3. ** 400 Bad Demande **: Format de demande de vérification et champs requis
4. ** 413 charge utile trop grande **: réduire les tailles de fichiers ou diviser de grandes demandes
5. ** 500 Erreur du serveur interne: ** Vérifiez s'il y a une mauvaise configuration des nœuds dans le flux
