# Parte 2: Chains Avanzadas

En esta parte profundizaremos en el uso avanzado de chains y características relacionadas que te permitirán crear flujos más sofisticados.

## Tipos de Chains

### Chains Básicas
- [LLM Chain](../../integraciones/langchain/chains/llm-chain.md): La chain más fundamental
- [Conversation Chain](../../integraciones/langchain/chains/conversation-chain.md): Para diálogos continuos
- [Retrieval QA Chain](../../integraciones/langchain/chains/retrieval-qa-chain.md): Para preguntas y respuestas

### Chains Especializadas
- [GET API Chain](../../integraciones/langchain/chains/get-api-chain.md): Para interactuar con APIs externas
- [OpenAPI Chain](../../integraciones/langchain/chains/openapi-chain.md): Para APIs que siguen OpenAPI
- [SQL Database Chain](../../integraciones/langchain/chains/sql-database-chain.md): Para consultas a bases de datos

## Cache

El sistema de cache mejora el rendimiento y reduce costos:

### Tipos de Cache
- [InMemory Cache](../../integraciones/langchain/cache/in-memory-cache.md)
- [Redis Cache](../../integraciones/langchain/cache/redis-cache.md)
- [Momento Cache](../../integraciones/langchain/cache/momento-cache.md)

### Cache de Embeddings
- [InMemory Embedding Cache](../../integraciones/langchain/cache/inmemory-embedding-cache.md)
- [Redis Embeddings Cache](../../integraciones/langchain/cache/redis-embeddings-cache.md)

## Encadenamiento de Chains

Aprenderás a:
- Conectar múltiples chains
- Gestionar el flujo de datos entre chains
- Optimizar el rendimiento del encadenamiento
- Manejar errores y excepciones

## Proveedores

### Principales Proveedores
- [OpenAI](../../integraciones/langchain/chat-models/chatopenai.md)
- [Google AI](../../integraciones/langchain/chat-models/google-ai.md)
- [Anthropic](../../integraciones/langchain/chat-models/chatanthropic.md)
- [Hugging Face](../../integraciones/langchain/chat-models/chathuggingface.md)

### Consideraciones
- Costos y límites de API
- Velocidad y latencia
- Capacidades específicas
- Requisitos de API keys

## Input de Imágenes

### Procesamiento de Imágenes
- Carga y preparación
- OCR y extracción de texto
- Análisis de contenido visual
- Generación de descripciones

### Integraciones
- [Document Loaders](../../integraciones/langchain/document-loaders/README.md)
- Vision Models
- Image Processing Tools

## Prompts

### Diseño de Prompts
- [Prompt Template](../../integraciones/langchain/prompts/prompt-template.md)
- [Chat Prompt Template](../../integraciones/langchain/prompts/chat-prompt-template.md)
- [Few Shot Prompt Template](../../integraciones/langchain/prompts/few-shot-prompt-template.md)

### Mejores Prácticas
- Estructuración clara
- Contexto adecuado
- Manejo de variables
- Validación de resultados

## Output Parsers

### Tipos de Parsers
- [CSV Output Parser](../../integraciones/langchain/output-parsers/csv-output-parser.md)
- [Structured Output Parser](../../integraciones/langchain/output-parsers/structured-output-parser.md)
- [Custom List Output Parser](../../integraciones/langchain/output-parsers/custom-list-output-parser.md)

### Características Avanzadas
- [Advanced Structured Output Parser](../../integraciones/langchain/output-parsers/advanced-structured-output-parser.md)
- Validación de datos
- Manejo de errores
- Transformación de formatos

## Moderación

### Herramientas de Moderación
- [OpenAI Moderation](../../integraciones/langchain/moderation/openai-moderation.md)
- [Simple Prompt Moderation](../../integraciones/langchain/moderation/simple-prompt-moderation.md)

### Aspectos Clave
- Filtrado de contenido inapropiado
- Políticas de uso
- Logs y auditoría
- Manejo de violaciones

## Próximos Pasos

Al completar esta parte, estarás listo para:
- Implementar chains avanzadas
- Utilizar cache efectivamente
- Trabajar con múltiples proveedores
- Procesar inputs de imágenes
- Diseñar prompts efectivos
- Parsear y moderar outputs
- Avanzar al [Desafío 1: Traductor de Lenguajes Antiguos](../../desafios/desafio-1.md) 