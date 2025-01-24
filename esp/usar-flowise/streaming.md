---
description: Aprende cómo funciona el streaming en Flowise
---

# Streaming

Si el streaming está configurado al hacer una predicción, los tokens se enviarán como eventos del servidor de solo datos ([server-sent events](https://developer.mozilla.org/en-US/docs/Web/API/Server-sent\_events/Using\_server-sent\_events#Event\_stream\_format)) a medida que estén disponibles.

### Usando la Biblioteca Python/TS

Flowise proporciona 2 bibliotecas:

* [Python](https://pypi.org/project/flowise/): `pip install flowise`
* [Typescript](https://www.npmjs.com/package/flowise-sdk): `npm install flowise-sdk`

{% tabs %}
{% tab title="Python" %}
```python
from flowise import Flowise, PredictionData

def test_streaming():
    client = Flowise()

    # Prueba de predicción con streaming
    completion = client.create_prediction(
        PredictionData(
            chatflowId="<chatflow-id>",
            question="¡Cuéntame un chiste!",
            streaming=True
        )
    )

    # Procesa e imprime cada fragmento transmitido
    print("Respuesta en streaming:")
    for chunk in completion:
        # {event: "token", data: "hola"}
        print(chunk)


if __name__ == "__main__":
    test_streaming()
```
{% endtab %}

{% tab title="Typescript" %}
```javascript
import { FlowiseClient } from 'flowise-sdk'

async function test_streaming() {
  const client = new FlowiseClient({ baseUrl: 'http://localhost:3000' });

  try {
    // Para predicción con streaming
    const prediction = await client.createPrediction({
      chatflowId: '<chatflow-id>',
      question: '¿Cuál es la capital de Francia?',
      streaming: true,
    });

    for await (const chunk of prediction) {
        // {event: "token", data: "hola"}
        console.log(chunk);
    }
    
  } catch (error) {
    console.error('Error:', error);
  }
}

// Ejecutar prueba de streaming
test_streaming()
```
{% endtab %}

{% tab title="cURL" %}
```bash
curl https://localhost:3000/api/v1/predictions/{chatflow-id} \
  -H "Content-Type: application/json" \
  -d '{
    "question": "¡Hola mundo!",
    "streaming": true
  }'
```
{% endtab %}
{% endtabs %}

```html
event: token
data: Había una vez...
```

El flujo de eventos de una predicción consiste en los siguientes tipos de eventos:

| Evento          | Descripción                                                                                                                                |
| --------------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| start           | El inicio del streaming                                                                                                                    |
| token           | Emitido cuando la predicción está transmitiendo un nuevo token de salida                                                                   |
| error           | Emitido cuando la predicción devuelve un error                                                                                            |
| end             | Emitido cuando la predicción finaliza                                                                                                      |
| metadata        | Todos los metadatos como chatId, messageId, del flujo relacionado. Emitido después de que todos los tokens han terminado de transmitirse, y antes del evento end |
| sourceDocuments | Emitido cuando el flujo devuelve fuentes del almacén de vectores                                                                           |
| usedTools       | Emitido cuando el flujo utilizó herramientas                                                                                               |

### Aplicación Streamlit

[https://github.com/HenryHengZJ/flowise-streamlit](https://github.com/HenryHengZJ/flowise-streamlit)
