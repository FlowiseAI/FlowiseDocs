# Query Engine

Un query engine sirve como un pipeline de extremo a extremo que permite a los usuarios hacer preguntas sobre sus datos. Recibe una consulta en lenguaje natural y proporciona una respuesta, acompañada de información contextual relevante recuperada y enviada al LLM (Large Language Model).

<figure><img src="../../../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (2) (1).png" alt=""><figcaption></figcaption></figure>

## Entradas

* Vector Store Retriever
* [Response Synthesizer](../response-synthesizer/)

## Parámetros

| Nombre                  | Descripción                                                          |
| ----------------------- | ------------------------------------------------------------------- |
| Return Source Documents | Para devolver citas/fuentes que se usaron para construir la respuesta|

## Salidas

| Nombre      | Descripción                            |
| ----------- | -------------------------------------- |
| QueryEngine | Nodo final para devolver la respuesta  |
