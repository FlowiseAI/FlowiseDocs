# Desafío 2: Chatbot Nikola Tesla

## Objetivo
Crear un chatbot que simule una conversación con Nikola Tesla, capaz de responder preguntas sobre su vida e invenciones con precisión histórica.

## Requisitos
- Procesamiento del PDF con información sobre Tesla
- Capacidad de responder preguntas específicas sobre su vida
- Mantenimiento de contexto conversacional
- Respuestas precisas y fundamentadas en hechos históricos

## Preguntas de Validación
El chatbot debe ser capaz de responder correctamente las siguientes preguntas:
1. ¿En qué ciudad comenzó Tesla a trabajar con Thomas Edison?
2. ¿Qué enfermedad grave sufrió Tesla durante su juventud?
3. ¿Qué nombre le dio Tesla a su torre para experimentos en Long Island?
4. ¿Qué número obsesionaba a Tesla?

## Herramientas Recomendadas
- [PDF File Loader](../../integraciones/langchain/document-loaders/pdf-file.md) para procesar la documentación
- [Text Splitters](../../integraciones/langchain/text-splitters/README.md) para segmentar el contenido
- [Vector Stores](../../integraciones/langchain/vector-stores/README.md) para almacenar y recuperar información
- [Conversation Chain](../../integraciones/langchain/chains/conversation-chain.md) para mantener el contexto

## Implementación Sugerida

### 1. Preparación de Datos
- Cargar y procesar el PDF proporcionado
- Segmentar el texto en chunks manejables
- Generar embeddings del contenido
- Almacenar en un vector store

### 2. Diseño del Chatbot
- Implementar sistema de memoria conversacional
- Crear prompts efectivos para mantener el personaje
- Establecer mecanismos de recuperación de información
- Configurar validación de respuestas

### 3. Optimización
- Ajustar parámetros de búsqueda
- Refinar prompts para mejorar precisión
- Implementar manejo de preguntas fuera de contexto
- Optimizar tiempos de respuesta

## Criterios de Evaluación
1. Precisión en las respuestas a las preguntas de validación
2. Calidad y naturalidad de la conversación
3. Mantenimiento del personaje de Tesla
4. Eficiencia en la recuperación de información
5. Manejo de preguntas inesperadas

## Entrega
La solución se revisará en la [Parte 4](../partes/parte-4/README.md) del curso, donde analizaremos diferentes enfoques y mejores prácticas.

## Consejos
- Utilizar RAG (Retrieval Augmented Generation) para mejorar la precisión
- Implementar filtros de relevancia en las búsquedas
- Mantener un balance entre precisión histórica y naturalidad conversacional
- Documentar el proceso de desarrollo y decisiones tomadas 