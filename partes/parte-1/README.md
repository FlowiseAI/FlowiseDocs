# Parte 1: Introducción

En esta primera parte del curso, aprenderás los fundamentos esenciales para comenzar con Flowise. Cubriremos desde la instalación básica hasta conceptos fundamentales que necesitarás para crear tus primeros flujos.

## Instalación de Flowise

La instalación de Flowise se puede realizar de varias maneras. Aquí cubriremos los métodos principales:

### Instalación Local
```bash
# Usando npm
npm install -g flowise

# Usando Docker
docker run -d -p 3000:3000 flowiseai/flowise
```

### Requisitos Previos
- Node.js 18 o superior
- NPM o PNPM
- Git (para actualizaciones y desarrollo)

## Familiarización con la Interfaz de Usuario

### Panel Principal
- **Canvas**: Área de trabajo principal donde construirás tus flujos
- **Nodos**: Componentes que puedes arrastrar y soltar en el canvas
- **Conexiones**: Enlaces entre nodos que definen el flujo de datos

### Elementos Clave
- Barra de herramientas
- Biblioteca de nodos
- Panel de configuración
- Área de pruebas
- Gestión de variables

## Chains

Las [Chains](../../documentacion-oficial/usar-flowise/chains.md) son componentes fundamentales en Flowise que permiten conectar diferentes operaciones.

### Tipos Básicos de Chains
- [LLM Chain](../../integraciones/langchain/chains/llm-chain.md)
- [Conversation Chain](../../integraciones/langchain/chains/conversation-chain.md)
- [Retrieval QA Chain](../../integraciones/langchain/chains/retrieval-qa-chain.md)

## Chat Models

Los [Chat Models](../../documentacion-oficial/usar-flowise/chat-models.md) son el núcleo de la funcionalidad conversacional.

### Modelos Disponibles
- [ChatGPT](../../integraciones/langchain/chat-models/chatopenai.md)
- [Google AI](../../integraciones/langchain/chat-models/google-ai.md)
- [Anthropic Claude](../../integraciones/langchain/chat-models/chatanthropic.md)
- [Ollama](../../integraciones/langchain/chat-models/chatollama.md)

## Cómo estar al día con Flowise

### Proceso de Actualización
```bash
git pull origin main
pnpm install
pnpm build
```

### Buenas Prácticas
- Revisar el [changelog](https://github.com/FlowiseAI/Flowise/releases) antes de actualizar
- Hacer backup de tus flujos importantes
- Probar en un entorno de desarrollo primero

## Memoria

La [memoria](../../integraciones/langchain/memory/README.md) es crucial para mantener el contexto en las conversaciones.

### Tipos de Memoria
- [Buffer Memory](../../integraciones/langchain/memory/buffer-memory.md)
- [Buffer Window Memory](../../integraciones/langchain/memory/buffer-window-memory.md)
- [Conversation Summary Memory](../../integraciones/langchain/memory/conversation-summary-memory.md)

### Consideraciones
- Gestión del contexto
- Límites de tokens
- Persistencia de datos
- Optimización de recursos

## Próximos Pasos

Al completar esta parte, estarás preparado para:
- Crear flujos básicos funcionales
- Trabajar con diferentes modelos de chat
- Mantener tu instalación actualizada
- Gestionar la memoria en tus aplicaciones
- Avanzar hacia conceptos más avanzados en la [Parte 2: Chains Avanzadas](../parte-2/README.md) 