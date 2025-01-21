# Parte 4: Despliegue y API

## Resolución del Desafío 2

### Análisis de la Solución
- Revisión del [Desafío 2: Chatbot Nikola Tesla](../../desafios/desafio-2.md)
- Demostración del chatbot implementado
- Análisis de las respuestas a las preguntas de validación
- Discusión de mejores prácticas

### Aspectos Destacados
- Procesamiento eficiente del PDF
- Implementación de RAG
- Gestión de contexto conversacional
- Optimización de respuestas

## Despliegue de Chatbot

### Despliegue Local
- Configuración del entorno
- Gestión de dependencias
- Testing local
- Monitoreo de recursos

### Despliegue en la Nube
- [AWS](../../configuracion/deployment/aws.md)
- [Azure](../../configuracion/deployment/azure.md)
- [GCP](../../configuracion/deployment/gcp.md)
- [Digital Ocean](../../configuracion/deployment/digital-ocean.md)

## Llamadas API

### Configuración de API
- [API Reference](../../referencia-api/README.md)
- Endpoints disponibles
- Autenticación y seguridad
- Rate limiting

### Métodos Principales
- [Chat Message](../../referencia-api/chat-message.md)
- [Prediction](../../referencia-api/prediction.md)
- [Chatflows](../../referencia-api/chatflows.md)
- [Variables](../../referencia-api/variables.md)

## Configuración Avanzada de Chatflows

### Optimización
- Gestión de recursos
- Caché y rendimiento
- Manejo de errores
- Logging y monitoreo

### Seguridad
- [Auth](../../configuracion/autorizacion/README.md)
- [App Level](../../configuracion/autorizacion/nivel-app.md)
- [Chatflow Level](../../configuracion/autorizacion/nivel-chatflow.md)

## CURL

### Ejemplos de Uso
```bash
# Ejemplo de chat
curl -X POST http://localhost:3000/api/v1/prediction/{chatflowid} \
     -H "Content-Type: application/json" \
     -d '{"question": "Hello", "history": []}'

# Ejemplo de variables
curl -X GET http://localhost:3000/api/v1/variables \
     -H "Authorization: Bearer {token}"
```

### Mejores Prácticas
- Manejo de respuestas
- Gestión de errores
- Rate limiting
- Autenticación

## Share

### Opciones de Compartir
- Enlaces públicos
- Embeddings
- Permisos y restricciones
- Analíticas

## Javascript Implementation

### Cliente Web
```javascript
const response = await fetch('http://localhost:3000/api/v1/prediction/{chatflowid}', {
    method: 'POST',
    headers: {
        'Content-Type': 'application/json'
    },
    body: JSON.stringify({
        question: 'Hello',
        history: []
    })
});
```

### Node.js
```javascript
const axios = require('axios');
const result = await axios.post('http://localhost:3000/api/v1/prediction/{chatflowid}', {
    question: 'Hello',
    history: []
});
```

## Python Implementation

### Cliente Python
```python
import requests

response = requests.post(
    'http://localhost:3000/api/v1/prediction/{chatflowid}',
    json={
        'question': 'Hello',
        'history': []
    }
)
```

### Async/Await
```python
import aiohttp
import asyncio

async def chat():
    async with aiohttp.ClientSession() as session:
        async with session.post(
            'http://localhost:3000/api/v1/prediction/{chatflowid}',
            json={'question': 'Hello', 'history': []}
        ) as response:
            return await response.json()
```

## Config

### Variables de Entorno
- [Environment Variables](../../configuracion/variables-entorno.md)
- Configuración de bases de datos
- API keys y secretos
- Límites y restricciones

### Deployment Config
- [Deployment](../../configuracion/deployment/README.md)
- Escalabilidad
- Backups
- Monitoreo

## React vs Html

### React Implementation
```jsx
import { useEffect, useState } from 'react';

function ChatComponent() {
    const [messages, setMessages] = useState([]);
    const [input, setInput] = useState('');

    const sendMessage = async () => {
        const response = await fetch('http://localhost:3000/api/v1/prediction/{chatflowid}', {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json'
            },
            body: JSON.stringify({
                question: input,
                history: messages
            })
        });
        const data = await response.json();
        setMessages([...messages, { role: 'user', content: input }, { role: 'assistant', content: data.text }]);
        setInput('');
    };

    return (
        <div>
            {/* Chat UI implementation */}
        </div>
    );
}
```

### HTML Implementation
```html
<div class="chat-container">
    <div id="messages"></div>
    <input type="text" id="input" />
    <button onclick="sendMessage()">Send</button>
</div>

<script>
async function sendMessage() {
    const input = document.getElementById('input');
    const messages = document.getElementById('messages');
    
    const response = await fetch('http://localhost:3000/api/v1/prediction/{chatflowid}', {
        method: 'POST',
        headers: {
            'Content-Type': 'application/json'
        },
        body: JSON.stringify({
            question: input.value,
            history: []
        })
    });
    
    const data = await response.json();
    messages.innerHTML += `<p><strong>You:</strong> ${input.value}</p>`;
    messages.innerHTML += `<p><strong>Bot:</strong> ${data.text}</p>`;
    input.value = '';
}
</script>
```

## Popup vs Full

### Popup Implementation
- Diseño compacto
- Integración en sitios existentes
- Gestión de estado
- Responsive design

### Full Page Implementation
- UI/UX completa
- Funcionalidades avanzadas
- Personalización extensa
- Analíticas detalladas

## Authorization

### Métodos de Autenticación
- [API Keys](../../configuracion/autorizacion/nivel-app.md)
- [OAuth](../../configuracion/sso.md)
- JWT
- Session-based

### Seguridad
- CORS
- Rate Limiting
- Input Validation
- Error Handling

## Próximos Pasos

Al completar esta parte, estarás preparado para:
- Desplegar chatbots en producción
- Implementar integraciones API
- Configurar seguridad y autorización
- Desarrollar interfaces de usuario
- Avanzar al [Desafío 3: Investigador de Memes](../../desafios/desafio-3.md) 