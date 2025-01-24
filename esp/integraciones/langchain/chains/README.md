---
description: Nodos de Cadenas LangChain
---

# Cadenas

***

En el contexto de chatbots y modelos de lenguaje grandes, las "cadenas" típicamente se refieren a secuencias de texto o turnos de conversación. Estas cadenas se utilizan para almacenar y gestionar el historial de conversación y el contexto para el chatbot o modelo de lenguaje. Las cadenas ayudan al modelo a entender la conversación en curso y proporcionar respuestas coherentes y contextualmente relevantes.

Así es como funcionan las cadenas:

1. **Historial de Conversación**: Cuando un usuario interactúa con un chatbot o modelo de lenguaje, la conversación a menudo se representa como una serie de mensajes de texto o turnos de conversación. Cada mensaje del usuario y del modelo se almacena en orden cronológico para mantener el contexto de la conversación.
2. **Entrada y Salida**: Cada cadena consiste tanto en la entrada del usuario como en la salida del modelo. La entrada del usuario generalmente se conoce como la "cadena de entrada", mientras que las respuestas del modelo se almacenan en la "cadena de salida". Esto permite que el modelo se refiera a mensajes anteriores en la conversación.
3. **Comprensión Contextual**: Al preservar todo el historial de conversación en estas cadenas, el modelo puede entender el contexto y referirse a mensajes anteriores para proporcionar respuestas coherentes y contextualmente relevantes. Esto es crucial para mantener una conversación natural y significativa con los usuarios.
4. **Longitud Máxima**: Las cadenas tienen una longitud máxima para gestionar el uso de memoria y los recursos computacionales. Cuando una cadena se vuelve demasiado larga, los mensajes más antiguos pueden ser eliminados o truncados para hacer espacio para nuevos mensajes. Esto puede potencialmente llevar a la pérdida de contexto si se eliminan detalles importantes de la conversación.
5. **Continuación de la Conversación**: En una interacción en tiempo real con un chatbot o modelo de lenguaje, la cadena de entrada se actualiza continuamente con los nuevos mensajes del usuario, y la cadena de salida se actualiza con las respuestas del modelo. Esto permite que el modelo mantenga un seguimiento de la conversación en curso y responda apropiadamente.

Las cadenas son un concepto fundamental en la construcción y mantenimiento de conversaciones con chatbots y modelos de lenguaje. Aseguran que el modelo tenga acceso al contexto que necesita para generar respuestas significativas y conscientes del contexto, haciendo la interacción más atractiva y útil para los usuarios.

### Nodos de Cadena:

* [Cadena API GET](get-api-chain.md)
* [Cadena OpenAPI](openapi-chain.md)
* [Cadena API POST](post-api-chain.md)
* [Cadena de Conversación](conversation-chain.md)
* [Cadena de Recuperación Conversacional QA](conversational-retrieval-qa-chain.md)
* [Cadena LLM](llm-chain.md)
* [Cadena Multi Prompt](multi-prompt-chain.md)
* [Cadena Multi Recuperación QA](multi-retrieval-qa-chain.md)
* [Cadena de Recuperación QA](retrieval-qa-chain.md)
* [Cadena de Base de Datos SQL](sql-database-chain.md)
* [Cadena Vectara QA](vectara-chain.md)
* [Cadena VectorDB QA](vectordb-qa-chain.md)
