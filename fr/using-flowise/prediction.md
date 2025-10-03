# Prediction

Prediction API is the primary endpoint for interacting with your Flowise flows and assistants. It allows you to send messages to your selected flow and receive responses back. This API handles the core chat functionality, including:

* **Chat Interactions**: Send questions or messages to your flow and receive AI-generated responses
* **Streaming Responses**: Get real-time streaming responses for better user experience
* **Conversation Memory**: Maintain context across multiple messages within a session
* **File Processing**: Upload and process images, audio, and other files as part of your queries
* **Dynamic Configuration**: Override chatflow settings and pass variables at runtime

For details, see the [Prediction Endpoint API Reference](../api-reference/prediction.md).

## Base URL and Authentication

**Base URL**: `http://localhost:3000` (or your Flowise instance URL)

**Endpoint**: `POST /api/v1/prediction/:id`

**Authentication**: Refer [Authentication for Flows](../configuration/authorization/chatflow-level.md)

## Request Format

#### Basic Request Structure

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

#### Parameters

| Parameter        | Type    | Required                    | Description                                 |
| ---------------- | ------- | --------------------------- | ------------------------------------------- |
| `question`       | string  | Yes                         | The message/question to send to the flow    |
| `form`           | object  | Either `question` or `form` | The form object to send to the flow         |
| `streaming`      | boolean | No                          | Enable streaming responses (default: false) |
| `overrideConfig` | object  | No                          | Override flow configuration                 |
| `history`        | array   | No                          | Previous conversation messages              |
| `uploads`        | array   | No                          | Files to upload (images, audio, etc.)       |
| `humanInput`     | object  | No                          | Return human feedback and resume execution  |

## SDK Libraries

Flowise provides official SDKs for Python and TypeScript/JavaScript:

#### Installation

**Python**: `pip install flowise`

**TypeScript/JavaScript**: `npm install flowise-sdk`

#### Python SDK Usage

{% tabs %}
{% tab title="Basic Usage" %}
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
{% endtab %}

{% tab title="Streaming" %}
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
{% endtab %}

{% tab title="With Configuration" %}
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
{% endtab %}
{% endtabs %}

#### TypeScript/JavaScript SDK Usage

{% tabs %}
{% tab title="Basic Usage" %}
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
{% endtab %}

{% tab title="Streaming" %}
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
{% endtab %}

{% tab title="With Configuration" %}
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
{% endtab %}
{% endtabs %}

## Direct HTTP API Usage

If you prefer to use the REST API directly without SDKs:

#### Basic Request

{% tabs %}
{% tab title="Python (requests)" %}
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
{% endtab %}

{% tab title="JavaScript (fetch)" %}
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
{% endtab %}

{% tab title="cURL" %}
```bash
curl -X POST "http://localhost:3000/api/v1/prediction/your-chatflow-id" \
  -H "Content-Type: application/json" \
  -d '{
    "question": "What is artificial intelligence?",
    "streaming": false
  }'
```
{% endtab %}
{% endtabs %}

## Advanced Features

### Form Input

In Agentflow V2, you can select `form` as input type.

<figure><img src="../.gitbook/assets/image (333).png" alt="" width="418"><figcaption></figcaption></figure>

You can override the value by Variable Name of the Form Input

```json
{
    "form": {
        "title": "Example",
        "count": 1,
        ...
    }
}
```

{% tabs %}
{% tab title="Python" %}
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
{% endtab %}

{% tab title="JavaScript" %}
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
{% endtab %}
{% endtabs %}

### Configuration Override

Override chatflow settings dynamically.

Override config is **disabled** by default for security reasons. Enable it from the top right: **Settings** → **Configuration** → **Security** tab:

<div align="right" data-full-width="false"><figure><img src="../.gitbook/assets/image (21).png" alt=""><figcaption></figcaption></figure></div>

{% tabs %}
{% tab title="Python" %}
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
{% endtab %}

{% tab title="JavaScript" %}
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
{% endtab %}
{% endtabs %}

For `array` type, hovering over the info icon will shows the schema that can be overriden.

Array value from overrideConfig will concatenate with existing array values. For example, if existing `startState` has:

```json
{
  "key": "key1",
  "value": "value1"
}
```

And if we enable override:

