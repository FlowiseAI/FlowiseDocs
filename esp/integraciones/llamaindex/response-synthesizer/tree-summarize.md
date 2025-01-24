# Tree Summarize

Cuando se proporcionan fragmentos de texto y una consulta, construye recursivamente una estructura de árbol y devuelve el nodo raíz como resultado.

**Pros**: Beneficioso para tareas de summarization

**Contras**: La precisión de la respuesta podría perderse durante el recorrido de la estructura del árbol

<figure><img src="../../../.gitbook/assets/image (7) (1) (1) (1) (2).png" alt=""><figcaption></figcaption></figure>

**Prompt**

```
La información de contexto de múltiples fuentes está a continuación.
---------------------
{context}
---------------------
Dada la información de múltiples fuentes y sin conocimiento previo, responde la consulta.
Query: {query}
Respuesta:
```
