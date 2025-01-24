# Refine

Crea y refina una respuesta pasando secuencialmente por cada fragmento de texto recuperado.

**Pros**: Bueno para respuestas más detalladas

**Contras**: Llamada separada al LLM por cada Nodo (puede ser costoso)

<figure><img src="../../../.gitbook/assets/image (5) (1) (1) (1) (1) (2) (1).png" alt=""><figcaption></figcaption></figure>

**Prompt de Refinamiento**

```markup
La consulta original es la siguiente: {query}
Hemos proporcionado una respuesta existente: {existingAnswer}
Tenemos la oportunidad de refinar la respuesta existente (solo si es necesario) con más contexto a continuación.
------------
{context}
------------
Dado el nuevo contexto, refina la respuesta original para responder mejor a la consulta. Si el contexto no es útil, devuelve la respuesta original.
Respuesta Refinada:
```

**Prompt de Text QA**

```
La información de contexto está a continuación.
---------------------
{context}
---------------------
Dado el contexto y sin conocimiento previo, responde a la consulta.
Consulta: {query}
Respuesta:
```
