# Parte 9: Agentes Secuenciales

En esta novena parte del curso, exploraremos los agentes secuenciales, que son sistemas que ejecutan acciones en un orden específico y controlado.

## Contenidos

- [Resolución del Desafío 7](#resolución-del-desafío-7)
- [Introducción a Agentes Secuenciales](#introducción-a-agentes-secuenciales)
- [Componentes](#componentes)
  - [Agents](#agents)
  - [Condition](#condition)
  - [Condition Agent](#condition-agent)
  - [End](#end)
  - [LLM Node](#llm-node)
  - [Loops](#loops)
  - [Start](#start)
  - [State](#state)
  - [Tool Node](#tool-node)

## Resolución del Desafío 7

Analizaremos la solución del Planificador de Vacaciones, explorando:
- Estructura del agente
- Manejo de requisitos
- Planificación de itinerarios
- Integración de servicios

> 💡 **Sugerencia de Diagrama**: Un diagrama de flujo mostrando el proceso completo de planificación de vacaciones, desde la recepción de requisitos hasta la generación del itinerario final.

## Introducción a Agentes Secuenciales

Los agentes secuenciales son sistemas que:
- Ejecutan acciones en un orden específico
- Mantienen un estado coherente
- Toman decisiones basadas en condiciones
- Pueden repetir acciones cuando es necesario

### Características Principales
- Flujo controlado
- Estado persistente
- Decisiones condicionales
- Capacidad de bucles
- Manejo de errores

> 💡 **Sugerencia de Diagrama**: Un diagrama mostrando el flujo básico de un agente secuencial y sus características principales.

## Componentes

### Agents
Los Agents son los ejecutores principales que:
- Realizan tareas específicas
- Procesan información
- Toman decisiones
- Interactúan con herramientas

> 💡 **Sugerencia de Diagrama**: Un diagrama mostrando la estructura interna de un Agent y sus interacciones.

### Condition
Las Conditions son nodos que:
- Evalúan estados
- Toman decisiones
- Dirigen el flujo
- Manejan lógica condicional

### Condition Agent
El Condition Agent es un agente especializado que:
- Evalúa condiciones complejas
- Toma decisiones basadas en múltiples factores
- Dirige el flujo del proceso
- Mantiene coherencia lógica

### End
Los nodos End:
- Marcan el final de un flujo
- Limpian recursos
- Finalizan procesos
- Devuelven resultados

### LLM Node
Los LLM Nodes son nodos que:
- Procesan lenguaje natural
- Generan respuestas
- Analizan contenido
- Toman decisiones complejas

### Loops
Los Loops permiten:
- Repetir acciones
- Procesar listas
- Iterar sobre datos
- Mantener estado entre iteraciones

> 💡 **Sugerencia de Diagrama**: Un diagrama mostrando los diferentes tipos de loops y sus casos de uso.

### Start
Los nodos Start:
- Inician el flujo
- Configuran el estado inicial
- Validan parámetros
- Preparan recursos

### State
El manejo de State permite:
- Mantener información entre nodos
- Compartir datos
- Trackear progreso
- Gestionar contexto

> 💡 **Sugerencia de Diagrama**: Un diagrama mostrando cómo el estado fluye entre diferentes componentes.

### Tool Node
Los Tool Nodes son nodos que:
- Integran herramientas externas
- Ejecutan acciones específicas
- Procesan datos
- Devuelven resultados

## Links Relevantes

### Conceptos Básicos
- [Sequential Agents Overview](../../usar-flowise/agentflows/sequential-agents/README.md)
- [State Management](../../usar-flowise/agentflows/sequential-agents/state.md)
- [Flow Control](../../usar-flowise/agentflows/sequential-agents/flow-control.md)

### Componentes
- [Agents](../../usar-flowise/agentflows/sequential-agents/agents.md)
- [Conditions](../../usar-flowise/agentflows/sequential-agents/conditions.md)
- [LLM Nodes](../../usar-flowise/agentflows/sequential-agents/llm-nodes.md)
- [Tool Nodes](../../usar-flowise/agentflows/sequential-agents/tool-nodes.md)

### Tutoriales
- [Getting Started](../../usar-flowise/agentflows/sequential-agents/getting-started.md)
- [Best Practices](../../usar-flowise/agentflows/sequential-agents/best-practices.md)
- [Advanced Patterns](../../usar-flowise/agentflows/sequential-agents/advanced-patterns.md) 