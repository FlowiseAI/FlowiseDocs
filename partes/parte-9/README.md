# Parte 9: Sequential Agents

Esta guía ofrece una visión completa de la arquitectura de Sequential Agent AI dentro de Flowise, explorando sus componentes principales y principios de diseño del flujo de trabajo.

## Contenidos
- [Concepto](#concepto)
    - [Entendiendo la Arquitectura DCG de Sequential Agents](#entendiendo-la-arquitectura-dcg-de-sequential-agents)
        - [Principios Fundamentales](#principios-fundamentales)
        - [Terminología](#terminología)
- [Sequential Agents vs Multi-Agents](#sequential-agents-vs-multi-agents)
- [Introduciendo State, Loop y Condition Nodes](#introduciendo-state-loop-y-condition-nodes)
- [Eligiendo el sistema correcto](#eligiendo-el-sistema-correcto)
- [Sequential Agents Nodes](#sequential-agents-nodes)
- [1. Start Node](#1-start-node)
    - [Entendiendo el Start Node](#entendiendo-el-start-node)
    - [Inputs](#inputs-start-node)
    - [Outputs](#outputs-start-node)
    - [Mejores Prácticas](#mejores-prácticas-start-node)
- [2. Agent Memory Node](#2-agent-memory-node)
    - [Dónde se registran los datos](#dónde-se-registran-los-datos)
        - [Entendiendo la estructura y formato de datos de la tabla "checkpoints"](#entendiendo-la-estructura-y-formato-de-datos-de-la-tabla-checkpoints)
        - [Estructura de la tabla](#estructura-de-la-tabla-agent-memory-node)
        - [Cómo funciona](#cómo-funciona-agent-memory-node)
    - [Inputs](#inputs-agent-memory-node)
    - [Configuración del Nodo](#configuración-del-nodo-agent-memory-node)
    - [Parámetros Adicionales](#parámetros-adicionales-agent-memory-node)
    - [Outputs](#outputs-agent-memory-node)
    - [Mejores Prácticas](#mejores-prácticas-agent-memory-node)
- [3. State Node](#3-state-node)
    - [Entendiendo el State Node](#entendiendo-el-state-node)
    - [Inputs](#inputs-state-node)
    - [Outputs](#outputs-state-node)
    - [Parámetros Adicionales](#parámetros-adicionales-state-node)
    - [Cómo establecer un State personalizado](#cómo-establecer-un-state-personalizado)
        - [Ejemplo usando JS](#ejemplo-usando-js)
        - [Ejemplo usando Tabla](#ejemplo-usando-tabla)
        - [Ejemplo de Tabla](#ejemplo-de-tabla)
        - [Ejemplo usando API](#ejemplo-usando-api)
    - [Mejores Prácticas](#mejores-prácticas-state-node)
- [4. Agent Node](#4-agent-node)
    - [Entendiendo el Agent Node](#entendiendo-el-agent-node)
    - [Inputs](#inputs-agent-node)
    - [Outputs](#outputs-agent-node)
    - [Configuración del Nodo](#configuración-del-nodo-agent-node)
    - [Parámetros Adicionales](#parámetros-adicionales-agent-node)
    - [Mejores Prácticas](#mejores-prácticas-agent-node)
- [5. LLM Node](#5-llm-node)
    - [Entendiendo el LLM Node](#entendiendo-el-llm-node)
    - [Inputs](#inputs-llm-node)
    - [Outputs](#outputs-llm-node)
    - [Configuración del Nodo](#configuración-del-nodo-llm-node)
    - [Parámetros Adicionales](#parámetros-adicionales-llm-node)
    - [Mejores Prácticas](#mejores-prácticas-llm-node)
- [6. Tool Node](#6-tool-node)
    - [Entendiendo el Tool Node](#entendiendo-el-tool-node)
    - [Inputs](#inputs-tool-node)
    - [Outputs](#outputs-tool-node)
    - [Configuración del Nodo](#configuración-del-nodo-tool-node)
    - [Parámetros Adicionales](#parámetros-adicionales-tool-node)
    - [Mejores Prácticas](#mejores-prácticas-tool-node)
- [7. Condition Node](#7-condition-node)
    - [Entendiendo el Condition Node](#entendiendo-el-condition-node)
        - [Aquí hay una explicación paso a paso de cómo funciona](#aquí-hay-una-explicación-paso-a-paso-de-cómo-funciona-condition-node)
    - [Inputs](#inputs-condition-node)
    - [Outputs](#outputs-condition-node)
    - [Configuración del Nodo](#configuración-del-nodo-condition-node)
    - [Parámetros Adicionales](#parámetros-adicionales-condition-node)
    - [Mejores Prácticas](#mejores-prácticas-condition-node)
- [8. Condition Agent Node](#8-condition-agent-node)
    - [Entendiendo el Condition Agent Node](#entendiendo-el-condition-agent-node)
    - [Inputs](#inputs-condition-agent-node)
    - [Outputs](#outputs-condition-agent-node)
    - [Configuración del Nodo](#configuración-del-nodo-condition-agent-node)
    - [Parámetros Adicionales](#parámetros-adicionales-condition-agent-node)
    - [Mejores Prácticas](#mejores-prácticas-condition-agent-node)
- [9. Loop Node](#9-loop-node)
    - [Entendiendo el Loop Node](#entendiendo-el-loop-node)
        - [Aquí hay un ejemplo de cómo se podría usar el Loop Node](#aquí-hay-un-ejemplo-de-cómo-se-podría-usar-el-loop-node)
    - [Inputs](#inputs-loop-node)
    - [Outputs](#outputs-loop-node)
    - [Configuración del Nodo](#configuración-del-nodo-loop-node)
    - [Parámetros Adicionales](#parámetros-adicionales-loop-node)
    - [Mejores Prácticas](#mejores-prácticas-loop-node)
- [10. End Node](#10-end-node)
    - [Entendiendo el End Node](#entendiendo-el-end-node)
    - [Inputs](#inputs-end-node)
    - [Outputs](#outputs-end-node)
    - [Mejores Prácticas](#mejores-prácticas-end-node)
- [Condition Node vs. Condition Agent Node](#condition-node-vs-condition-agent-node)
    - [Condition Node](#condition-node-comparison)
    - [Condition Agent Node](#condition-agent-node-comparison)
    - [Resumen](#resumen-condition-node-vs-condition-agent-node)
    - [Eligiendo el nodo correcto](#eligiendo-el-nodo-correcto-condition-node-vs-condition-agent-node)
- [Agent Node vs. LLM Node](#agent-node-vs-llm-node)
    - [Agent Node](#agent-node-comparison)
    - [LLM Node](#llm-node-comparison)
    - [Resumen](#resumen-agent-node-vs-llm-node)
    - [Eligiendo el nodo correcto](#eligiendo-el-nodo-correcto-agent-node-vs-llm-node)
- [Relevant Links](#relevant-links)

## Concepto

Construido sobre [LangGraph](https://www.langchain.com/langgraph), la arquitectura Sequential Agents de Flowise facilita el **desarrollo de sistemas agénticos conversacionales estructurando el flujo de trabajo como un grafo cíclico dirigido (DCG)**, permitiendo bucles controlados y procesos iterativos.

Este grafo, compuesto de nodos interconectados, define el flujo secuencial de información y acciones, permitiendo que los agentes procesen entradas, ejecuten tareas y generen respuestas de manera estructurada.

<figure><img src="../../.gitbook/assets/seq-21.svg" alt=""><figcaption></figcaption></figure>

### Entendiendo la Arquitectura DCG de Sequential Agents

Esta arquitectura simplifica la gestión de flujos de trabajo conversacionales complejos definiendo una secuencia clara y comprensible de operaciones a través de su estructura DCG.

Exploremos algunos elementos clave de este enfoque:

{% tabs %}
{% tab title="Principios Fundamentales" %}
* **Procesamiento basado en nodos:** Cada nodo en el grafo representa una unidad discreta de procesamiento, encapsulando su propia funcionalidad como procesamiento de lenguaje, ejecución de herramientas o lógica condicional.
* **Flujo de datos como conexiones:** Los bordes en el grafo representan el flujo de datos entre nodos, donde la salida de un nodo se convierte en la entrada para el siguiente nodo, permitiendo una cadena de pasos de procesamiento.
* **Gestión de estado:** El estado se gestiona como un objeto compartido, persistiendo a lo largo de la conversación. Esto permite a los nodos acceder a información relevante a medida que avanza el flujo de trabajo.
{% endtab %}

{% tab title="Terminología" %}
* **Flow:** El movimiento o dirección de datos dentro del flujo de trabajo. Describe cómo la información pasa entre nodos durante una conversación.
* **Workflow:** El diseño y estructura general del sistema. Es el plano que define la secuencia de nodos, sus conexiones y la lógica que orquesta el flujo de conversación.
* **State:** Una estructura de datos compartida que representa la instantánea actual de la conversación. Incluye el historial de conversación `state.messages` y cualquier variable de State personalizada definida por el usuario.
* **Custom State:** Pares clave-valor definidos por el usuario añadidos al objeto state para almacenar información adicional relevante para el flujo de trabajo.
* **Tool:** Un sistema externo, API o servicio que puede ser accedido y ejecutado por el flujo de trabajo para realizar tareas específicas, como recuperar información, procesar datos o interactuar con otras aplicaciones.
* **Human-in-the-Loop (HITL):** Una característica que permite la intervención humana en el flujo de trabajo, principalmente durante la ejecución de herramientas. Permite que un revisor humano apruebe o rechace una llamada a herramienta antes de que se ejecute.
* **Parallel node execution:** Se refiere a la capacidad de ejecutar múltiples nodos concurrentemente dentro de un flujo de trabajo usando un mecanismo de ramificación. Esto significa que diferentes ramas del flujo de trabajo pueden procesar información o interactuar con herramientas simultáneamente, aunque el flujo general de ejecución permanece secuencial.
{% endtab %}
{% endtabs %}

## Sequential Agents vs Multi-Agents

Si bien tanto los sistemas Multi-Agent como Sequential Agent en Flowise están construidos sobre el framework LangGraph y comparten los mismos principios fundamentales, la arquitectura Sequential Agent proporciona un [nivel más bajo de abstracción](#user-content-fn-1)[^1], ofreciendo un control más granular sobre cada paso del flujo de trabajo.

Los **sistemas Multi-Agent**, que se caracterizan por una estructura jerárquica con un agente supervisor central que delega tareas a agentes trabajadores especializados, **sobresalen en el manejo de flujos de trabajo complejos al dividirlos en sub-tareas manejables**. Esta descomposición en sub-tareas es posible gracias a la preconfiguración de elementos centrales del sistema bajo el capó, como los nodos de condición, que requerirían una configuración manual en un sistema Sequential Agent. Como resultado, los usuarios pueden construir y gestionar equipos de agentes más fácilmente.

En contraste, los **sistemas Sequential Agent** operan como una línea de ensamblaje optimizada, donde los datos fluyen secuencialmente a través de una cadena de nodos, haciéndolos ideales para tareas que demandan un orden preciso de operaciones y refinamiento incremental de datos. Comparado con el sistema Multi-Agent, su acceso de nivel más bajo a la estructura del flujo de trabajo subyacente lo hace fundamentalmente más **flexible y personalizable, ofreciendo ejecución paralela de nodos y control total sobre la lógica del sistema**, incorporando nodos de condición, estado y bucle en el flujo de trabajo, permitiendo la creación de nuevas capacidades de ramificación dinámicas.

## Introduciendo State, Loop y Condition Nodes

Los Sequential Agents de Flowise ofrecen nuevas capacidades para crear sistemas conversacionales que pueden adaptarse a la entrada del usuario, tomar decisiones basadas en el contexto y realizar tareas iterativas.

Estas capacidades son posibles gracias a la introducción de cuatro nuevos nodos principales; el State Node, el Loop Node y dos Condition Nodes.

<figure><img src="../../.gitbook/assets/seq-20.png" alt=""><figcaption></figcaption></figure>

* **State Node:** Definimos State como una estructura de datos compartida que representa la instantánea actual de nuestra aplicación o flujo de trabajo. El State Node nos permite **añadir un State personalizado** a nuestro flujo de trabajo desde el inicio de la conversación. Este State personalizado es accesible y modificable por otros nodos en el flujo de trabajo, permitiendo comportamiento dinámico y compartición de datos.
* **Loop Node:** Este nodo **introduce ciclos controlados** dentro del flujo de trabajo Sequential Agent, permitiendo procesos iterativos donde una secuencia de nodos puede repetirse basada en condiciones específicas. Esto permite a los agentes refinar salidas, recopilar información adicional del usuario o realizar tareas múltiples veces.
* **Condition Nodes:** El Condition y Condition Agent Node proporcionan el control necesario para **crear flujos conversacionales complejos con caminos ramificados**. El Condition Node evalúa condiciones directamente, mientras que el Condition Agent Node usa el razonamiento de un agente para determinar la lógica de ramificación. Esto nos permite guiar dinámicamente el comportamiento del flujo basado en la entrada del usuario, el State personalizado o resultados de acciones tomadas por otros nodos.

## Eligiendo el sistema correcto

Seleccionar el sistema ideal para tu aplicación depende de entender tus necesidades específicas de flujo de trabajo. Factores como la complejidad de la tarea, la necesidad de procesamiento paralelo y tu nivel deseado de control sobre el flujo de datos son todas consideraciones clave.

* **Para simplicidad:** Si tu flujo de trabajo es relativamente directo, donde las tareas pueden completarse una tras otra y por lo tanto no requiere ejecución paralela de nodos o Human-in-the-Loop (HITL), el enfoque Multi-Agent ofrece facilidad de uso y configuración rápida.
* **Para flexibilidad:** Si tu flujo de trabajo necesita ejecución paralela, conversaciones dinámicas, gestión de State personalizado y la capacidad de incorporar HITL, el enfoque **Sequential Agent** proporciona la flexibilidad y control necesarios.

Aquí hay una tabla comparando las implementaciones Multi-Agent y Sequential Agent en Flowise, destacando diferencias clave y consideraciones de diseño:

<table><thead><tr><th width="173.33333333333331"></th><th width="281">Multi-Agent</th><th>Sequential Agent</th></tr></thead><tbody><tr><td>Estructura</td><td><strong>Jerárquica</strong>; Supervisor delega a Trabajadores especializados.</td><td><strong>Lineal, cíclica y/o</strong> <strong>ramificada</strong>; los nodos se conectan en una secuencia, con lógica condicional para ramificación.</td></tr><tr><td>Workflow</td><td>Flexible; diseñado para dividir una tarea compleja en una <strong>secuencia de sub-tareas</strong>, completadas una tras otra.</td><td>Altamente flexible; <strong>soporta ejecución paralela de nodos</strong>, flujos de diálogo complejos, lógica de ramificación y bucles dentro de un único turno de conversación.</td></tr><tr><td>Parallel Node Execution</td><td><strong>No</strong>; Supervisor maneja una tarea a la vez.</td><td><strong>Sí</strong>; puede activar múltiples acciones en paralelo dentro de una única ejecución.</td></tr><tr><td>State Management</td><td><strong>Implícito</strong>; State está en su lugar, pero no es gestionado explícitamente por el desarrollador.</td><td><strong>Explícito</strong>; State está en su lugar, y los desarrolladores pueden definir y gestionar un State inicial o personalizado usando el State Node y el campo "Update State" en varios nodos.</td></tr><tr><td>Tool Usage</td><td>Los <strong>Workers</strong> pueden acceder y usar herramientas según sea necesario.</td><td>Las herramientas son accedidas y ejecutadas a través de <strong>Agent Nodes</strong> y <strong>Tool Nodes</strong>.</td></tr><tr><td>Human-in-the-Loop (HITL)</td><td>HITL <strong>no está soportado.</strong></td><td><strong>Soportado</strong> a través de la característica "Require Approval" del Agent Node y Tool Node, permitiendo revisión humana y aprobación o rechazo de la ejecución de herramientas.</td></tr><tr><td>Complejidad</td><td>Nivel más alto de abstracción; <strong>simplifica el diseño del flujo de trabajo.</strong></td><td>Nivel más bajo de abstracción; <strong>diseño de flujo de trabajo más complejo</strong>, requiriendo planificación cuidadosa de interacciones entre nodos, gestión de State personalizado y lógica condicional.</td></tr><tr><td>Casos de Uso Ideales</td><td><ul><li>Automatización de procesos lineales (ej., extracción de datos, generación de leads).</li><li>Situaciones donde las sub-tareas necesitan completarse una tras otra.</li></ul></td><td><ul><li>Construcción de sistemas conversacionales con flujos dinámicos.</li><li>Flujos de trabajo complejos que requieren ejecución paralela de nodos o lógica de ramificación.</li><li>Situaciones donde se necesita toma de decisiones en múltiples puntos de la conversación.</li></ul></td></tr></tbody></table>

{% hint style="info" %}
**Nota**: Aunque los sistemas Multi-Agent son técnicamente una capa de nivel superior construida sobre la arquitectura Sequential Agent, ofrecen una experiencia de usuario y enfoque distintivos para el diseño de flujos de trabajo. La comparación anterior los trata como sistemas separados para ayudarte a seleccionar la mejor opción para tus necesidades específicas.
{% endhint %}

## Sequential Agents Nodes

Los Sequential Agents introducen una nueva dimensión a Flowise, **introduciendo 10 nodos especializados**, cada uno sirviendo un propósito específico, ofreciendo más control sobre cómo nuestros agentes conversacionales interactúan con los usuarios, procesan información, toman decisiones y ejecutan acciones.

Las siguientes secciones tienen como objetivo proporcionar una comprensión completa de la funcionalidad de cada nodo, entradas, salidas y mejores prácticas, permitiéndote finalmente crear flujos de trabajo conversacionales sofisticados para una variedad de aplicaciones.

<figure><img src="../../.gitbook/assets/seq-00.png" alt=""><figcaption></figcaption></figure>

## 1. Start Node

Como su nombre indica, el Start Node es el **punto de entrada para todos los flujos de trabajo en la arquitectura Sequential Agent**. Recibe la consulta inicial del usuario, inicializa el State de la conversación y pone en marcha el flujo.

<figure><img src="../../.gitbook/assets/seq-02.png" alt="" width="300"><figcaption></figcaption></figure>

### Entendiendo el Start Node

El Start Node asegura que nuestros flujos de trabajo conversacionales tengan la configuración y el contexto necesarios para funcionar correctamente. **Es responsable de configurar funcionalidades clave** que se utilizarán a lo largo del resto del flujo de trabajo:

* **Definiendo el LLM por defecto:** El Start Node requiere que especifiquemos un Chat Model (LLM) compatible con function calling, permitiendo a los agentes en el flujo de trabajo interactuar con herramientas y sistemas externos. Será el LLM por defecto utilizado bajo el capó en el flujo de trabajo.
* **Inicializando Memory:** Opcionalmente podemos conectar un Agent Memory Node para almacenar y recuperar el historial de conversación, permitiendo respuestas más conscientes del contexto.
* **Estableciendo un State personalizado:** Por defecto, el State contiene un array inmutable `state.messages`, que actúa como la transcripción o historial de la conversación entre el usuario y los agentes. El Start Node permite conectar un State personalizado al flujo de trabajo agregando un State Node, permitiendo el almacenamiento de información adicional relevante para tu flujo de trabajo
* **Habilitando moderación:** Opcionalmente, podemos conectar Input Moderation para analizar la entrada del usuario y prevenir que contenido potencialmente dañino sea enviado al LLM.

### Inputs <a name="inputs-start-node"></a>

<table><thead><tr><th width="212"></th><th width="103">Required</th><th>Description</th></tr></thead><tbody><tr><td>Chat Model</td><td><strong>Sí</strong></td><td>El LLM por defecto que alimentará la conversación. Solo compatible con <strong>modelos que son capaces de function calling</strong>.</td></tr><tr><td>Agent Memory Node</td><td>No</td><td>Conecta un Agent Memory Node para <strong>habilitar persistencia y preservación del contexto</strong>.</td></tr><tr><td>State Node</td><td>No</td><td>Conecta un State Node para <strong>establecer un State personalizado</strong>, un contexto compartido que puede ser accedido y modificado por otros nodos en el flujo de trabajo.</td></tr><tr><td>Input Moderation</td><td>No</td><td>Conecta un Moderation Node para <strong>filtrar contenido</strong> detectando texto que podría generar salida dañina, evitando que sea enviado al LLM.</td></tr></tbody></table>

### Outputs <a name="outputs-start-node"></a>

El Start Node puede conectarse a los siguientes nodos como salidas:

* **Agent Node:** Dirige el flujo de conversación a un Agent Node, que puede entonces ejecutar acciones o acceder a herramientas basadas en el contexto de la conversación.
* **LLM Node:** Dirige el flujo de conversación a un LLM Node para procesamiento y generación de respuestas.
* **Condition Agent Node:** Se conecta a un Condition Agent Node para implementar lógica de ramificación basada en la evaluación del agente de la conversación.
* **Condition Node:** Se conecta a un Condition Node para implementar lógica de ramificación basada en condiciones predefinidas.

### Mejores Prácticas <a name="mejores-prácticas-start-node"></a>

{% tabs %}
{% tab title="Pro Tips" %}
**Elige el Chat Model correcto**

Asegúrate de que tu LLM seleccionado soporte function calling, una característica clave para habilitar interacciones agente-herramienta. Además, elige un LLM que se alinee con la complejidad y requisitos de tu aplicación. Puedes sobrescribir el LLM por defecto estableciéndolo a nivel de nodo Agent/LLM/Condition Agent cuando sea necesario.

**Considera el contexto y la persistencia**

Si tu caso de uso lo demanda, utiliza Agent Memory Node para mantener el contexto y personalizar interacciones.
{% endtab %}

{% tab title="Potential Pitfalls" %}
**Selección incorrecta de Chat Model (LLM)**

* **Problema:** El Chat Model seleccionado en el Start Node no es adecuado para las tareas o capacidades previstas del flujo de trabajo, resultando en bajo rendimiento o respuestas inexactas.
* **Ejemplo:** Un flujo de trabajo requiere un Chat Model con fuertes capacidades de resumen, pero el Start Node selecciona un modelo optimizado para generación de código, llevando a resúmenes inadecuados.
* **Solución:** Elige un Chat Model que se alinee con los requisitos específicos de tu flujo de trabajo. Considera las fortalezas, debilidades y los tipos de tareas en las que sobresale. Consulta la documentación y experimenta con diferentes modelos para encontrar el mejor ajuste.

**Pasar por alto la configuración del Agent Memory Node**

* **Problema:** El Agent Memory Node no está correctamente conectado o configurado, resultando en la pérdida de datos del historial de conversación entre sesiones.
* **Ejemplo:** Pretendes usar memoria persistente para almacenar preferencias de usuario, pero el Agent Memory Node no está conectado al Start Node, causando que las preferencias se reinicien en cada nueva conversación.
* **Solución:** Asegúrate de que el Agent Memory Node esté conectado al Start Node y configurado con la base de datos apropiada (SQLite). Para la mayoría de los casos de uso, la base de datos SQLite por defecto será suficiente.

**Moderación de entrada inadecuada**

* **Problema:** El "Input Moderation" no está habilitado o configurado correctamente, permitiendo que entrada de usuario potencialmente dañina o inapropiada llegue al LLM y genere respuestas indeseables.
* **Ejemplo:** Un usuario envía lenguaje ofensivo, pero la moderación de entrada falla en detectarlo o no está configurada en absoluto, permitiendo que la consulta llegue al LLM.
* **Solución:** Agrega y configura un nodo de moderación de entrada en el Start Node para filtrar lenguaje potencialmente dañino o inapropiado. Personaliza la configuración de moderación para alinearse con tus requisitos específicos y casos de uso.
{% endtab %}
{% endtabs %}

## 2. Agent Memory Node

El Agent Memory Node **proporciona un mecanismo para almacenamiento de memoria persistente**, permitiendo que el flujo de trabajo Sequential Agent retenga el historial de conversación `state.messages` y cualquier State personalizado previamente definido a través de múltiples interacciones.

Esta memoria a largo plazo es esencial para que los agentes aprendan de interacciones previas, mantengan el contexto durante conversaciones extendidas y proporcionen respuestas más relevantes.

<figure><img src="../../.gitbook/assets/seq-03.png" alt="" width="299"><figcaption></figcaption></figure>

### Dónde se registran los datos

Por defecto, Flowise utiliza su **base de datos SQLite integrada** para almacenar el historial de conversación y datos de state personalizados, creando una tabla "**checkpoints**" para gestionar esta información persistente.

#### Entendiendo la estructura y formato de datos de la tabla "checkpoints"

Esta tabla **almacena instantáneas del State del sistema en varios puntos durante una conversación**, permitiendo la persistencia y recuperación del historial de conversación. Cada fila representa un punto específico o "checkpoint" en la ejecución del flujo de trabajo.

<figure><img src="../../.gitbook/assets/seq-12.png" alt=""><figcaption></figcaption></figure>

#### Estructura de la tabla <a name="estructura-de-la-tabla-agent-memory-node"></a>

* **thread_id:** Un identificador único que representa una sesión de conversación específica, **nuestro ID de sesión**. Agrupa todos los checkpoints relacionados con una única ejecución del flujo de trabajo.
* **checkpoint_id:** Un identificador único para cada paso de ejecución (ejecución de nodo) dentro del flujo de trabajo. Ayuda a rastrear el orden de operaciones e identificar el State en cada paso.
* **parent_id:** Indica el checkpoint_id del paso de ejecución precedente que llevó al checkpoint actual. Esto establece una relación jerárquica entre checkpoints, permitiendo la reconstrucción del flujo de ejecución del flujo de trabajo.
* **checkpoint:** Contiene una cadena JSON que representa el State actual del flujo de trabajo en ese checkpoint específico. Esto incluye los valores de variables, los mensajes intercambiados y cualquier otro dato relevante capturado en ese punto de la ejecución.
* **metadata:** Proporciona contexto adicional sobre el checkpoint, específicamente relacionado con operaciones de nodo.

#### Cómo funciona <a name="cómo-funciona-agent-memory-node"></a>

A medida que un flujo de trabajo Sequential Agent se ejecuta, el sistema registra un checkpoint en esta tabla para cada paso significativo. Este mecanismo proporciona varios beneficios:

* **Seguimiento de ejecución:** Los checkpoints permiten al sistema entender el camino de ejecución y el orden de operaciones dentro del flujo de trabajo.
* **Gestión de State:** Los checkpoints almacenan el State del flujo de trabajo en cada paso, incluyendo valores de variables, historial de conversación y cualquier otro dato relevante. Esto permite al sistema mantener conciencia contextual y tomar decisiones informadas basadas en el State actual.
* **Reanudación del flujo de trabajo:** Si el flujo de trabajo es pausado o interrumpido (por ejemplo, debido a un error del sistema o solicitud del usuario), el sistema puede usar los checkpoints almacenados para reanudar la ejecución desde el último State registrado. Esto asegura que la conversación o tarea continúe desde donde se quedó, preservando el progreso del usuario y previniendo pérdida de datos.

### **Inputs** <a name="inputs-agent-memory-node"></a>

El Agent Memory Node **no tiene conexiones de entrada específicas**.

### Configuración del Nodo <a name="configuración-del-nodo-agent-memory-node"></a>

<table><thead><tr><th width="189"></th><th width="107">Required</th><th>Description</th></tr></thead><tbody><tr><td>Database</td><td><strong>Sí</strong></td><td>El tipo de base de datos utilizada para almacenar el historial de conversación. Actualmente, <strong>solo SQLite está soportado</strong>.</td></tr></tbody></table>

### Parámetros Adicionales <a name="parámetros-adicionales-agent-memory-node"></a>

<table><thead><tr><th width="189"></th><th width="107">Required</th><th>Description</th></tr></thead><tbody><tr><td>Database File Path</td><td>No</td><td>La ruta del archivo a la base de datos SQLite. <strong>Si no se proporciona, el sistema utilizará una ubicación por defecto</strong>.</td></tr></tbody></table>

### **Outputs** <a name="outputs-agent-memory-node"></a>

El Agent Memory Node interactúa únicamente con el **Start Node**, haciendo que el historial de conversación esté disponible desde el comienzo del flujo de trabajo.

### **Mejores Prácticas** <a name="mejores-prácticas-agent-memory-node"></a>

{% tabs %}
{% tab title="Pro Tips" %}
**Uso estratégico**

Emplea Agent Memory solo cuando sea necesario. Para interacciones simples y sin estado, podría ser excesivo. Resérvalo para escenarios donde retener información a través de turnos o sesiones es esencial.
{% endtab %}

{% tab title="Potential Pitfalls" %}
**Sobrecarga innecesaria**

* **El Problema:** Usar Agent Memory para cada interacción, incluso cuando no es necesario, introduce sobrecarga innecesaria de almacenamiento y procesamiento. Esto puede ralentizar los tiempos de respuesta y aumentar el consumo de recursos.
* **Ejemplo:** Un chatbot simple del clima que proporciona información basada en una única solicitud del usuario no necesita almacenar historial de conversación.
* **Solución:** Analiza los requisitos de tu sistema y utiliza Agent Memory solo cuando el almacenamiento persistente de datos es esencial para la funcionalidad o experiencia del usuario.
{% endtab %}
{% endtabs %}

## 3. State Node

El State Node, que solo puede conectarse al Start Node, **proporciona un mecanismo para establecer un State definido por el usuario o personalizado** en nuestro flujo de trabajo desde el inicio de la conversación. Este State personalizado es un objeto JSON que es compartido y puede ser actualizado por nodos en el grafo, pasando de un nodo a otro a medida que el flujo progresa.

<figure><img src="../../.gitbook/assets/seq-04.png" alt="" width="299"><figcaption></figcaption></figure>

### Entendiendo el State Node

Por defecto, el State incluye un array `state.messages`, que actúa como nuestro historial de conversación. Este array almacena todos los mensajes intercambiados entre el usuario y los agentes, o cualquier otro actor en el flujo de trabajo, preservándolo durante toda la ejecución del flujo de trabajo.

Dado que por definición este array `state.messages` es inmutable y no puede ser modificado, **el propósito del State Node es permitirnos definir pares clave-valor personalizados**, expandiendo el objeto state para contener cualquier información adicional relevante para nuestro flujo de trabajo.

{% hint style="info" %}
Cuando no se usa **Agent Memory Node**, el State opera en memoria y no persiste para uso futuro.
{% endhint %}

### Inputs <a name="inputs-state-node"></a>

El State Node **no tiene conexiones de entrada específicas**.

### Outputs <a name="outputs-state-node"></a>

El State Node solo puede conectarse al **Start Node**, permitiendo la configuración de un State personalizado desde el inicio del flujo de trabajo y permitiendo que otros nodos accedan y potencialmente modifiquen este State compartido personalizado.

### Parámetros Adicionales <a name="parámetros-adicionales-state-node"></a>

<table><thead><tr><th width="157"></th><th width="113">Required</th><th>Description</th></tr></thead><tbody><tr><td>Custom State</td><td><strong>Sí</strong></td><td>Un objeto JSON que representa el <strong>State inicial personalizado del flujo de trabajo</strong>. Este objeto puede contener cualquier par clave-valor relevante para la aplicación.</td></tr></tbody></table>

### Cómo establecer un State personalizado <a name="cómo-establecer-un-state-personalizado"></a> <a href="#alert-dialog-title" id="alert-dialog-title"></a>

Especifica la **clave**, **tipo de operación** y **valor por defecto** para el objeto state. El tipo de operación puede ser "Replace" o "Append".

* **Replace**
  1. Reemplaza el valor existente con el nuevo valor.
  2. Si el nuevo valor es null, se mantendrá el valor existente.
* **Append**
  1. Añade el nuevo valor al valor existente.
  2. Los valores por defecto pueden estar vacíos o ser un array. Ej: \["a", "b"]
  3. El valor final es un array.

#### Ejemplo usando JS <a name="ejemplo-usando-js"></a>

{% code overflow="wrap" %}
```javascript
{
    aggregate: {
        value: (x, y) => x.concat(y), // aquí añadimos el nuevo mensaje a los mensajes existentes
        default: () => []
    }
}
content_copy
download
Use code with caution.
Markdown

{% endcode %}

Ejemplo usando Tabla <a name="ejemplo-usando-tabla"></a>

Para definir un State personalizado usando la interfaz de tabla en el State Node, sigue estos pasos:

Agregar elemento: Haz clic en el botón "+ Add Item" para agregar filas a la tabla. Cada fila representa un par clave-valor en tu State personalizado.

Especificar claves: En la columna "Key", ingresa el nombre de cada clave que deseas definir en tu objeto state. Por ejemplo, podrías tener claves como "userName", "userLocation", etc.

Elegir operaciones: En la columna "Operation", selecciona la operación deseada para cada clave. Tienes dos opciones:

Replace: Esto reemplazará el valor existente de la clave con el nuevo valor proporcionado por un nodo. Si el nuevo valor es null, se mantendrá el valor existente.

Append: Esto añadirá el nuevo valor al valor existente de la clave. El valor final será un array.

Establecer valores por defecto: En la columna "Default Value", ingresa el valor inicial para cada clave. Este valor se usará si ningún otro nodo proporciona un valor para la clave. El valor por defecto puede estar vacío o ser un array.

Ejemplo de Tabla <a name="ejemplo-de-tabla"></a>
Key	Operation	Default Value
userName	Replace	null
<figure><img src="../../.gitbook/assets/seq-14.png" alt="" width="375"><figcaption></figcaption></figure>


Esta tabla define una clave en el State personalizado: userName.

La clave userName usará la operación "Replace", lo que significa que su valor se actualizará cuando un nodo proporcione un nuevo valor.

La clave userName tiene un valor por defecto de null, indicando que no tiene valor inicial.

{% hint style="info" %}
Recuerda que este enfoque basado en tabla es una alternativa a definir el State personalizado usando JavaScript. Ambos métodos logran el mismo resultado.
{% endhint %}

Ejemplo usando API <a name="ejemplo-usando-api"></a>
{
    "question": "hello",
    "overrideConfig": {
        "stateMemory": [
            {
                "Key": "userName",
                "Operation": "Replace",
                "Default Value": "somevalue"
            }
        ]
    }
}
content_copy
download
Use code with caution.
Json
Mejores Prácticas <a name="mejores-prácticas-state-node"></a>

{% tabs %}
{% tab title="Pro-Tips" %}
Planifica la estructura de tu State personalizado

Antes de construir tu flujo de trabajo, diseña la estructura de tu State personalizado. Un State personalizado bien organizado hará que tu flujo de trabajo sea más fácil de entender, gestionar y depurar.

Usa nombres de clave significativos

Elige nombres de clave descriptivos y consistentes que indiquen claramente el propósito de los datos que contienen. Esto