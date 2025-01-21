# Context Chat Engine

Un chat engine sirve como un pipeline de extremo a extremo para mantener una conversación similar a la humana con tus datos, permitiendo múltiples intercambios en lugar de una única interacción de pregunta y respuesta.

<figure><img src="../../../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (2) (1) (1).png" alt=""><figcaption></figcaption></figure>

## Entradas

* Chat Model
* Vector Store Retriever
* [Memory](../../langchain/memory/)

## Parámetros

| Nombre                  | Descripción                                                          |
| ----------------------- | ------------------------------------------------------------------- |
| Return Source Documents | Para devolver citas/fuentes que se usaron para construir la respuesta|
| System Message          | Una instrucción para el LLM sobre cómo responder la consulta        |

## Salidas

| Nombre            | Descripción                            |
| ----------------- | -------------------------------------- |
| ContextChatEngine | Nodo final para devolver la respuesta  |
