---
description: Custom Retriever permite al usuario especificar el formato del contexto para el LLM
---

# Custom Retriever

<figure><img src="../../../.gitbook/assets/image (3) (1) (1).png" alt="" width="298"><figcaption></figcaption></figure>

Por defecto, cuando el contexto se recupera del vector store, tiene el siguiente formato:

```json
[ 
    {
        "pageContent": "This is an example",
        "metadata": {
            "source": "example.pdf"
        }
    },
    {
        "pageContent": "This is example 2",
        "metadata": {
            "source": "example2.txt"
        }
    }
]
```

El **pageContent** del array se unirá como una cadena de texto y se enviará al LLM para su completado.

Sin embargo, en algunos casos, es posible que desees incluir información de los metadatos para proporcionar más información al LLM, como la fuente, el enlace, etc. Aquí es donde entra el **Custom Retriever**. Podemos especificar el formato que se devolverá al LLM.

Por ejemplo, usando el siguiente formato:

```javascript
{{context}}
Source: {{metadata.source}}
```

Resultará en la cadena combinada como se muestra a continuación:

```
This is an example
Source: example.pdf

This is example 2
Source: example2.txt
```

Esto se enviará de vuelta al LLM. Como el LLM ahora tiene las fuentes de las respuestas, podemos usar prompts para instruir al LLM para que devuelva respuestas seguidas de citas.
