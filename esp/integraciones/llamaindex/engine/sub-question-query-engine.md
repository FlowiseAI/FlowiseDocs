# Sub-Question Query Engine

Un query engine diseñado para resolver el problema de responder a una consulta compleja utilizando múltiples fuentes de datos. Primero divide la consulta compleja en sub-preguntas para cada fuente de datos relevante, luego reúne todas las respuestas intermedias y sintetiza una respuesta final.

<figure><img src="../../../.gitbook/assets/image (4) (1) (1) (1) (1) (2) (1).png" alt=""><figcaption></figcaption></figure>

## Entradas

* Query Engine Tools
* Chat Model
* Embeddings
* [Response Synthesizer](../response-synthesizer/)

## Parámetros

| Nombre                  | Descripción                                                          |
| ----------------------- | ------------------------------------------------------------------- |
| Return Source Documents | Para devolver citas/fuentes que se usaron para construir la respuesta|

## Salidas

| Nombre                 | Descripción                            |
| ---------------------- | -------------------------------------- |
| SubQuestionQueryEngine | Nodo final para devolver la respuesta  |
