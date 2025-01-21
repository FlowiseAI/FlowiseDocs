# Ejemplos de Código

## Instalación y Configuración

### Instalación Local
```bash
# Usando npm
npm install -g flowise
npx flowise start

# Usando Docker
docker pull flowiseai/flowise
docker run -d -p 3000:3000 flowiseai/flowise
```

### Variables de Entorno
```env
PORT=3000
APIKEY=tu-api-key
DATABASE_PATH=.flowise
SECRETKEY=tu-secreto
DEBUG=true
```

## Chains Básicos

### Chain Simple
```javascript
const chain = {
    nodes: [
        {
            "id": "1",
            "data": {
                "type": "chatModel",
                "model": "gpt-3.5-turbo"
            }
        },
        {
            "id": "2",
            "data": {
                "type": "prompt",
                "template": "Responde como un experto en {tema}"
            }
        }
    ],
    edges: [
        {
            "source": "2",
            "target": "1"
        }
    ]
}
```

### Chain con Memoria
```javascript
const chain = {
    nodes: [
        {
            "id": "1",
            "data": {
                "type": "chatModel",
                "model": "gpt-3.5-turbo"
            }
        },
        {
            "id": "2",
            "data": {
                "type": "memory",
                "memoryType": "buffer"
            }
        }
    ],
    edges: [
        {
            "source": "2",
            "target": "1"
        }
    ]
}
```

## Agentes

### Agente Conversacional
```javascript
const agent = {
    nodes: [
        {
            "id": "1",
            "data": {
                "type": "conversationalAgent",
                "model": "gpt-3.5-turbo"
            }
        },
        {
            "id": "2",
            "data": {
                "type": "tool",
                "tool": "calculator"
            }
        }
    ],
    edges: [
        {
            "source": "2",
            "target": "1"
        }
    ]
}
```

### Agente Autónomo
```javascript
const agent = {
    nodes: [
        {
            "id": "1",
            "data": {
                "type": "autonomousAgent",
                "model": "gpt-4"
            }
        },
        {
            "id": "2",
            "data": {
                "type": "memory",
                "memoryType": "vector"
            }
        }
    ],
    edges: [
        {
            "source": "2",
            "target": "1"
        }
    ]
}
```

## Sequential Agents

### Estado y Control
```javascript
const flow = {
    nodes: [
        {
            "id": "1",
            "data": {
                "type": "stateNode",
                "initialState": { "count": 0 }
            }
        },
        {
            "id": "2",
            "data": {
                "type": "loopNode",
                "maxIterations": 5
            }
        }
    ],
    edges: [
        {
            "source": "1",
            "target": "2"
        }
    ]
}
```

### Condiciones
```javascript
const flow = {
    nodes: [
        {
            "id": "1",
            "data": {
                "type": "conditionNode",
                "condition": "state.count > 10"
            }
        },
        {
            "id": "2",
            "data": {
                "type": "endNode"
            }
        }
    ],
    edges: [
        {
            "source": "1",
            "target": "2"
        }
    ]
}
```

## API Calls

### Chat Message
```javascript
const response = await fetch('http://localhost:3000/api/v1/prediction/123', {
    method: 'POST',
    headers: {
        'Content-Type': 'application/json',
        'Authorization': 'Bearer tu-api-key'
    },
    body: JSON.stringify({
        question: "¿Cuál es la capital de Francia?",
        history: []
    })
})
```

### Variables
```javascript
const response = await fetch('http://localhost:3000/api/v1/variables', {
    method: 'POST',
    headers: {
        'Content-Type': 'application/json',
        'Authorization': 'Bearer tu-api-key'
    },
    body: JSON.stringify({
        chatflowid: "123",
        variables: {
            tema: "historia"
        }
    })
})
```

## Integración Frontend

### React Component
```jsx
import { FlowiseChat } from 'flowise-react'

function App() {
    return (
        <FlowiseChat
            chatflowid="123"
            apiHost="http://localhost:3000"
            theme={{
                button: { backgroundColor: "#3B81F6" },
                chat: { backgroundColor: "#ffffff" }
            }}
        />
    )
}
```

### HTML Integration
```html
<script type="module">
    import { FlowiseChat } from 'https://cdn.jsdelivr.net/npm/flowise-embed/dist/web.js'
    
    FlowiseChat.init({
        chatflowid: "123",
        apiHost: "http://localhost:3000"
    })
</script>
```

## Python Client

### Async Client
```python
from flowise import FlowiseAsync

client = FlowiseAsync(
    api_host="http://localhost:3000",
    api_key="tu-api-key"
)

async def main():
    response = await client.chat(
        chatflow_id="123",
        message="¿Cuál es la capital de España?"
    )
    print(response)

asyncio.run(main())
```

### Sync Client
```python
from flowise import Flowise

client = Flowise(
    api_host="http://localhost:3000",
    api_key="tu-api-key"
)

response = client.chat(
    chatflow_id="123",
    message="¿Cuál es la capital de Italia?"
)
print(response)
``` 