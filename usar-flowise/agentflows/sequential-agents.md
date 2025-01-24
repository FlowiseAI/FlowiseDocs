---
description: Aprende los Fundamentos de Sequential Agents en Flowise, escrito por @toi500
---

# Sequential Agents

Esta guía ofrece una visión completa de la arquitectura de Sequential Agent AI dentro de Flowise, explorando sus componentes principales y principios de diseño del flujo de trabajo.

{% hint style="warning" %}
**Aviso**: Esta documentación está destinada a ayudar a los usuarios de Flowise a entender y construir flujos de trabajo conversacionales usando la arquitectura Sequential Agent. No pretende ser una referencia técnica exhaustiva del framework LangGraph y no debe interpretarse como una definición de estándares de la industria o conceptos fundamentales de LangGraph.
{% endhint %}

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

***

## Sequential Agents vs Multi-Agents

Si bien tanto los sistemas Multi-Agent como Sequential Agent en Flowise están construidos sobre el framework LangGraph y comparten los mismos principios fundamentales, la arquitectura Sequential Agent proporciona un [nivel más bajo de abstracción](#user-content-fn-1)[^1], ofreciendo un control más granular sobre cada paso del flujo de trabajo.

Los **sistemas Multi-Agent**, que se caracterizan por una estructura jerárquica con un agente supervisor central que delega tareas a agentes trabajadores especializados, **sobresalen en el manejo de flujos de trabajo complejos al dividirlos en sub-tareas manejables**. Esta descomposición en sub-tareas es posible gracias a la preconfiguración de elementos centrales del sistema bajo el capó, como los nodos de condición, que requerirían una configuración manual en un sistema Sequential Agent. Como resultado, los usuarios pueden construir y gestionar equipos de agentes más fácilmente.

En contraste, los **sistemas Sequential Agent** operan como una línea de ensamblaje optimizada, donde los datos fluyen secuencialmente a través de una cadena de nodos, haciéndolos ideales para tareas que demandan un orden preciso de operaciones y refinamiento incremental de datos. Comparado con el sistema Multi-Agent, su acceso de nivel más bajo a la estructura del flujo de trabajo subyacente lo hace fundamentalmente más **flexible y personalizable, ofreciendo ejecución paralela de nodos y control total sobre la lógica del sistema**, incorporando nodos de condición, estado y bucle en el flujo de trabajo, permitiendo la creación de nuevas capacidades de ramificación dinámicas.

### Introduciendo State, Loop y Condition Nodes

Los Sequential Agents de Flowise ofrecen nuevas capacidades para crear sistemas conversacionales que pueden adaptarse a la entrada del usuario, tomar decisiones basadas en el contexto y realizar tareas iterativas.

Estas capacidades son posibles gracias a la introducción de cuatro nuevos nodos principales; el State Node, el Loop Node y dos Condition Nodes.

<figure><img src="../../.gitbook/assets/seq-20.png" alt=""><figcaption></figcaption></figure>

* **State Node:** Definimos State como una estructura de datos compartida que representa la instantánea actual de nuestra aplicación o flujo de trabajo. El State Node nos permite **añadir un State personalizado** a nuestro flujo de trabajo desde el inicio de la conversación. Este State personalizado es accesible y modificable por otros nodos en el flujo de trabajo, permitiendo comportamiento dinámico y compartición de datos.
* **Loop Node:** Este nodo **introduce ciclos controlados** dentro del flujo de trabajo Sequential Agent, permitiendo procesos iterativos donde una secuencia de nodos puede repetirse basada en condiciones específicas. Esto permite a los agentes refinar salidas, recopilar información adicional del usuario o realizar tareas múltiples veces.
* **Condition Nodes:** El Condition y Condition Agent Node proporcionan el control necesario para **crear flujos conversacionales complejos con caminos ramificados**. El Condition Node evalúa condiciones directamente, mientras que el Condition Agent Node usa el razonamiento de un agente para determinar la lógica de ramificación. Esto nos permite guiar dinámicamente el comportamiento del flujo basado en la entrada del usuario, el State personalizado o resultados de acciones tomadas por otros nodos.

### Eligiendo el sistema correcto

Seleccionar el sistema ideal para tu aplicación depende de entender tus necesidades específicas de flujo de trabajo. Factores como la complejidad de la tarea, la necesidad de procesamiento paralelo y tu nivel deseado de control sobre el flujo de datos son todas consideraciones clave.

* **Para simplicidad:** Si tu flujo de trabajo es relativamente directo, donde las tareas pueden completarse una tras otra y por lo tanto no requiere ejecución paralela de nodos o Human-in-the-Loop (HITL), el enfoque Multi-Agent ofrece facilidad de uso y configuración rápida.
* **Para flexibilidad:** Si tu flujo de trabajo necesita ejecución paralela, conversaciones dinámicas, gestión de State personalizado y la capacidad de incorporar HITL, el enfoque **Sequential Agent** proporciona la flexibilidad y control necesarios.

Aquí hay una tabla comparando las implementaciones Multi-Agent y Sequential Agent en Flowise, destacando diferencias clave y consideraciones de diseño:

<table><thead><tr><th width="173.33333333333331"></th><th width="281">Multi-Agent</th><th>Sequential Agent</th></tr></thead><tbody><tr><td>Estructura</td><td><strong>Jerárquica</strong>; Supervisor delega a Trabajadores especializados.</td><td><strong>Lineal, cíclica y/o</strong> <strong>ramificada</strong>; los nodos se conectan en una secuencia, con lógica condicional para ramificación.</td></tr><tr><td>Workflow</td><td>Flexible; diseñado para dividir una tarea compleja en una <strong>secuencia de sub-tareas</strong>, completadas una tras otra.</td><td>Altamente flexible; <strong>soporta ejecución paralela de nodos</strong>, flujos de diálogo complejos, lógica de ramificación y bucles dentro de un único turno de conversación.</td></tr><tr><td>Parallel Node Execution</td><td><strong>No</strong>; Supervisor maneja una tarea a la vez.</td><td><strong>Sí</strong>; puede activar múltiples acciones en paralelo dentro de una única ejecución.</td></tr><tr><td>State Management</td><td><strong>Implícito</strong>; State está en su lugar, pero no es gestionado explícitamente por el desarrollador.</td><td><strong>Explícito</strong>; State está en su lugar, y los desarrolladores pueden definir y gestionar un State inicial o personalizado usando el State Node y el campo "Update State" en varios nodos.</td></tr><tr><td>Tool Usage</td><td>Los <strong>Workers</strong> pueden acceder y usar herramientas según sea necesario.</td><td>Las herramientas son accedidas y ejecutadas a través de <strong>Agent Nodes</strong> y <strong>Tool Nodes</strong>.</td></tr><tr><td>Human-in-the-Loop (HITL)</td><td>HITL <strong>no está soportado.</strong></td><td><strong>Soportado</strong> a través de la característica "Require Approval" del Agent Node y Tool Node, permitiendo revisión humana y aprobación o rechazo de la ejecución de herramientas.</td></tr><tr><td>Complejidad</td><td>Nivel más alto de abstracción; <strong>simplifica el diseño del flujo de trabajo.</strong></td><td>Nivel más bajo de abstracción; <strong>diseño de flujo de trabajo más complejo</strong>, requiriendo planificación cuidadosa de interacciones entre nodos, gestión de State personalizado y lógica condicional.</td></tr><tr><td>Casos de Uso Ideales</td><td><ul><li>Automatización de procesos lineales (ej., extracción de datos, generación de leads).</li><li>Situaciones donde las sub-tareas necesitan completarse una tras otra.</li></ul></td><td><ul><li>Construcción de sistemas conversacionales con flujos dinámicos.</li><li>Flujos de trabajo complejos que requieren ejecución paralela de nodos o lógica de ramificación.</li><li>Situaciones donde se necesita toma de decisiones en múltiples puntos de la conversación.</li></ul></td></tr></tbody></table>

{% hint style="info" %}
**Nota**: Aunque los sistemas Multi-Agent son técnicamente una capa de nivel superior construida sobre la arquitectura Sequential Agent, ofrecen una experiencia de usuario y enfoque distintivos para el diseño de flujos de trabajo. La comparación anterior los trata como sistemas separados para ayudarte a seleccionar la mejor opción para tus necesidades específicas.
{% endhint %}

***

## Sequential Agents Nodes

Los Sequential Agents introducen una nueva dimensión a Flowise, **introduciendo 10 nodos especializados**, cada uno sirviendo un propósito específico, ofreciendo más control sobre cómo nuestros agentes conversacionales interactúan con los usuarios, procesan información, toman decisiones y ejecutan acciones.

Las siguientes secciones tienen como objetivo proporcionar una comprensión completa de la funcionalidad de cada nodo, entradas, salidas y mejores prácticas, permitiéndote finalmente crear flujos de trabajo conversacionales sofisticados para una variedad de aplicaciones.

<figure><img src="../../.gitbook/assets/seq-00.png" alt=""><figcaption></figcaption></figure>

***

## 1. Start Node

Como su nombre indica, el Start Node es el **punto de entrada para todos los flujos de trabajo en la arquitectura Sequential Agent**. Recibe la consulta inicial del usuario, inicializa el State de la conversación y pone en marcha el flujo.

<figure><img src="../../.gitbook/assets/seq-02.png" alt="" width="300"><figcaption></figcaption></figure>

### Entendiendo el Start Node

El Start Node asegura que nuestros flujos de trabajo conversacionales tengan la configuración y el contexto necesarios para funcionar correctamente. **Es responsable de configurar funcionalidades clave** que se utilizarán a lo largo del resto del flujo de trabajo:

* **Definiendo el LLM por defecto:** El Start Node requiere que especifiquemos un Chat Model (LLM) compatible con function calling, permitiendo a los agentes en el flujo de trabajo interactuar con herramientas y sistemas externos. Será el LLM por defecto utilizado bajo el capó en el flujo de trabajo.
* **Inicializando Memory:** Opcionalmente podemos conectar un Agent Memory Node para almacenar y recuperar el historial de conversación, permitiendo respuestas más conscientes del contexto.
* **Estableciendo un State personalizado:** Por defecto, el State contiene un array inmutable `state.messages`, que actúa como la transcripción o historial de la conversación entre el usuario y los agentes. El Start Node permite conectar un State personalizado al flujo de trabajo agregando un State Node, permitiendo el almacenamiento de información adicional relevante para tu flujo de trabajo
* **Habilitando moderación:** Opcionalmente, podemos conectar Input Moderation para analizar la entrada del usuario y prevenir que contenido potencialmente dañino sea enviado al LLM.

### Inputs

<table><thead><tr><th width="212"></th><th width="103">Required</th><th>Description</th></tr></thead><tbody><tr><td>Chat Model</td><td><strong>Sí</strong></td><td>El LLM por defecto que alimentará la conversación. Solo compatible con <strong>modelos que son capaces de function calling</strong>.</td></tr><tr><td>Agent Memory Node</td><td>No</td><td>Conecta un Agent Memory Node para <strong>habilitar persistencia y preservación del contexto</strong>.</td></tr><tr><td>State Node</td><td>No</td><td>Conecta un State Node para <strong>establecer un State personalizado</strong>, un contexto compartido que puede ser accedido y modificado por otros nodos en el flujo de trabajo.</td></tr><tr><td>Input Moderation</td><td>No</td><td>Conecta un Moderation Node para <strong>filtrar contenido</strong> detectando texto que podría generar salida dañina, evitando que sea enviado al LLM.</td></tr></tbody></table>

### Outputs

El Start Node puede conectarse a los siguientes nodos como salidas:

* **Agent Node:** Dirige el flujo de conversación a un Agent Node, que puede entonces ejecutar acciones o acceder a herramientas basadas en el contexto de la conversación.
* **LLM Node:** Dirige el flujo de conversación a un LLM Node para procesamiento y generación de respuestas.
* **Condition Agent Node:** Se conecta a un Condition Agent Node para implementar lógica de ramificación basada en la evaluación del agente de la conversación.
* **Condition Node:** Se conecta a un Condition Node para implementar lógica de ramificación basada en condiciones predefinidas.

### Mejores Prácticas

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

#### Estructura de la tabla

* **thread_id:** Un identificador único que representa una sesión de conversación específica, **nuestro ID de sesión**. Agrupa todos los checkpoints relacionados con una única ejecución del flujo de trabajo.
* **checkpoint_id:** Un identificador único para cada paso de ejecución (ejecución de nodo) dentro del flujo de trabajo. Ayuda a rastrear el orden de operaciones e identificar el State en cada paso.
* **parent_id:** Indica el checkpoint_id del paso de ejecución precedente que llevó al checkpoint actual. Esto establece una relación jerárquica entre checkpoints, permitiendo la reconstrucción del flujo de ejecución del flujo de trabajo.
* **checkpoint:** Contiene una cadena JSON que representa el State actual del flujo de trabajo en ese checkpoint específico. Esto incluye los valores de variables, los mensajes intercambiados y cualquier otro dato relevante capturado en ese punto de la ejecución.
* **metadata:** Proporciona contexto adicional sobre el checkpoint, específicamente relacionado con operaciones de nodo.

#### Cómo funciona

A medida que un flujo de trabajo Sequential Agent se ejecuta, el sistema registra un checkpoint en esta tabla para cada paso significativo. Este mecanismo proporciona varios beneficios:

* **Seguimiento de ejecución:** Los checkpoints permiten al sistema entender el camino de ejecución y el orden de operaciones dentro del flujo de trabajo.
* **Gestión de State:** Los checkpoints almacenan el State del flujo de trabajo en cada paso, incluyendo valores de variables, historial de conversación y cualquier otro dato relevante. Esto permite al sistema mantener conciencia contextual y tomar decisiones informadas basadas en el State actual.
* **Reanudación del flujo de trabajo:** Si el flujo de trabajo es pausado o interrumpido (por ejemplo, debido a un error del sistema o solicitud del usuario), el sistema puede usar los checkpoints almacenados para reanudar la ejecución desde el último State registrado. Esto asegura que la conversación o tarea continúe desde donde se quedó, preservando el progreso del usuario y previniendo pérdida de datos.

### **Inputs**

El Agent Memory Node **no tiene conexiones de entrada específicas**.

### Configuración del Nodo

<table><thead><tr><th width="189"></th><th width="107">Required</th><th>Description</th></tr></thead><tbody><tr><td>Database</td><td><strong>Sí</strong></td><td>El tipo de base de datos utilizada para almacenar el historial de conversación. Actualmente, <strong>solo SQLite está soportado</strong>.</td></tr></tbody></table>

### Parámetros Adicionales

<table><thead><tr><th width="189"></th><th width="107">Required</th><th>Description</th></tr></thead><tbody><tr><td>Database File Path</td><td>No</td><td>La ruta del archivo a la base de datos SQLite. <strong>Si no se proporciona, el sistema utilizará una ubicación por defecto</strong>.</td></tr></tbody></table>

### **Outputs**

El Agent Memory Node interactúa únicamente con el **Start Node**, haciendo que el historial de conversación esté disponible desde el comienzo del flujo de trabajo.

### **Mejores Prácticas**

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

***

## 3. State Node

El State Node, que solo puede conectarse al Start Node, **proporciona un mecanismo para establecer un State definido por el usuario o personalizado** en nuestro flujo de trabajo desde el inicio de la conversación. Este State personalizado es un objeto JSON que es compartido y puede ser actualizado por nodos en el grafo, pasando de un nodo a otro a medida que el flujo progresa.

<figure><img src="../../.gitbook/assets/seq-04.png" alt="" width="299"><figcaption></figcaption></figure>

### Entendiendo el State Node

Por defecto, el State incluye un array `state.messages`, que actúa como nuestro historial de conversación. Este array almacena todos los mensajes intercambiados entre el usuario y los agentes, o cualquier otro actor en el flujo de trabajo, preservándolo durante toda la ejecución del flujo de trabajo.

Dado que por definición este array `state.messages` es inmutable y no puede ser modificado, **el propósito del State Node es permitirnos definir pares clave-valor personalizados**, expandiendo el objeto state para contener cualquier información adicional relevante para nuestro flujo de trabajo.

{% hint style="info" %}
Cuando no se usa **Agent Memory Node**, el State opera en memoria y no persiste para uso futuro.
{% endhint %}

### Inputs

El State Node **no tiene conexiones de entrada específicas**.

### Outputs

El State Node solo puede conectarse al **Start Node**, permitiendo la configuración de un State personalizado desde el inicio del flujo de trabajo y permitiendo que otros nodos accedan y potencialmente modifiquen este State compartido personalizado.

### Parámetros Adicionales

<table><thead><tr><th width="157"></th><th width="113">Required</th><th>Description</th></tr></thead><tbody><tr><td>Custom State</td><td><strong>Sí</strong></td><td>Un objeto JSON que representa el <strong>State inicial personalizado del flujo de trabajo</strong>. Este objeto puede contener cualquier par clave-valor relevante para la aplicación.</td></tr></tbody></table>

### Cómo establecer un State personalizado <a href="#alert-dialog-title" id="alert-dialog-title"></a>

Especifica la **clave**, **tipo de operación** y **valor por defecto** para el objeto state. El tipo de operación puede ser "Replace" o "Append".

* **Replace**
  1. Reemplaza el valor existente con el nuevo valor.
  2. Si el nuevo valor es null, se mantendrá el valor existente.
* **Append**
  1. Añade el nuevo valor al valor existente.
  2. Los valores por defecto pueden estar vacíos o ser un array. Ej: \["a", "b"]
  3. El valor final es un array.

#### Ejemplo usando JS

{% code overflow="wrap" %}
```javascript
{
    aggregate: {
        value: (x, y) => x.concat(y), // aquí añadimos el nuevo mensaje a los mensajes existentes
        default: () => []
    }
}
```
{% endcode %}

#### Ejemplo usando Tabla

Para definir un State personalizado usando la interfaz de tabla en el State Node, sigue estos pasos:

1. **Agregar elemento:** Haz clic en el botón "+ Add Item" para agregar filas a la tabla. Cada fila representa un par clave-valor en tu State personalizado.
2. **Especificar claves:** En la columna "Key", ingresa el nombre de cada clave que deseas definir en tu objeto state. Por ejemplo, podrías tener claves como "userName", "userLocation", etc.
3. **Elegir operaciones:** En la columna "Operation", selecciona la operación deseada para cada clave. Tienes dos opciones:
   * **Replace:** Esto reemplazará el valor existente de la clave con el nuevo valor proporcionado por un nodo. Si el nuevo valor es null, se mantendrá el valor existente.
   * **Append:** Esto añadirá el nuevo valor al valor existente de la clave. El valor final será un array.
4. **Establecer valores por defecto:** En la columna "Default Value", ingresa el valor inicial para cada clave. Este valor se usará si ningún otro nodo proporciona un valor para la clave. El valor por defecto puede estar vacío o ser un array.

#### Ejemplo de Tabla

| Key      | Operation | Default Value |
| -------- | --------- | ------------- |
| userName | Replace   | null          |

<figure><img src="../../.gitbook/assets/seq-14.png" alt="" width="375"><figcaption></figcaption></figure>

1. Esta tabla define una clave en el State personalizado: `userName`.
2. La clave `userName` usará la operación "Replace", lo que significa que su valor se actualizará cuando un nodo proporcione un nuevo valor.
3. La clave `userName` tiene un valor por defecto de _null_, indicando que no tiene valor inicial.

{% hint style="info" %}
Recuerda que este enfoque basado en tabla es una alternativa a definir el State personalizado usando JavaScript. Ambos métodos logran el mismo resultado.
{% endhint %}

#### Ejemplo usando API

```json
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
```

### Mejores Prácticas

{% tabs %}
{% tab title="Pro-Tips" %}
**Planifica la estructura de tu State personalizado**

Antes de construir tu flujo de trabajo, diseña la estructura de tu State personalizado. Un State personalizado bien organizado hará que tu flujo de trabajo sea más fácil de entender, gestionar y depurar.

**Usa nombres de clave significativos**

Elige nombres de clave descriptivos y consistentes que indiquen claramente el propósito de los datos que contienen. Esto mejorará la legibilidad de tu código y facilitará que otros (o tú en el futuro) entiendan cómo se está usando el State personalizado.

**Mantén el State personalizado mínimo**

Solo almacena información en el State personalizado que sea esencial para la lógica del flujo de trabajo y la toma de decisiones.

**Considera la persistencia del State**

Si necesitas preservar el State a través de múltiples sesiones de conversación (por ejemplo, para preferencias de usuario, historial de pedidos, etc.), usa el Agent Memory Node para almacenar el State en una base de datos persistente.
{% endtab %}

{% tab title="Potential Pitfalls" %}
**Actualizaciones Inconsistentes del State**

* **Problema:** Actualizar el State personalizado en múltiples nodos sin una estrategia clara puede llevar a inconsistencias y comportamiento inesperado.
* **Ejemplo**
  1. Agent 1 actualiza `orderStatus` a "Payment Confirmed".
  2. Agent 2, en una rama diferente, actualiza `orderStatus` a "Order Complete" sin verificar el estado anterior.
* **Solución:** Usa Conditions Nodes para controlar el flujo de las actualizaciones del State personalizado y asegura que las transiciones del State personalizado ocurran de manera lógica y consistente.
{% endtab %}
{% endtabs %}

***

## 4. Agent Node

El Agent Node es un **componente central de la arquitectura Sequential Agent**. Actúa como un tomador de decisiones y orquestador dentro de nuestro flujo de trabajo.

<figure><img src="../../.gitbook/assets/sa-agent.png" alt="" width="268"><figcaption></figcaption></figure>

### Entendiendo el Agent Node

Al recibir entrada de nodos precedentes, que siempre incluye el historial completo de conversación `state.messages` y cualquier State personalizado en ese punto de la ejecución, el Agent Node usa su "persona" definida, establecida por el System Prompt, para determinar si se necesitan herramientas externas para cumplir con la solicitud del usuario.

* Si se requieren herramientas, el Agent Node selecciona y ejecuta autónomamente la herramienta apropiada. Esta ejecución puede ser automática o, para tareas sensibles, requerir aprobación humana (HITL) antes de proceder. Una vez que la herramienta completa su operación, el Agent Node recibe los resultados, los procesa usando el Chat Model (LLM) designado y genera una respuesta completa.
* En casos donde no se necesitan herramientas, el Agent Node aprovecha directamente el Chat Model (LLM) para formular una respuesta basada en el contexto actual de la conversación.

### Inputs

<table><thead><tr><th width="195"></th><th width="107">Required</th><th>Description</th></tr></thead><tbody><tr><td>External Tools</td><td>No</td><td>Proporciona al Agent Node <strong>acceso a un conjunto de herramientas externas</strong>, permitiéndole realizar acciones y recuperar información.</td></tr><tr><td>Chat Model</td><td>No</td><td>Agrega un nuevo Chat Model para <strong>sobrescribir el Chat Model por defecto</strong> (LLM) del flujo de trabajo. Solo compatible con modelos que son capaces de function calling.</td></tr><tr><td>Start Node</td><td><strong>Sí</strong></td><td>Recibe la <strong>entrada inicial del usuario</strong>, junto con el State personalizado (si está configurado) y el resto del array <code>state.messages</code> por defecto del Start Node.</td></tr><tr><td>Condition Node</td><td><strong>Sí</strong></td><td>Recibe entrada de un Condition Node precedente, permitiendo que el Agent Node <strong>tome acciones o guíe la conversación basado en el resultado de la evaluación del Condition Node</strong>.</td></tr><tr><td>Condition Agent Node</td><td><strong>Sí</strong></td><td>Recibe entrada de un Condition Agent Node precedente, permitiendo que el Agent Node <strong>tome acciones o guíe la conversación basado en el resultado de la evaluación del Condition Agent Node</strong>.</td></tr><tr><td>Agent Node</td><td><strong>Sí</strong></td><td>Recibe entrada de un Agent Node precedente, <strong>permitiendo acciones de agente encadenadas</strong> y manteniendo el contexto conversacional</td></tr><tr><td>LLM Node</td><td><strong>Sí</strong></td><td>Recibe la salida del LLM Node, permitiendo que el Agent Node <strong>procese la respuesta del LLM</strong>.</td></tr><tr><td>Tool Node</td><td><strong>Sí</strong></td><td>Recibe la salida de un Tool Node, permitiendo que el Agent Node <strong>procese e integre las salidas de la herramienta en su respuesta</strong>.</td></tr></tbody></table>

{% hint style="info" %}
El **Agent Node requiere al menos una conexión de los siguientes nodos**: Start Node, Agent Node, Condition Node, Condition Agent Node, LLM Node, o Tool Node.
{% endhint %}

### Outputs

El Agent Node puede conectarse a los siguientes nodos como salidas:

* **Agent Node:** Pasa el control a un Agent Node subsiguiente, permitiendo el encadenamiento de múltiples acciones de agente dentro de un flujo de trabajo. Esto permite flujos conversacionales más complejos y orquestación de tareas.
* **LLM Node:** Pasa la salida del agente a un LLM Node, permitiendo más procesamiento de lenguaje, generación de respuestas o toma de decisiones basada en las acciones e insights del agente.
* **Condition Agent Node:** Dirige el flujo a un Condition Agent Node. Este nodo evalúa la salida del Agent Node y sus condiciones predefinidas para determinar el siguiente paso apropiado en el flujo de trabajo.
* **Condition Node:** Similar al Condition Agent Node, el Condition Node usa condiciones predefinidas para evaluar la salida del Agent Node, dirigiendo el flujo por diferentes ramas basado en el resultado.
* **End Node:** Concluye el flujo de conversación.
* **Loop Node:** Redirige el flujo de vuelta a un nodo anterior, permitiendo procesos iterativos o cíclicos dentro del flujo de trabajo. Esto es útil para tareas que requieren múltiples pasos o involucran refinar resultados basados en interacciones previas. Por ejemplo, podrías volver a un Agent Node o LLM Node anterior para recopilar información adicional o refinar el flujo de conversación basado en la salida del Agent Node actual.

### Configuración del Nodo

<table><thead><tr><th width="201"></th><th width="101">Required</th><th>Description</th></tr></thead><tbody><tr><td>Agent Name</td><td><strong>Sí</strong></td><td>Agrega un nombre descriptivo al Agent Node para mejorar la legibilidad del flujo de trabajo y fácilmente <strong>apuntarlo de vuelta cuando uses bucles</strong> dentro del flujo de trabajo.</td></tr><tr><td>System Prompt</td><td>No</td><td>Define la <strong>'persona' del agente</strong> y <strong>guía su comportamiento</strong>. Por ejemplo, "<em>Eres un agente de servicio al cliente especializado en soporte técnico</em> [...]."</td></tr><tr><td>Require Approval</td><td>No</td><td><strong>Activa la característica Human-in-the-loop (HITL)</strong>. Si se establece en '<strong>True</strong>,' el Agent Node solicitará aprobación humana antes de ejecutar cualquier herramienta. Esto es particularmente valioso para operaciones sensibles o cuando se desea supervisión humana. Por defecto es '<strong>False</strong>,' permitiendo que el Agent Node ejecute herramientas autónomamente.</td></tr></tbody></table>

### Parámetros Adicionales

<table><thead><tr><th width="200"></th><th width="102">Required</th><th>Description</th></tr></thead><tbody><tr><td>Human Prompt</td><td>No</td><td>Este prompt se añade al array <code>state.messages</code> como un mensaje humano. Permite <strong>inyectar un mensaje similar al humano en el flujo de conversación</strong> después de que el Agent Node ha procesado su entrada y antes de que el siguiente nodo reciba la salida del Agent Node.</td></tr><tr><td>Approval Prompt</td><td>No</td><td><strong>Un prompt personalizable presentado al revisor humano cuando la característica HITL está activa</strong>. Este prompt proporciona contexto sobre la ejecución de la herramienta, incluyendo el nombre y propósito de la herramienta. La variable <code>{tools}</code> dentro del prompt será reemplazada dinámicamente con la lista actual de herramientas sugeridas por el agente, asegurando que el revisor humano tenga toda la información necesaria para tomar una decisión informada.</td></tr><tr><td>Approve Button Text</td><td>No</td><td>Personaliza <strong>el texto mostrado en el botón para aprobar la ejecución de la herramienta</strong> en la interfaz HITL. Esto permite adaptar el lenguaje al contexto específico y asegurar claridad para el revisor humano.</td></tr><tr><td>Reject Button Text</td><td>No</td><td>Personaliza el <strong>texto mostrado en el botón para rechazar la ejecución de la herramienta</strong> en la interfaz HITL. Como el Approve Button Text, esta personalización mejora la claridad y proporciona una acción clara para que el revisor humano tome si considera que la ejecución de la herramienta es innecesaria o potencialmente dañina.</td></tr><tr><td>Update State</td><td>No</td><td>Proporciona un <strong>mecanismo para modificar el objeto State personalizado compartido dentro del flujo de trabajo</strong>. Esto es útil para almacenar información recopilada por el agente o influir en el comportamiento de nodos subsiguientes.</td></tr><tr><td>Max Iteration</td><td>No</td><td>Limita el <strong>número de iteraciones</strong> que un Agent Node puede hacer dentro de una única ejecución del flujo de trabajo.</td></tr></tbody></table>

### Mejores Prácticas

{% tabs %}
{% tab title="Pro Tips" %}
**System prompt claro.**

Crea un System Prompt conciso y sin ambigüedades que refleje con precisión el rol y capacidades del agente. Esto guía la toma de decisiones del agente y asegura que actúe dentro de su alcance definido.

**Selección estratégica de herramientas**

Elige y configura las herramientas disponibles para el Agent Node, asegurando que se alineen con el propósito del agente y los objetivos generales del flujo de trabajo.

**HITL para tareas sensibles**

Utiliza la opción 'Require Approval' para tareas que involucran datos sensibles, requieren juicio humano o conllevan un riesgo de consecuencias no intencionadas.

**Aprovecha las actualizaciones de State personalizado**

Actualiza el objeto State personalizado estratégicamente para almacenar información recopilada o influir en el comportamiento de nodos posteriores.
{% endtab %}

{% tab title="Potential Pitfalls" %}
**Inacción del agente debido a sobrecarga de herramientas**

* **Problema:** Cuando un Agent Node tiene acceso a un gran número de herramientas dentro de una única ejecución del flujo de trabajo, podría tener dificultades para decidir qué herramienta es la más apropiada para usar, incluso cuando una herramienta es claramente necesaria. Esto puede llevar a que el agente no llame a ninguna herramienta en absoluto, resultando en respuestas incompletas o inexactas.
* **Ejemplo:** Imagina un agente de soporte al cliente diseñado para manejar una amplia gama de consultas. Lo has equipado con herramientas para seguimiento de pedidos, información de facturación, devoluciones de productos, soporte técnico y más. Un usuario pregunta, "¿Cuál es el estado de mi pedido?" pero el agente, abrumado por el número de herramientas potenciales, responde con una respuesta genérica como, "Puedo ayudarte con eso. ¿Cuál es tu número de pedido?" sin usar realmente la herramienta de seguimiento de pedidos.
* **Solución**
  1. **Refina los system prompts:** Proporciona instrucciones más claras y ejemplos dentro del System Prompt del Agent Node para guiarlo hacia la selección correcta de herramientas. Si es necesario, enfatiza las capacidades específicas de cada herramienta y las situaciones en las que deberían usarse.
  2. **Limita las opciones de herramientas por nodo:** Si es posible, divide los flujos de trabajo complejos en segmentos más pequeños y manejables, cada uno con un conjunto más enfocado de herramientas. Esto puede ayudar a reducir la carga cognitiva en el agente y mejorar su precisión en la selección de herramientas.

**Pasar por alto HITL para tareas sensibles**

* **Problema:** No utilizar la característica "Require Approval" (HITL) del Agent Node para tareas que involucran información sensible, decisiones críticas o acciones con potenciales consecuencias en el mundo real puede llevar a resultados no intencionados o daño a la confianza del usuario.
* **Ejemplo:** Tu agente de reservas de viajes tiene acceso a la información de pago de un usuario y puede reservar vuelos y hoteles automáticamente. Sin HITL, una mala interpretación de la intención del usuario o un error en la comprensión del agente podría resultar en una reserva incorrecta o uso no autorizado de los detalles de pago del usuario.
* **Solución**
  1. **Identifica acciones sensibles:** Analiza tu flujo de trabajo e identifica cualquier acción que involucre acceder o procesar datos sensibles (por ejemplo, información de pago, detalles personales).
  2. **Implementa "Require Approval":** Para estas acciones sensibles, habilita la opción "Require Approval" dentro del Agent Node. Esto asegura que un humano revise la acción propuesta por el agente y el contexto relevante antes de que se acceda a cualquier dato sensible o se tome cualquier acción irreversible.
  3. **Diseña prompts de aprobación claros:** Proporciona prompts claros y concisos para los revisores humanos, resumiendo la intención del agente, la acción propuesta y la información relevante necesaria para que el revisor tome una decisión informada.

**System prompt poco claro o incompleto**

* **Problema:** El System Prompt proporcionado al Agent Node carece de la especificidad y contexto necesarios para guiar al agente efectivamente en llevar a cabo sus tareas previstas. Un prompt vago o demasiado general puede llevar a respuestas irrelevantes, dificultad para entender la intención del usuario y una incapacidad para aprovechar herramientas o datos apropiadamente.
* **Ejemplo:** Estás construyendo un agente de reservas de viajes, y tu System Prompt simplemente dice "_Eres un asistente AI útil._" Esto carece de las instrucciones específicas y el contexto necesario para que el agente guíe efectivamente a los usuarios a través de búsquedas de vuelos, reservas de hoteles y planificación de itinerarios.
* **Solución:** Crea un System Prompt detallado y consciente del contexto:

{% code overflow="wrap" %}
```
Eres un agente de reservas de viajes. Tu objetivo principal es ayudar a los usuarios a planificar y reservar sus viajes. 
- Guíalos a través de la búsqueda de vuelos, encontrar alojamiento y explorar destinos.
- Sé amable, paciente y ofrece recomendaciones de viaje basadas en sus preferencias.
- Utiliza las herramientas disponibles para acceder a datos de vuelos, disponibilidad de hoteles e información de destinos.
```
{% endcode %}
{% endtab %}
{% endtabs %}

***

## 5. LLM Node

El LLM Node es un **componente especializado** que permite el procesamiento directo de lenguaje natural y la generación de respuestas usando un modelo de lenguaje específico, independientemente del Chat Model por defecto establecido en el Start Node.

<figure><img src="../../.gitbook/assets/seq-05.png" alt="" width="299"><figcaption></figcaption></figure>

### Entendiendo el LLM Node

El LLM Node proporciona una forma de **procesar la entrada del usuario o la salida de otros nodos usando un modelo de lenguaje específico**. Esto es particularmente útil cuando necesitas:

* Usar un modelo de lenguaje diferente para tareas específicas
* Procesar o reformular respuestas antes de pasarlas a otros nodos
* Generar contenido específico basado en el contexto de la conversación

### Inputs

<table><thead><tr><th width="195"></th><th width="107">Required</th><th>Description</th></tr></thead><tbody><tr><td>Chat Model</td><td><strong>Sí</strong></td><td>El modelo de lenguaje específico a usar para el procesamiento. A diferencia del Start Node, aquí <strong>puedes usar cualquier Chat Model</strong>, no solo aquellos con capacidades de function calling.</td></tr><tr><td>Start Node</td><td><strong>Sí</strong></td><td>Recibe la <strong>entrada inicial del usuario</strong> junto con el State personalizado (si está configurado) y el resto del array <code>state.messages</code> por defecto.</td></tr><tr><td>Condition Node</td><td><strong>Sí</strong></td><td>Recibe entrada de un Condition Node precedente, permitiendo que el LLM Node <strong>procese o genere contenido basado en el resultado de la evaluación del Condition Node</strong>.</td></tr><tr><td>Condition Agent Node</td><td><strong>Sí</strong></td><td>Recibe entrada de un Condition Agent Node precedente, permitiendo que el LLM Node <strong>procese o genere contenido basado en el resultado de la evaluación del Condition Agent Node</strong>.</td></tr><tr><td>Agent Node</td><td><strong>Sí</strong></td><td>Recibe entrada de un Agent Node precedente, permitiendo que el LLM Node <strong>procese la salida del agente</strong> o genere contenido basado en las acciones del agente.</td></tr><tr><td>LLM Node</td><td><strong>Sí</strong></td><td>Recibe la salida de otro LLM Node, permitiendo el <strong>procesamiento en cadena de lenguaje natural</strong>.</td></tr><tr><td>Tool Node</td><td><strong>Sí</strong></td><td>Recibe la salida de un Tool Node, permitiendo que el LLM Node <strong>procese o reformule los resultados de la herramienta</strong>.</td></tr></tbody></table>

{% hint style="info" %}
El **LLM Node requiere al menos una conexión de los siguientes nodos**: Start Node, Agent Node, Condition Node, Condition Agent Node, LLM Node, o Tool Node.
{% endhint %}

### Outputs

El LLM Node puede conectarse a los siguientes nodos como salidas:

* **Agent Node:** Pasa la salida procesada a un Agent Node para más acciones o toma de decisiones.
* **LLM Node:** Conecta con otro LLM Node para procesamiento adicional de lenguaje natural.
* **Condition Agent Node:** Dirige el flujo a un Condition Agent Node para evaluación y ramificación basada en la salida procesada.
* **Condition Node:** Similar al Condition Agent Node, permite ramificación basada en condiciones predefinidas.
* **End Node:** Concluye el flujo de conversación.
* **Loop Node:** Redirige el flujo de vuelta a un nodo anterior, permitiendo procesamiento iterativo o refinamiento de la salida.

### Configuración del Nodo

<table><thead><tr><th width="201"></th><th width="101">Required</th><th>Description</th></tr></thead><tbody><tr><td>System Prompt</td><td>No</td><td>Define el <strong>comportamiento y propósito</strong> del LLM Node. Por ejemplo, "<em>Eres un asistente especializado en reformular texto técnico para una audiencia general</em> [...]."</td></tr></tbody></table>

### Parámetros Adicionales

<table><thead><tr><th width="200"></th><th width="102">Required</th><th>Description</th></tr></thead><tbody><tr><td>Human Prompt</td><td>No</td><td>Este prompt se añade al array <code>state.messages</code> como un mensaje humano. Permite <strong>inyectar un mensaje similar al humano en el flujo de conversación</strong> después de que el LLM Node ha procesado su entrada y antes de que el siguiente nodo reciba la salida del LLM Node.</td></tr><tr><td>Update State</td><td>No</td><td>Proporciona un <strong>mecanismo para modificar el objeto State personalizado compartido dentro del flujo de trabajo</strong>. Esto es útil para almacenar información procesada o influir en el comportamiento de nodos subsiguientes.</td></tr></tbody></table>

### Mejores Prácticas

{% tabs %}
{% tab title="Pro Tips" %}
**Selección estratégica del modelo**

Elige un Chat Model que se alinee con el propósito específico del nodo. Por ejemplo, usa modelos optimizados para resumen cuando necesites condensar texto, o modelos enfocados en creatividad para generación de contenido.

**System prompts específicos**

Crea System Prompts que definan claramente el rol y propósito del LLM Node. Sé específico sobre el tipo de procesamiento o generación que debe realizar.

**Gestión eficiente del State**

Usa el campo Update State estratégicamente para almacenar información procesada que será relevante para nodos posteriores en el flujo de trabajo.
{% endtab %}

{% tab title="Potential Pitfalls" %}
**Selección incorrecta del modelo**

* **Problema:** Elegir un Chat Model que no está bien alineado con el propósito previsto del LLM Node puede resultar en salidas subóptimas o inapropiadas.
* **Ejemplo:** Usar un modelo optimizado para código cuando el nodo está destinado a reformular texto técnico para una audiencia general.
* **Solución:** Evalúa cuidadosamente las fortalezas y debilidades de diferentes modelos y selecciona uno que se alinee bien con el propósito específico del nodo.

**System prompt vago**

* **Problema:** Un System Prompt poco claro o demasiado general puede llevar a salidas inconsistentes o fuera del alcance deseado.
* **Ejemplo:** Un prompt que simplemente dice "Procesa este texto" sin especificar cómo o para qué propósito.
* **Solución:** Proporciona System Prompts detallados que especifiquen claramente:
  * El propósito exacto del procesamiento
  * El formato o estilo deseado de la salida
  * Cualquier restricción o requisito específico

**Procesamiento redundante**

* **Problema:** Encadenar múltiples LLM Nodes innecesariamente puede llevar a procesamiento redundante y potencial degradación del contenido.
* **Ejemplo:** Usar tres LLM Nodes separados para reformular, resumir y simplificar cuando todo podría hacerse en un solo nodo.
* **Solución:**
  * Consolida múltiples pasos de procesamiento en un solo LLM Node cuando sea posible
  * Usa LLM Nodes adicionales solo cuando se necesita un modelo diferente o un tipo distinto de procesamiento
{% endtab %}
{% endtabs %}

***

## 6. Tool Node

El Tool Node es un **componente especializado diseñado para ejecutar herramientas o funciones específicas** dentro del flujo de trabajo Sequential Agent. A diferencia del Agent Node, que puede seleccionar y ejecutar herramientas dinámicamente, el Tool Node está configurado para ejecutar una herramienta específica con parámetros predefinidos.

<figure><img src="../../.gitbook/assets/seq-06.png" alt="" width="299"><figcaption></figcaption></figure>

### Entendiendo el Tool Node

El Tool Node proporciona una forma de **ejecutar herramientas específicas directamente**, sin requerir un agente para tomar decisiones sobre qué herramienta usar. Esto es particularmente útil cuando:

* Sabes exactamente qué herramienta necesitas ejecutar en un punto específico del flujo de trabajo
* Quieres tener control preciso sobre cómo y cuándo se ejecutan las herramientas
* Necesitas ejecutar una herramienta con parámetros específicos que no deberían ser determinados dinámicamente por un agente

### Inputs

<table><thead><tr><th width="195"></th><th width="107">Required</th><th>Description</th></tr></thead><tbody><tr><td>External Tool</td><td><strong>Sí</strong></td><td>La <strong>herramienta específica</strong> que este nodo ejecutará.</td></tr><tr><td>Start Node</td><td><strong>Sí</strong></td><td>Recibe la <strong>entrada inicial del usuario</strong> junto con el State personalizado (si está configurado) y el resto del array <code>state.messages</code> por defecto.</td></tr><tr><td>Condition Node</td><td><strong>Sí</strong></td><td>Recibe entrada de un Condition Node precedente, permitiendo que el Tool Node <strong>ejecute basado en el resultado de la evaluación del Condition Node</strong>.</td></tr><tr><td>Condition Agent Node</td><td><strong>Sí</strong></td><td>Recibe entrada de un Condition Agent Node precedente, permitiendo que el Tool Node <strong>ejecute basado en el resultado de la evaluación del Condition Agent Node</strong>.</td></tr><tr><td>Agent Node</td><td><strong>Sí</strong></td><td>Recibe entrada de un Agent Node precedente, permitiendo que el Tool Node <strong>ejecute basado en las acciones o decisiones del agente</strong>.</td></tr><tr><td>LLM Node</td><td><strong>Sí</strong></td><td>Recibe la salida de un LLM Node, permitiendo que el Tool Node <strong>ejecute basado en el contenido procesado</strong>.</td></tr><tr><td>Tool Node</td><td><strong>Sí</strong></td><td>Recibe la salida de otro Tool Node, permitiendo <strong>ejecución encadenada de herramientas</strong>.</td></tr></tbody></table>

{% hint style="info" %}
El **Tool Node requiere al menos una conexión de los siguientes nodos**: Start Node, Agent Node, Condition Node, Condition Agent Node, LLM Node, o Tool Node.
{% endhint %}

### Outputs

El Tool Node puede conectarse a los siguientes nodos como salidas:

* **Agent Node:** Pasa los resultados de la herramienta a un Agent Node para procesamiento o toma de decisiones adicional.
* **LLM Node:** Conecta con un LLM Node para procesar o reformular los resultados de la herramienta.
* **Condition Agent Node:** Dirige el flujo a un Condition Agent Node para evaluación y ramificación basada en los resultados de la herramienta.
* **Condition Node:** Similar al Condition Agent Node, permite ramificación basada en condiciones predefinidas.
* **End Node:** Concluye el flujo de conversación.
* **Loop Node:** Redirige el flujo de vuelta a un nodo anterior, permitiendo ejecución iterativa de herramientas o refinamiento de resultados.
* **Tool Node:** Conecta con otro Tool Node para ejecución encadenada de herramientas.

### Configuración del Nodo

<table><thead><tr><th width="201"></th><th width="101">Required</th><th>Description</th></tr></thead><tbody><tr><td>Tool Parameters</td><td>No</td><td>Define los <strong>parámetros específicos requeridos por la herramienta</strong>. Estos variarán dependiendo de la herramienta seleccionada.</td></tr><tr><td>Require Approval</td><td>No</td><td><strong>Activa la característica Human-in-the-loop (HITL)</strong>. Si se establece en '<strong>True</strong>,' el Tool Node solicitará aprobación humana antes de ejecutar la herramienta.</td></tr></tbody></table>

### Parámetros Adicionales

<table><thead><tr><th width="200"></th><th width="102">Required</th><th>Description</th></tr></thead><tbody><tr><td>Approval Prompt</td><td>No</td><td><strong>Un prompt personalizable presentado al revisor humano cuando la característica HITL está activa</strong>. Este prompt proporciona contexto sobre la ejecución de la herramienta.</td></tr><tr><td>Approve Button Text</td><td>No</td><td>Personaliza <strong>el texto mostrado en el botón para aprobar la ejecución de la herramienta</strong> en la interfaz HITL.</td></tr><tr><td>Reject Button Text</td><td>No</td><td>Personaliza el <strong>texto mostrado en el botón para rechazar la ejecución de la herramienta</strong> en la interfaz HITL.</td></tr><tr><td>Update State</td><td>No</td><td>Proporciona un <strong>mecanismo para modificar el objeto State personalizado compartido dentro del flujo de trabajo</strong>. Esto es útil para almacenar los resultados de la herramienta o influir en el comportamiento de nodos subsiguientes.</td></tr></tbody></table>

### Mejores Prácticas

{% tabs %}
{% tab title="Pro Tips" %}
**Configuración clara de parámetros**

Define cuidadosamente los parámetros de la herramienta para asegurar que ejecute exactamente como se pretende. Documenta cualquier valor hardcodeado y su propósito.

**HITL estratégico**

Usa la característica "Require Approval" para herramientas que:
* Realizan acciones irreversibles
* Acceden a datos sensibles
* Tienen costos asociados
* Podrían tener consecuencias significativas si se ejecutan incorrectamente

**Gestión eficiente del State**

Usa el campo Update State para almacenar resultados importantes de la herramienta que podrían ser necesarios más adelante en el flujo de trabajo.
{% endtab %}

{% tab title="Potential Pitfalls" %}
**Parámetros mal configurados**

* **Problema:** Configuración incorrecta o incompleta de los parámetros de la herramienta puede resultar en ejecución fallida o resultados inesperados.
* **Ejemplo:** Una herramienta de API configurada con un endpoint incorrecto o faltante parámetros requeridos.
* **Solución:**
  * Revisa cuidadosamente la documentación de la herramienta
  * Prueba la configuración con varios casos de uso
  * Implementa manejo de errores apropiado

**Sobrecarga de HITL**

* **Problema:** Habilitar HITL innecesariamente para herramientas de bajo riesgo puede ralentizar el flujo de trabajo y crear fatiga del revisor.
* **Ejemplo:** Requerir aprobación humana para una herramienta que simplemente recupera información pública no sensible.
* **Solución:**
  * Reserva HITL para operaciones verdaderamente sensibles o de alto riesgo
  * Documenta claramente por qué se requiere HITL para herramientas específicas
  * Considera implementar diferentes niveles de aprobación basados en el riesgo

**Manejo inadecuado de errores**

* **Problema:** No manejar apropiadamente los errores de la herramienta puede llevar a flujos de trabajo rotos o comportamiento indefinido.
* **Ejemplo:** Una herramienta falla silenciosamente y el flujo de trabajo continúa con datos inválidos o faltantes.
* **Solución:**
  * Implementa manejo de errores robusto
  * Usa Condition Nodes para verificar resultados válidos
  * Proporciona mensajes de error claros y accionables
  * Considera implementar reintentos para errores temporales
{% endtab %}
{% endtabs %}

***

## 7. Condition Node

El Condition Node actúa como un **punto de toma de decisiones en los flujos de trabajo Sequential Agent**, evaluando un conjunto de condiciones predefinidas para determinar el siguiente camino del flujo.

<figure><img src="../../.gitbook/assets/seq-08.png" alt="" width="299"><figcaption></figcaption></figure>

### Entendiendo el Condition Node

El Condition Node es esencial para construir flujos de trabajo que se adapten a diferentes situaciones y entradas de usuario. Examina el State actual de la conversación, que incluye todos los mensajes intercambiados y cualquier variable de State personalizada previamente definida. Luego, basado en la evaluación de las condiciones especificadas en la configuración del nodo, el Condition Node dirige el flujo a una de sus salidas.

Por ejemplo, después de que un Agent o LLM Node proporciona una respuesta, un Condition Node podría verificar si la respuesta contiene una palabra clave específica o si se cumple cierta condición en el State personalizado. Si es así, el flujo podría dirigirse a un Agent Node para más acciones. Si no, podría llevar a un camino diferente, quizás terminando la conversación o solicitando al usuario preguntas adicionales.

Esto nos permite **crear ramificaciones en nuestro flujo de trabajo**, donde el camino tomado depende de los datos que fluyen a través del sistema.

#### Aquí hay una explicación paso a paso de cómo funciona

1. El Condition Node recibe entrada de cualquier nodo precedente: Start Node, Agent Node, LLM Node, o Tool Node.
2. Tiene acceso al historial completo de conversación y al State personalizado (si existe), dándole mucho contexto con el que trabajar.
3. Definimos una condición que el nodo evaluará. Esto podría ser verificar palabras clave, comparar valores en el state, o cualquier otra lógica que podamos implementar vía JavaScript.
4. Basado en si la condición se evalúa como **true** o **false**, el Condition Node envía el flujo por uno de sus caminos de salida predefinidos. Esto crea una "bifurcación en el camino" o rama para nuestro flujo de trabajo.

### Inputs

<table><thead><tr><th width="195"></th><th width="107">Required</th><th>Description</th></tr></thead><tbody><tr><td>Start Node</td><td><strong>Sí</strong></td><td>Recibe la <strong>entrada inicial del usuario</strong> junto con el State personalizado (si está configurado) y el resto del array <code>state.messages</code> por defecto.</td></tr><tr><td>Agent Node</td><td><strong>Sí</strong></td><td>Recibe entrada de un Agent Node precedente, permitiendo que el Condition Node <strong>evalúe la salida del agente</strong> para determinar el siguiente paso en el flujo de trabajo.</td></tr><tr><td>LLM Node</td><td><strong>Sí</strong></td><td>Recibe la salida de un LLM Node, permitiendo que el Condition Node <strong>evalúe el contenido procesado</strong> para determinar la ruta del flujo.</td></tr><tr><td>Tool Node</td><td><strong>Sí</strong></td><td>Recibe la salida de un Tool Node, permitiendo que el Condition Node <strong>evalúe los resultados de la herramienta</strong> para determinar el siguiente paso.</td></tr></tbody></table>

{% hint style="info" %}
El **Condition Node requiere al menos una conexión de los siguientes nodos**: Start Node, Agent Node, LLM Node, o Tool Node.
{% endhint %}

### Outputs

El Condition Node puede conectarse a los siguientes nodos como salidas:

* **Agent Node:** Dirige el flujo a un Agent Node basado en el resultado de la evaluación de la condición.
* **LLM Node:** Conecta con un LLM Node para procesamiento adicional cuando se cumple una condición específica.
* **Tool Node:** Activa la ejecución de una herramienta específica basado en el resultado de la evaluación.
* **Condition Node:** Permite evaluación adicional de condiciones, creando lógica de ramificación más compleja.
* **Condition Agent Node:** Similar al Condition Node, pero utiliza un agente para evaluación adicional.
* **End Node:** Concluye el flujo de conversación cuando se cumple una condición específica.
* **Loop Node:** Redirige el flujo de vuelta a un nodo anterior basado en el resultado de la evaluación.

### Configuración del Nodo

<table><thead><tr><th width="201"></th><th width="101">Required</th><th>Description</th></tr></thead><tbody><tr><td>Condition</td><td><strong>Sí</strong></td><td>La <strong>expresión JavaScript</strong> que será evaluada. Debe devolver un valor booleano (<code>true</code> o <code>false</code>).</td></tr></tbody></table>

### Parámetros Adicionales

<table><thead><tr><th width="200"></th><th width="102">Required</th><th>Description</th></tr></thead><tbody><tr><td>Update State</td><td>No</td><td>Proporciona un <strong>mecanismo para modificar el objeto State personalizado compartido dentro del flujo de trabajo</strong>. Esto es útil para almacenar el resultado de la evaluación o influir en el comportamiento de nodos subsiguientes.</td></tr></tbody></table>

### Mejores Prácticas

{% tabs %}
{% tab title="Pro Tips" %}
**Condiciones claras y específicas**

Escribe condiciones que sean fáciles de entender y mantener. Usa nombres de variables descriptivos y comenta el código cuando sea necesario.

**Manejo de casos límite**

Asegúrate de que tus condiciones manejen todos los casos posibles, incluyendo valores nulos o indefinidos.

**Actualización estratégica del State**

Usa el campo Update State para almacenar información sobre el resultado de la evaluación que podría ser útil para nodos posteriores.
{% endtab %}

{% tab title="Potential Pitfalls" %}
**Condiciones demasiado complejas**

* **Problema:** Escribir condiciones excesivamente complejas puede hacer que el flujo de trabajo sea difícil de entender y mantener.
* **Ejemplo:** Una única condición que verifica múltiples estados y variables con lógica anidada compleja.
* **Solución:**
  * Divide condiciones complejas en múltiples Condition Nodes más simples
  * Usa variables intermedias para claridad
  * Documenta la lógica de la condición

**Manejo inadecuado de errores**

* **Problema:** No manejar apropiadamente casos donde las variables o propiedades necesarias podrían no existir.
* **Ejemplo:** Intentar acceder a una propiedad anidada sin verificar si los objetos padre existen.
* **Solución:**
  * Usa el operador de encadenamiento opcional (?.) para acceso seguro a propiedades
  * Implementa verificaciones de existencia
  * Proporciona valores por defecto cuando sea apropiado

**Lógica de ramificación poco clara**

* **Problema:** Crear estructuras de ramificación confusas o redundantes que hacen que el flujo sea difícil de seguir.
* **Ejemplo:** Múltiples Condition Nodes evaluando condiciones similares o superpuestas.
* **Solución:**
  * Planifica la estructura de ramificación antes de implementar
  * Usa nombres descriptivos para los nodos
  * Considera consolidar condiciones relacionadas
  * Documenta el propósito de cada rama
{% endtab %}
{% endtabs %}

***

## 8. Condition Agent Node

El Condition Agent Node es una **variante especializada del Condition Node** que utiliza un agente AI para evaluar condiciones y tomar decisiones de ramificación dentro del flujo de trabajo Sequential Agent.

<figure><img src="../../.gitbook/assets/seq-07.png" alt="" width="299"><figcaption></figcaption></figure>

### Entendiendo el Condition Agent Node

A diferencia del Condition Node estándar, que utiliza condiciones predefinidas y lógica JavaScript, el Condition Agent Node **emplea un agente AI para evaluar dinámicamente el contexto de la conversación y tomar decisiones de ramificación más sofisticadas**. Esto es particularmente útil cuando:

* Las condiciones son demasiado complejas o sutiles para ser capturadas por lógica simple
* La decisión requiere comprensión del lenguaje natural o contexto conversacional
* Las condiciones necesitan adaptarse dinámicamente basadas en el comportamiento del usuario

### Inputs

<table><thead><tr><th width="195"></th><th width="107">Required</th><th>Description</th></tr></thead><tbody><tr><td>Chat Model</td><td><strong>Sí</strong></td><td>El modelo de lenguaje que el agente usará para evaluar condiciones. Debe ser <strong>capaz de function calling</strong>.</td></tr><tr><td>Start Node</td><td><strong>Sí</strong></td><td>Recibe la <strong>entrada inicial del usuario</strong> junto con el State personalizado (si está configurado) y el resto del array <code>state.messages</code> por defecto.</td></tr><tr><td>Agent Node</td><td><strong>Sí</strong></td><td>Recibe entrada de un Agent Node precedente, permitiendo que el Condition Agent Node <strong>evalúe la salida del agente</strong> para determinar el siguiente paso.</td></tr><tr><td>LLM Node</td><td><strong>Sí</strong></td><td>Recibe la salida de un LLM Node, permitiendo que el Condition Agent Node <strong>evalúe el contenido procesado</strong> para determinar la ruta del flujo.</td></tr><tr><td>Tool Node</td><td><strong>Sí</strong></td><td>Recibe la salida de un Tool Node, permitiendo que el Condition Agent Node <strong>evalúe los resultados de la herramienta</strong> para determinar el siguiente paso.</td></tr></tbody></table>

{% hint style="info" %}
El **Condition Agent Node requiere al menos una conexión de los siguientes nodos**: Start Node, Agent Node, LLM Node, o Tool Node.
{% endhint %}

### Outputs

El Condition Agent Node puede conectarse a los siguientes nodos como salidas:

* **Agent Node:** Dirige el flujo a un Agent Node basado en la evaluación del agente.
* **LLM Node:** Conecta con un LLM Node para procesamiento adicional cuando el agente determina que es necesario.
* **Tool Node:** Activa la ejecución de una herramienta específica basado en la decisión del agente.
* **Condition Node:** Permite evaluación adicional de condiciones usando lógica predefinida.
* **Condition Agent Node:** Permite evaluación adicional usando otro agente para casos más complejos.
* **End Node:** Concluye el flujo de conversación cuando el agente determina que es apropiado.
* **Loop Node:** Redirige el flujo de vuelta a un nodo anterior basado en la evaluación del agente.

### Configuración del Nodo

<table><thead><tr><th width="201"></th><th width="101">Required</th><th>Description</th></tr></thead><tbody><tr><td>System Prompt</td><td><strong>Sí</strong></td><td>Define el <strong>comportamiento y criterios de evaluación</strong> del agente. Por ejemplo, "<em>Eres un agente especializado en evaluar el sentimiento y la intención del usuario</em> [...]."</td></tr><tr><td>Output Paths</td><td><strong>Sí</strong></td><td>Define los <strong>posibles caminos de salida</strong> que el agente puede seleccionar basado en su evaluación.</td></tr></tbody></table>

### Parámetros Adicionales

<table><thead><tr><th width="200"></th><th width="102">Required</th><th>Description</th></tr></thead><tbody><tr><td>Human Prompt</td><td>No</td><td>Este prompt se añade al array <code>state.messages</code> como un mensaje humano. Permite <strong>inyectar un mensaje similar al humano en el flujo de conversación</strong> después de que el Condition Agent Node ha procesado su entrada.</td></tr><tr><td>Update State</td><td>No</td><td>Proporciona un <strong>mecanismo para modificar el objeto State personalizado compartido dentro del flujo de trabajo</strong>. Esto es útil para almacenar el resultado de la evaluación o influir en el comportamiento de nodos subsiguientes.</td></tr></tbody></table>

### Mejores Prácticas

{% tabs %}
{% tab title="Pro Tips" %}
**System prompts claros y específicos**

Define claramente los criterios que el agente debe usar para evaluar y tomar decisiones. Sé específico sobre qué aspectos de la conversación o State debe considerar.

**Caminos de salida bien definidos**

Proporciona nombres descriptivos y significativos para los caminos de salida que ayuden a entender el propósito de cada rama.

**Gestión eficiente del State**

Usa el campo Update State para almacenar información sobre la decisión del agente que podría ser útil para nodos posteriores.
{% endtab %}

{% tab title="Potential Pitfalls" %}
**Criterios de evaluación ambiguos**

* **Problema:** System prompts poco claros o demasiado generales pueden llevar a decisiones de ramificación inconsistentes o inapropiadas.
* **Ejemplo:** Un prompt que simplemente dice "Evalúa la entrada del usuario" sin especificar qué aspectos evaluar o qué criterios usar.
* **Solución:**
  * Define criterios de evaluación específicos y medibles
  * Proporciona ejemplos claros de cada tipo de situación
  * Especifica qué aspectos del contexto son más importantes

**Sobrecarga de opciones**

* **Problema:** Proporcionar demasiados caminos de salida posibles puede hacer que el agente dude o tome decisiones subóptimas.
* **Ejemplo:** Un Condition Agent Node con 10 posibles salidas, cada una con criterios sutilmente diferentes.
* **Solución:**
  * Limita el número de caminos de salida a un conjunto manejable
  * Asegúrate de que cada camino sea claramente distinto
  * Considera usar múltiples Condition Agent Nodes para decisiones más complejas

**Dependencia excesiva del contexto**

* **Problema:** El agente intenta considerar demasiados factores o depende demasiado de contexto sutil que podría no estar siempre disponible.
* **Ejemplo:** Un agente que requiere un historial de conversación extenso o información de State específica para tomar decisiones.
* **Solución:**
  * Prioriza los factores más importantes para la decisión
  * Asegúrate de que el agente pueda tomar decisiones razonables incluso con información limitada
  * Proporciona comportamiento por defecto claro cuando falte información crítica
{% endtab %}
{% endtabs %}

***

## 9. Loop Node

El Loop Node nos permite crear bucles dentro de nuestro flujo conversacional, **redirigiendo la conversación de vuelta a un punto específico**. Esto es útil para escenarios donde necesitamos repetir una cierta secuencia de acciones o preguntas basadas en la entrada del usuario o condiciones específicas.

<figure><img src="../../.gitbook/assets/sa-loop.png" alt="" width="335"><figcaption></figcaption></figure>

### Entendiendo el Loop Node

El Loop Node actúa como un conector, redirigiendo el flujo de vuelta a un punto específico en el grafo, permitiéndonos crear bucles dentro de nuestro flujo conversacional. **Pasa el State actual, que incluye la salida del nodo que precede al Loop Node a nuestro nodo objetivo.** Esta transferencia de datos permite que nuestro nodo objetivo procese información de la iteración anterior del bucle y ajuste su comportamiento en consecuencia.

Por ejemplo, digamos que estamos construyendo un chatbot que ayuda a los usuarios a reservar vuelos. Podríamos usar un bucle para refinar iterativamente los criterios de búsqueda basados en la retroalimentación del usuario.

#### Aquí hay un ejemplo de cómo se podría usar el Loop Node

1. Un Agent Node pregunta al usuario sobre sus preferencias de vuelo (destino, fecha, presupuesto).
2. Otro Agent Node busca vuelos que coincidan con estos criterios.
3. Un Condition Node evalúa si el usuario está satisfecho con las opciones presentadas.
4. Si el usuario no está satisfecho, el Loop Node redirige el flujo de vuelta al primer Agent Node para refinar los criterios de búsqueda.
5. Este proceso continúa hasta que el usuario encuentra un vuelo adecuado o decide terminar la búsqueda.

### Inputs

<table><thead><tr><th width="195"></th><th width="107">Required</th><th>Description</th></tr></thead><tbody><tr><td>Start Node</td><td><strong>Sí</strong></td><td>Recibe la <strong>entrada inicial del usuario</strong> junto con el State personalizado (si está configurado) y el resto del array <code>state.messages</code> por defecto.</td></tr><tr><td>Agent Node</td><td><strong>Sí</strong></td><td>Recibe entrada de un Agent Node precedente, permitiendo que el Loop Node <strong>redirija el flujo basado en las acciones o decisiones del agente</strong>.</td></tr><tr><td>LLM Node</td><td><strong>Sí</strong></td><td>Recibe la salida de un LLM Node, permitiendo que el Loop Node <strong>redirija el flujo basado en el contenido procesado</strong>.</td></tr><tr><td>Tool Node</td><td><strong>Sí</strong></td><td>Recibe la salida de un Tool Node, permitiendo que el Loop Node <strong>redirija el flujo basado en los resultados de la herramienta</strong>.</td></tr><tr><td>Condition Node</td><td><strong>Sí</strong></td><td>Recibe entrada de un Condition Node precedente, permitiendo que el Loop Node <strong>redirija el flujo basado en el resultado de la evaluación de la condición</strong>.</td></tr><tr><td>Condition Agent Node</td><td><strong>Sí</strong></td><td>Recibe entrada de un Condition Agent Node precedente, permitiendo que el Loop Node <strong>redirija el flujo basado en la evaluación del agente</strong>.</td></tr></tbody></table>

{% hint style="info" %}
El **Loop Node requiere al menos una conexión de los siguientes nodos**: Start Node, Agent Node, LLM Node, Tool Node, Condition Node, o Condition Agent Node.
{% endhint %}

### Outputs

El Loop Node puede conectarse a cualquier nodo que haya aparecido previamente en el flujo de trabajo, incluyendo:

* **Start Node:** Para reiniciar completamente el flujo de conversación.
* **Agent Node:** Para volver a un punto donde un agente puede tomar nuevas decisiones o acciones.
* **LLM Node:** Para reprocesar o refinar contenido previamente generado.
* **Tool Node:** Para reejecutar una herramienta con parámetros actualizados.
* **Condition Node:** Para reevaluar condiciones con nuevo contexto.
* **Condition Agent Node:** Para permitir que un agente reevalúe la situación con información actualizada.

### Configuración del Nodo

<table><thead><tr><th width="201"></th><th width="101">Required</th><th>Description</th></tr></thead><tbody><tr><td>Target Node</td><td><strong>Sí</strong></td><td>El <strong>nodo al cual redirigir</strong> el flujo. Debe ser un nodo que aparezca previamente en el flujo de trabajo.</td></tr><tr><td>Max Iterations</td><td>No</td><td>El <strong>número máximo de veces</strong> que el bucle puede ejecutarse antes de terminar automáticamente. Por defecto es ilimitado.</td></tr></tbody></table>

### Parámetros Adicionales

<table><thead><tr><th width="200"></th><th width="102">Required</th><th>Description</th></tr></thead><tbody><tr><td>Update State</td><td>No</td><td>Proporciona un <strong>mecanismo para modificar el objeto State personalizado compartido</strong> antes de redirigir el flujo. Esto es útil para rastrear el número de iteraciones o almacenar información entre iteraciones.</td></tr></tbody></table>

### Mejores Prácticas

{% tabs %}
{% tab title="Pro Tips" %}
**Define condiciones de salida claras**

Asegúrate de tener condiciones bien definidas para salir del bucle, ya sea a través de un contador de iteraciones, logro de objetivo, o entrada específica del usuario.

**Gestiona el State eficientemente**

Usa el campo Update State para mantener un registro del progreso y prevenir bucles infinitos.

**Considera la experiencia del usuario**

Proporciona retroalimentación clara en cada iteración y permite que los usuarios salgan del bucle en cualquier momento.
{% endtab %}

{% tab title="Potential Pitfalls" %}
**Bucles infinitos**

* **Problema:** Sin condiciones de salida apropiadas o contadores de iteración, el flujo puede quedar atrapado en un bucle infinito.
* **Ejemplo:** Un bucle que continúa pidiendo más detalles sin una condición clara para determinar cuándo se ha recopilado suficiente información.
* **Solución:**
  * Implementa un contador de iteraciones usando el State personalizado
  * Establece un límite máximo de iteraciones
  * Define condiciones claras para salir del bucle

**Pérdida de contexto**

* **Problema:** Información importante puede perderse o corromperse a través de múltiples iteraciones del bucle.
* **Ejemplo:** Variables de State siendo sobrescritas incorrectamente en cada iteración, llevando a pérdida de datos históricos importantes.
* **Solución:**
  * Gestiona cuidadosamente las actualizaciones del State
  * Considera usar arrays para mantener un historial de valores
  * Documenta claramente cómo se debe mantener y actualizar el State a través de las iteraciones

**Experiencia de usuario confusa**

* **Problema:** Los usuarios pueden perder el rastro de su progreso o sentirse atrapados en un bucle sin fin.
* **Ejemplo:** Un chatbot que continúa haciendo las mismas preguntas sin indicar cuánto progreso se ha hecho o cuántas preguntas quedan.
* **Solución:**
  * Proporciona indicadores claros de progreso
  * Ofrece una opción para salir del bucle en cualquier momento
  * Da retroalimentación clara sobre por qué se están repitiendo ciertas preguntas o acciones
{% endtab %}
{% endtabs %}

***

## 10. End Node

El End Node marca el **punto de terminación definitivo de la conversación** en un flujo de trabajo Sequential Agent. Significa que no se requieren más procesamientos, acciones o interacciones.

<figure><img src="../../.gitbook/assets/seq-end-node.png" alt="" width="375"><figcaption></figcaption></figure>

### Entendiendo el End Node

El End Node sirve como una señal dentro de la arquitectura Sequential Agent de Flowise, **indicando que la conversación ha alcanzado su conclusión prevista**. Al llegar al End Node, el sistema "entiende" que el objetivo conversacional se ha cumplido y no se requieren más acciones o interacciones dentro del flujo.

### Inputs

<table><thead><tr><th width="212"></th><th width="103">Required</th><th>Description</th></tr></thead><tbody><tr><td>Agent Node</td><td><strong>Sí</strong></td><td>Recibe la salida final de un Agent Node precedente, indicando el fin del procesamiento del agente.</td></tr><tr><td>LLM Node</td><td><strong>Sí</strong></td><td>Recibe la salida final de un LLM Node precedente, indicando el fin del procesamiento del LLM Node.</td></tr><tr><td>Tool Node</td><td><strong>Sí</strong></td><td>Recibe la salida final de un Tool Node precedente, indicando la finalización de la ejecución del Tool Node.</td></tr><tr><td>Condition Node</td><td><strong>Sí</strong></td><td>Recibe la salida final de un Condition Node precedente, indicando el fin de la ejecución del Condition Node.</td></tr><tr><td>Condition Agent Node</td><td><strong>Sí</strong></td><td>Recibe la salida final de un Condition Agent Node precedente, indicando la finalización del procesamiento del Condition Agent Node.</td></tr></tbody></table>

{% hint style="info" %}
El **End Node requiere al menos una conexión de los siguientes nodos**: Agent Node, LLM Node, o Tool Node.
{% endhint %}

### Outputs

El **End Node no tiene ninguna conexión de salida** ya que significa la terminación del flujo de información.

### Mejores Prácticas

{% tabs %}
{% tab title="Pro Tips" %}
**Proporciona una respuesta final**

Si es apropiado, conecta el End Node a un LLM o Agent Node dedicado para generar un mensaje final o resumen para el usuario, proporcionando cierre a la conversación.
{% endtab %}

{% tab title="Potential Pitfalls" %}
**Terminación prematura de la conversación**

* **Problema:** El End Node se coloca demasiado temprano en el flujo de trabajo, causando que la conversación termine antes de que todos los pasos necesarios se completen o la solicitud del usuario sea completamente atendida.
* **Ejemplo:** Un chatbot diseñado para recopilar retroalimentación del usuario termina la conversación después de que el usuario proporciona su primer comentario, sin darles la oportunidad de proporcionar retroalimentación adicional o hacer preguntas.
* **Solución:** Revisa la lógica de tu flujo de trabajo y asegúrate de que el End Node se coloque solo después de que todos los pasos esenciales se hayan completado o el usuario haya indicado explícitamente su intención de terminar la conversación.

**Falta de cierre para el usuario**

* **Problema:** La conversación termina abruptamente sin una señal clara para el usuario o un mensaje final que proporcione una sensación de cierre.
* **Ejemplo:** Un chatbot de soporte al cliente termina la conversación inmediatamente después de resolver un problema, sin confirmar la resolución con el usuario u ofrecer asistencia adicional.
* **Solución:** Conecta el End Node a un LLM o Agent Node dedicado para generar una respuesta final que resuma la conversación, confirme cualquier acción tomada y proporcione una sensación de cierre para el usuario.
{% endtab %}
{% endtabs %}

***

## Condition Node vs. Condition Agent Node

Los nodos Condition y Condition Agent son esenciales en la arquitectura Sequential Agent de Flowise para crear experiencias conversacionales dinámicas.

Estos nodos permiten flujos de trabajo adaptativos, respondiendo a la entrada del usuario, el contexto y decisiones complejas, pero difieren en su enfoque para la evaluación de condiciones y sofisticación.

<details>

<summary><strong>Condition Node</strong></summary>

**Propósito**

Crear ramificaciones basadas en condiciones lógicas simples y predefinidas.

**Evaluación de condiciones**

Utiliza una interfaz basada en tablas o un editor de código JavaScript para definir condiciones que se verifican contra el State personalizado y/o el historial completo de la conversación.

**Comportamiento de salida**

* Soporta múltiples caminos de salida, cada uno asociado con una condición específica.
* Las condiciones se evalúan en orden. La primera condición que coincida determina la salida.
* Si no se cumple ninguna condición, el flujo sigue una salida "End" por defecto.

**Más adecuado para**

* Decisiones de enrutamiento directas basadas en condiciones fácilmente definibles.
* Flujos de trabajo donde la lógica puede expresarse usando comparaciones simples, verificaciones de palabras clave o valores de variables de state personalizadas.

</details>

<details>

<summary><strong>Condition Agent Node</strong></summary>

**Propósito**

Crear enrutamiento dinámico basado en el análisis de la conversación por parte de un agente y su salida estructurada.

**Evaluación de condiciones**

* Si no hay un Chat Model conectado, usa el LLM del sistema por defecto (del Start Node) para procesar el historial de conversación y cualquier State personalizado.
* Puede generar salida estructurada, que luego se usa para la evaluación de condiciones.
* Utiliza una interfaz basada en tablas o un editor de código JavaScript para definir condiciones que se verifican contra la propia salida del agente, estructurada o no.

**Comportamiento de salida**

Igual que el Condition Node:

* Soporta múltiples caminos de salida, cada uno asociado con una condición específica.
* Las condiciones se evalúan en orden. La primera condición que coincida determina la salida.
* Si no se cumple ninguna condición, el flujo sigue la salida "End" por defecto.

**Más adecuado para**

* Decisiones de enrutamiento más complejas que requieren una comprensión del contexto de la conversación, la intención del usuario o factores matizados.
* Escenarios donde las condiciones lógicas simples son insuficientes para capturar la lógica de enrutamiento deseada.
* **Ejemplo:** Un chatbot necesita determinar si la pregunta de un usuario está relacionada con una categoría específica de producto. Se podría usar un Condition Agent Node para analizar la consulta del usuario y generar un objeto JSON con un campo "categoría". El Condition Agent Node puede entonces usar esta salida estructurada para dirigir al usuario al especialista de producto apropiado.

</details>

### Resumen

<table><thead><tr><th width="218"></th><th width="258">Condition Node</th><th>Condition Agent Node</th></tr></thead><tbody><tr><td><strong>Lógica de Decisión</strong></td><td>Basada en condiciones lógicas predefinidas.</td><td>Basada en el razonamiento del agente y salida estructurada.</td></tr><tr><td><strong>Participación del Agente</strong></td><td>No hay agente involucrado en la evaluación de condiciones.</td><td>Usa un agente para procesar el contexto y generar salida para las condiciones.</td></tr><tr><td><strong>Salida Estructurada</strong></td><td>No es posible.</td><td>Posible y recomendada para evaluación confiable de condiciones.</td></tr><tr><td><strong>Evaluación de Condiciones</strong></td><td>Solo define condiciones que se verifican contra el historial completo de conversación.</td><td>Puede definir condiciones que se verifican contra la propia salida del agente, estructurada o no.</td></tr><tr><td><strong>Complejidad</strong></td><td>Adecuado para lógica de ramificación simple.</td><td>Maneja enrutamiento más matizado y consciente del contexto.</td></tr><tr><td><strong>Casos de Uso Ideales</strong></td><td><ul><li>Enrutamiento basado en la edad del usuario o una palabra clave en la conversación.</li></ul></td><td><ul><li>Enrutamiento basado en el sentimiento del usuario, intención o factores contextuales complejos.</li></ul></td></tr></tbody></table>

### Eligiendo el nodo correcto

* **Condition Node:** Usa el Condition Node cuando tu lógica de enrutamiento involucra decisiones directas basadas en condiciones fácilmente definibles. Por ejemplo, es perfecto para verificar palabras clave específicas, comparar valores en el State, o evaluar otras expresiones lógicas simples.
* **Condition Agent Node:** Sin embargo, cuando tu enrutamiento demanda una comprensión más profunda de los matices de la conversación, el Condition Agent Node es la mejor opción. Este nodo actúa como tu asistente de enrutamiento inteligente, aprovechando un LLM para analizar la conversación, hacer juicios basados en el contexto y proporcionar salida estructurada que impulsa un enrutamiento más sofisticado y dinámico.

***

## Agent Node vs. LLM Node

Es importante entender que tanto el **LLM Node como el Agent Node pueden considerarse entidades agénticas dentro de nuestro sistema**, ya que ambos aprovechan las capacidades de un modelo de lenguaje grande (LLM) o Chat Model.

Sin embargo, aunque ambos nodos pueden procesar lenguaje e interactuar con herramientas, están diseñados para diferentes propósitos dentro de un flujo de trabajo.

<details>

<summary>Agent Node</summary>

**Enfoque**

El enfoque principal del Agent Node es simular las acciones y toma de decisiones de un agente humano dentro de un contexto conversacional.

Actúa como un coordinador de alto nivel dentro del flujo de trabajo, uniendo comprensión del lenguaje, ejecución de herramientas y toma de decisiones para crear una experiencia conversacional más humana.

**Fortalezas**

* Gestiona eficazmente la ejecución de múltiples herramientas e integra sus resultados.
* Ofrece soporte integrado para Human-in-the-Loop (HITL), permitiendo revisión y aprobación humana para operaciones sensibles.

**Más Adecuado Para**

* Flujos de trabajo donde el agente necesita guiar al usuario, recopilar información, tomar decisiones y gestionar el flujo general de la conversación.
* Escenarios que requieren integración con múltiples herramientas externas.
* Tareas que involucran datos sensibles o acciones donde la supervisión humana es beneficiosa, como aprobar transacciones financieras.

</details>

<details>

<summary>LLM Node</summary>

**Enfoque**

Similar al Agent Node, pero proporciona más flexibilidad al usar herramientas y Human-in-the-Loop (HITL), ambos a través del Tool Node.

**Fortalezas**

* Permite la definición de esquemas JSON para estructurar la salida del LLM, facilitando la extracción de información específica.
* Ofrece flexibilidad en la integración de herramientas, permitiendo secuencias más complejas de llamadas a LLM y herramientas, y proporcionando control granular sobre la característica HITL.

**Más Adecuado Para**

* Escenarios donde se necesita extraer datos estructurados de la respuesta del LLM.
* Flujos de trabajo que requieren una mezcla de ejecuciones de herramientas automatizadas y revisadas por humanos. Por ejemplo, un LLM Node podría llamar a una herramienta para recuperar información de productos (automatizado), y luego a una herramienta diferente para procesar un pago, que requeriría aprobación HITL.

</details>

### Resumen

<table><thead><tr><th width="206"></th><th width="253">Agent Node</th><th>LLM Node</th></tr></thead><tbody><tr><td><strong>Interacción con Herramientas</strong></td><td>Llama y gestiona múltiples herramientas directamente, HITL integrado.</td><td>Activa herramientas a través del Tool Node, control granular de HITL a nivel de herramienta.</td></tr><tr><td><strong>Human-in-the-Loop (HITL)</strong></td><td>HITL controlado a nivel de Agent Node (todas las herramientas conectadas afectadas).</td><td>HITL gestionado a nivel de Tool Node individual (más flexibilidad).</td></tr><tr><td><strong>Salida Estructurada</strong></td><td>Se basa en el formato de salida natural del LLM.</td><td>Se basa en el formato de salida natural del LLM, pero, si es necesario, proporciona definición de esquema JSON para estructurar la salida del LLM.</td></tr><tr><td><strong>Casos de Uso Ideales</strong></td><td><ul><li>Flujos de trabajo con orquestación compleja de herramientas.</li><li>HITL simplificado a nivel de Agente.</li></ul></td><td><ul><li>Extracción de datos estructurados de la salida del LLM</li><li>Flujos de trabajo con interacciones complejas de LLM y herramientas, que requieren niveles HITL mixtos.</li></ul></td></tr></tbody></table>

### Eligiendo el nodo correcto

* **Elige el Agent Node:** Usa el Agent Node cuando necesites crear un sistema conversacional que pueda gestionar la ejecución de múltiples herramientas, todas las cuales comparten la misma configuración HITL (habilitada o deshabilitada para todo el Agent Node). El Agent Node también es adecuado para manejar conversaciones complejas de múltiples pasos donde se desea un comportamiento consistente similar al de un agente.
* **Elige el LLM Node:** Por otro lado, usa el LLM Node cuando necesites extraer datos estructurados de la salida del LLM usando la característica de esquema JSON, una capacidad no disponible en el Agent Node. El LLM Node también sobresale en la orquestación de ejecución de herramientas con control granular sobre HITL a nivel de herramienta individual, permitiéndote mezclar ejecuciones de herramientas automatizadas y revisadas por humanos usando múltiples Tool Nodes conectados al LLM Node.

[^1]: En nuestro contexto actual, un nivel más bajo de abstracción se refiere a un sistema que expone un mayor grado de detalle de implementación al desarrollador.
