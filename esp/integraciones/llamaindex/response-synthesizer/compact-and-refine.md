# Compact And Refine

Este es el valor por defecto cuando no se define explícitamente un Response Synthesizer.

Compacta el prompt durante cada llamada al LLM agrupando tantos fragmentos de texto como puedan caber dentro del tamaño máximo del prompt. Si hay demasiados fragmentos para agrupar en un solo prompt, "crea y refina" una respuesta pasando por múltiples prompts compactos.

**Pros**: Los mismos que [Refine](refine.md), bueno para respuestas más detalladas y debería resultar en menos llamadas al LLM

**Contras**: Debido a las múltiples llamadas al LLM, puede ser costoso

<figure><img src="../../../.gitbook/assets/image (6) (1) (1) (1) (2).png" alt=""><figcaption></figcaption></figure>

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
