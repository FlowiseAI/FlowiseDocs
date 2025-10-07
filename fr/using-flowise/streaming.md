---
description: Learn how Flowise streaming works
---

# Streaming

Si le streaming est défini lors de la prédiction, les jetons seront envoyés comme des données uniquement[server-sent events](https://developer.mozilla.org/en-US/docs/Web/API/Server-sent_events/Using_server-sent_events#Event_stream_format)à mesure qu'ils deviennent disponibles.

### Utilisation de la bibliothèque Python / TS

Flowise fournit 2 bibliothèques:

* [Python](https://pypi.org/project/flowise/): `pip install flowise`
* [Typescript](https://www.npmjs.com/package/flowise-sdk): `npm install flowise-sdk`

{% Tabs%}
{% tab title = "python"%}
```python
from flowise import Flowise, PredictionData

def test_streaming():
    client = Flowise()

    # Test streaming prediction
    completion = client.create_prediction(
        PredictionData(
            chatflowId="<flow-id>",
            question="Tell me a joke!",
            streaming=True
        )
    )

    # Process and print each streamed chunk
    print("Streaming response:")
    for chunk in completion:
        # {event: "token", data: "hello"}
        print(chunk)


if __name__ == "__main__":
    test_streaming()
```
{% endtab%}

{% tab title = "TypeScript"%}
```javascript
import { FlowiseClient } from 'flowise-sdk'

async function test_streaming() {
  const client = new FlowiseClient({ baseUrl: 'http://localhost:3000' });

  try {
    // For streaming prediction
    const prediction = await client.createPrediction({
      chatflowId: '<flow-id>',
      question: 'What is the capital of France?',
      streaming: true,
    });

    for await (const chunk of prediction) {
        // {event: "token", data: "hello"}
        console.log(chunk);
    }
    
  } catch (error) {
    console.error('Error:', error);
  }
}

// Run streaming test
test_streaming()
```
{% endtab%}

{% tab title = "curl"%}
```bash
curl https://localhost:3000/api/v1/predictions/{flow-id} \
  -H "Content-Type: application/json" \
  -d '{
    "question": "Hello world!",
    "streaming": true
  }'
```
{% endtab%}
{% endtabs%}

```html
event: token
data: Once upon a time...
```

Le flux d'événements d'une prédiction se compose des types d'événements suivants:

| Événement | Description |
| --------------- | ----------------------------------------------------------------------------------------------------------------------------------- |
| Commencez | Le début du streaming |
| Token | Émis lorsque la prédiction diffuse une nouvelle sortie de jeton |
| Erreur | Émis lorsque la prédiction renvoie une erreur |
| Fin | Émis lorsque la prédiction se termine |
| métadonnées | Toutes les métadonnées telles que Chatid, MessageID, du flux connexe. Émis après tout, les jetons ont terminé le streaming et avant l'événement de fin |
| SourcedoDuments | Émis lorsque le flux renvoie des sources du magasin vectoriel |
| usageTools | Émis lorsque le flux a utilisé des outils |

### Rationaliser l'application

[https://github.com/HenryHengZJ/flowise-streamlit](https://github.com/HenryHengZJ/flowise-streamlit)
