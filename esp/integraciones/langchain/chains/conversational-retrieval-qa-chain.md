# Cadena de Recuperación Conversacional QA

Una cadena para realizar tareas de preguntas y respuestas con un componente de recuperación.

<figure><img src="../../../.gitbook/assets/image (6) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

## Definiciones

**Una cadena de preguntas y respuestas basada en recuperación**, que se integra con un componente de recuperación y te permite configurar parámetros de entrada y realizar tareas de preguntas y respuestas.\
**Chatbots Basados en Recuperación:** Los chatbots basados en recuperación son chatbots que generan respuestas seleccionando respuestas predefinidas de una base de datos o un conjunto de posibles respuestas. "Recuperan" la respuesta más apropiada basándose en la entrada del usuario.\
**QA (Preguntas y Respuestas):** Los sistemas QA están diseñados para responder preguntas planteadas en lenguaje natural. Típicamente involucran entender la pregunta y buscar o generar una respuesta apropiada.

## Entradas

* [Modelo de Lenguaje](../chat-models/)
* [Recuperador de Almacén de Vectores](../vector-stores/)
* [Memoria (opcional)](../memory/)

## Parámetros

| Nombre                         | Descripción                                                                                                                                                    |
| ------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Devolver Documentos Fuente     | Para devolver citas/fuentes que se utilizaron para construir la respuesta                                                                                      |
| Mensaje del Sistema            | Una instrucción para el LLM sobre cómo responder la consulta                                                                                                    |
| Opción de Cadena              | Método sobre cómo resumir, responder preguntas y extraer información de documentos. Lee [más](https://js.langchain.com/docs/modules/chains/document/)           |

## Salidas

| Nombre                         | Descripción                            |
| ------------------------------ | -------------------------------------- |
| ConversationalRetrievalQAChain | Nodo final para devolver la respuesta |