<figure><img src="../.gitbook/assets/image (337).png" alt=""><figcaption></figcaption></figure>

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

The final `startState` will be:

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

### Overriding Specific Node

By default, if multiple nodes share the same type and no node ID is specified, overriding a property will update that property across all matching nodes.

For example, there are 2 LLM nodes where I want to override the system message:

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

After enabling the ability to override:

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

I can override the system message for both LLMs like so:

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

From the Execution, you can see the overriden system message:

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

In some cases, you might want to just override config for specific node. You can do so by specifying the node id **inside** the property you want to override.

For example:

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

If you head back to Execution, you can see each LLM has the correct overriden value:

<figure><img src="../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

### Conversation History

Provide conversation context by including previous messages in the history array.

**History Message Format**

```json
{
    "role": "apiMessage" | "userMessage",
    "content": "Message content"
}
```

{% tabs %}
{% tab title="Python" %}
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
{% endtab %}

{% tab title="JavaScript" %}
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
{% endtab %}
{% endtabs %}

### Session Management

Use `sessionId` to maintain conversation state across multiple API calls. Each session maintains its own conversation context and memory.

{% tabs %}
{% tab title="Python" %}
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
{% endtab %}

{% tab title="JavaScript" %}
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
{% endtab %}
{% endtabs %}

### Variables

Pass dynamic variables to your flow using the `vars` property in `overrideConfig`. Variables can be used in your flow to inject dynamic content.

{% hint style="warning" %}
Variables must be created first before you can override it. Refer to [Variables](variables.md)
{% endhint %}

{% tabs %}
{% tab title="Python" %}
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
{% endtab %}

{% tab title="JavaScript" %}
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
{% endtab %}
{% endtabs %}

### Image Uploads

Upload images for visual analysis when your flow supports image processing. Refer to [Image](uploads.md#image) for more reference.

**Upload Structure:**

```json
{
    "data": "", 
    "type": "",
    "name": ",
    "mime": "
}
```

**Data:** Base64 or URL of an image

**Type**: `url` or `file`

**Name:** name of the image

**Mime**: `image/png`, `image/jpeg`, `image/jpg`

{% tabs %}
{% tab title="Python (Base64)" %}
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
{% endtab %}

{% tab title="Python (URL)" %}
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
{% endtab %}

{% tab title="JavaScript (File Upload)" %}
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
{% endtab %}

{% tab title="JavaScript (URL)" %}
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
{% endtab %}

{% tab title="JavaScript (Node.js)" %}
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
{% endtab %}
{% endtabs %}

### Audio Uploads (Speech to Text)

Upload audio files for speech-to-text processing. Refer to [Audio](uploads.md#audio) for more reference.

**Upload Structure:**

```json
{
    "data": "", 
    "type": "",
    "name": ",
    "mime": "
}
```

**Data:** Base64 or URL of an audio

**Type**: `url` or `file`

**Name:** name of the audio

**Mime**: `audio/mp4`, `audio/webm`, `audio/wav`, `audio/mpeg`

{% tabs %}
{% tab title="Python (Base64)" %}
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
{% endtab %}

{% tab title="Python (URL)" %}
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
{% endtab %}

{% tab title="JavaScript (File Upload)" %}
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
{% endtab %}

{% tab title="JavaScript (URL)" %}
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
{% endtab %}
{% endtabs %}

### File Uploads

Upload files to have LLM process the files and answer query related to the files. Refer to [Files](uploads.md#files) for more reference.

### Human Input

To resume the execution from a previously stopped checkpoint, `humanInput` needs to be provided. Refer [Human In The Loop](../tutorials/human-in-the-loop.md) for details.

**Human Input Structure**

```json
{
    "type": "",
    "feedback": ""
}
```

* **type**: Either `proceed` or `reject`
* **feedback**: Feedback to the last output

{% hint style="warning" %}
Must specify the same `sessionId` where the execution was stopped
{% endhint %}

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

## Troubleshooting

1. **404 Not Found**: Verify the flow ID is correct and the flow exists
2. **401 Unauthorized Access**: Verify if the flow is API key protected
3. **400 Bad Request**: Check request format and required fields
4. **413 Payload Too Large**: Reduce file sizes or split large requests
5. **500 Internal Server Error:** Check if there is any misconfiguration from the nodes in the flow
