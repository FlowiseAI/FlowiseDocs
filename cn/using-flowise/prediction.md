＃ 预言

预测 API 是与 Flowise 流和助手交互的主要端点。它允许您向所选流发送消息并接收回复。该 API 处理核心聊天功能，包括：

* **聊天互动**：向您的流程发送问题或消息并接收人工智能生成的回复
* **流式输出响应**：获取实时流式输出响应以获得更好的用户体验
* **对话记忆**：维护会话中多个消息的上下文
* **文件处理**：作为查询的一部分上传和处理图像、音频和其他文件
* **动态配置**：覆盖聊天流设置并在运行时传递变量

有关详细信息，请参阅[预测端点API参考](../api-reference/prediction.md)。

## 基础 URL 和身份验证

**基础 URL**：`http://localhost:3000`（或您的 Flowise 实例 URL）

**端点**：`POST /api/v1/prediction/:id`

**身份验证**：请参阅[流身份验证](../configuration/authorization/chatflow-level.md)

## 请求格式

#### 基本请求结构

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

#### 参数

|参数|类型 |必填 |描述 |
| ---------------- | ------- | --------------------------- | ------------------------------------------- |
| `question` |字符串|是的 |要发送到流程的消息/问题 |
| `form` |对象| `question` 或 `form` |要发送到流程的表单对象 |
| `streaming` |布尔 |没有 |启用流响应（默认值：false）|
| `overrideConfig` |对象|没有 |覆盖流配置 |
| `history` |数组|没有 |以前的对话消息 |
| `uploads` |数组|没有 |要上传的文件（图像、音频等）|
| `humanInput` |对象|没有 |返回人类反馈并恢复执行 |

## SDK 库

Flowise 提供 Python 和 TypeScript/JavaScript 的官方 SDK：

####安装

**Python**：`pip install flowise`

**TypeScript/JavaScript**：`npm install flowise-sdk`

#### Python SDK 用法

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

#### TypeScript/JavaScript SDK 用法

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

## 直接 HTTP API 用法

如果您更喜欢直接使用 REST API 而不使用 SDK：

#### 基本请求

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

## 高级功能

### 表单输入

在 智能体流程 V2 中，您可以选择 `form` 作为输入类型。

<figure><img src="../.gitbook/assets/image (333).png" alt="" width="418"><figcaption></figcaption></figure>

您可以通过表单输入的变量名称覆盖该值

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

### 配置覆盖

动态覆盖聊天流设置。

出于安全原因，默认情况下，覆盖配置是**禁用**的。从右上角启用它： **设置** → **配置** → **安全** 选项卡：

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

对于 `array` 类型，将鼠标悬停在信息图标上将显示可以覆盖的架构。

overrideConfig 中的数组值将与现有数组值连接。例如，如果现有 `startState` 具有：

```json
{
  "key": "key1",
  "value": "value1"
}
```

如果我们启用覆盖：

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

最终的 `startState` 将是：

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

### 覆盖特定节点

默认情况下，如果多个节点共享相同类型并且未指定节点 ID，则覆盖属性将在所有匹配节点上更新该属性。

例如，有 2 个 LLM 节点，我想覆盖系统消息：

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

启用覆盖功能后：

<figure><img src="../.gitbook/assets/image (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

我可以覆盖两个 LLM 的系统消息，如下所示：

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

从执行中，您可以看到覆盖的系统消息：

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

在某些情况下，您可能只想覆盖特定节点的配置。您可以通过在要覆盖的属性**内部**指定节点 ID 来实现此目的。

例如：

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

如果返回执行，您可以看到每个 LLM 都有正确的覆盖值：

<figure><img src="../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

### 对话历史记录

通过在历史数组中包含以前的消息来提供对话上下文。

**历史消息格式**

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

### 会话管理

使用 `sessionId` 在多个 API 调用之间维护对话状态。每个会话都维护自己的对话上下文和内存。

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

### 变量

使用 `overrideConfig` 中的 `vars` 属性将动态变量传递到流。可以在流程中使用变量来注入动态内容。

{% hint style="warning" %}
必须先创建变量，然后才能覆盖它。请参阅[变量](variables.md)
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

### 图片上传

当您的流程支持图像处理时，上传图像以进行视觉分析。请参阅[图片](uploads.md#image)以获取更多参考。

**上传结构：**

```json
{
    "data": "", 
    "type": "",
    "name": ",
    "mime": "
}
```

**数据：** 图像的 Base64 或 URL

**类型**：`url` 或 `file`

**名称：** 图片名称

**哑剧**：`image/png`、`image/jpeg`、`image/jpg`

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

### 音频上传（语音转文本）

上传音频文件以进行语音转文本处理。有关更多参考，请参阅[音频](uploads.md#audio)。

**上传结构：**

```json
{
    "data": "", 
    "type": "",
    "name": ",
    "mime": "
}
```

**数据：** 音频的 Base64 或 URL

**类型**：`url` 或 `file`

**名称：** 音频名称

**哑剧**：`audio/mp4`、`audio/webm`、`audio/wav`、`audio/mpeg`

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

### 文件上传

上传文件以使 LLM 处理文件并回答与文件相关的查询。请参阅[文件](uploads.md#files)以获取更多参考。

### 人工输入

当执行/对话停止并需要 [人的输入](../tutorials/human-in-the-loop.md)时，响应正文将返回 `action` 数据。用户可以使用它来呈现 UI 中的特定操作。

{% hint style="warning" %}
请记住保存 `chatId`，因为需要它来恢复执行/对话。
{% endhint %}

```json
{
    "text": "I'm just a computer program, but I'm here and ready to help you! How can I assist you today?\n\nProceed?",
    "question": "Hey, how are you?",
    "chatId": "c5a49fa0-3609-448c-bb36-b9937a34390e",
    "chatMessageId": "04fdaea7-276a-455c-9d18-479653b76c13",
    "executionId": "2cee26fa-5d6d-472b-a82d-fe1a5e2cafeb",
    "agentFlowExecutedData": [
        ...
    ],
    "action": {
        "id": "fcadd7ad-f5e0-4ca8-b11d-e2463aa20d0c",
        "mapping": {
            "approve": "Proceed",
            "reject": "Reject"
        },
        "elements": [
            {
                "type": "agentflowv2-approve-button",
                "label": "Proceed"
            },
            {
                "type": "agentflowv2-reject-button",
                "label": "Reject"
            }
        ],
        "data": {
            "nodeId": "humanInputAgentflow_0",
            "nodeLabel": "Human Input 0",
            "input": {
                "messages": [
                    {
                        "role": "user",
                        "content": "Hey, how are you?"
                    },
                    {
                        "role": "user",
                        "content": "I'm just a computer program, but I'm here and ready to help you! How can I assist you today?",
                        "name": "agent_0"
                    }
                ],
                "humanInputEnableFeedback": true
            }
        }
    }
}
```

#### UI 如何呈现批准/拒绝/反馈

Flowise Chat UI 读取 `action.elements` 并根据元素 `type` 呈现按钮：

* <mark style="color:$success;">**`agentflowv2-approve-button`**</mark> — 带有复选标记图标的绿色轮廓按钮
* <mark style="color:$danger;">**`agentflowv2-reject-button`**</mark>— 带有 X 图标的红色轮廓按钮

单击时，如果 `action.data.input.humanInputEnableFeedback` 是 `true`，则在提交之前会显示反馈对话框（文本区域）。否则立即提交。

对于您自己的聊天 UI，呈现如下所示的操作：

```javascript
// Pseudo-code for a custom UI
if (response.action) {
  const { elements, data } = response.action
  const showFeedback = data?.input?.humanInputEnableFeedback

  elements.forEach(elem => {
    if (elem.type === 'agentflowv2-approve-button') {
      // Render a green "Proceed" button
    }
    if (elem.type === 'agentflowv2-reject-button') {
      // Render a red "Reject" button
    }
  })

  // On button click:
  //   - If showFeedback is true, show a text input for feedback first
  //   - Then call the prediction API to resume (see below)
}
```

#### 如何恢复对话

当用户单击批准或拒绝时，发送另一个带有 `humanInput` 字段的 `POST /api/v1/prediction/{chatflowId}` ：

```json
{
  "chatId": "c5a49fa0-3609-448c-bb36-b9937a34390e",
  "humanInput": {
    "type": "proceed",
    "startNodeId": "humanInputAgentflow_0",
    "feedback": ""
  }
}
```

`humanInput` 中的三个关键字段：

|领域 |价值|描述 |
| ------------- | ------------------------- | -------------------------------------------------------------------------- |
| `type` | `"proceed"` 或 `"reject"` |按钮类型的映射 — `approve` → `"proceed"`、`reject` → `"reject"` |
| `startNodeId` |                           |来自 `action.data.nodeId` — 告诉服务器恢复哪个节点 |
| `feedback` |                           |如果 `humanInputEnableFeedback` 为 true，则可选反馈文本 |

**示例 - 拒绝并反馈：**

```json
{
  "chatId": "c5a49fa0-3609-448c-bb36-b9937a34390e",
  "humanInput": {
    "type": "reject",
    "startNodeId": "humanInputAgentflow_0",
    "feedback": "I think we should use a different approach"
  }
}
```

服务器将找到待处理的操作，将其清除，并根据用户的决定从 `startNodeId` 恢复智能体流程。

## 故障排除

1. **404 Not Found**：验证流ID是否正确且流存在
2. **401 Unauthorized Access**：验证流是否受 API 密钥保护
3. **400 Bad Request**：检查请求格式和必填字段
4. **413 Payload Too Large**：减小文件大小或拆分大请求
5. **500 内部服务器错误：** 检查流程中的节点是否存在任何配置错误
