---
description: Aprende cómo usar Multi-Agents en Flowise, escrito por @toi500
---

# Multi-Agents

Esta guía pretende proporcionar una introducción a la arquitectura del sistema de IA multi-agente dentro de Flowise, detallando sus componentes, restricciones operativas y flujo de trabajo.

## Concepto

Análogo a un equipo de expertos en dominios colaborando en un proyecto complejo, un sistema multi-agente utiliza el principio de especialización dentro de la inteligencia artificial.

Este sistema multi-agente utiliza un flujo de trabajo jerárquico y secuencial, maximizando la eficiencia y la especialización.

### 1. Arquitectura del Sistema

Podemos definir la arquitectura de IA multi-agente como un sistema de IA escalable capaz de manejar proyectos complejos dividiéndolos en sub-tareas manejables.

En Flowise, un sistema multi-agente comprende dos nodos principales o tipos de agentes y un usuario, interactuando en un gráfico jerárquico para procesar solicitudes y entregar un resultado específico:

1. **Usuario:** El usuario actúa como el **punto de inicio del sistema**, proporcionando la entrada o solicitud inicial. Si bien un sistema multi-agente puede ser diseñado para manejar una amplia gama de solicitudes, es importante que estas solicitudes del usuario se alineen con el propósito previsto del sistema. Cualquier solicitud que caiga fuera de este alcance puede llevar a resultados inexactos, bucles inesperados o incluso errores del sistema. Por lo tanto, las interacciones del usuario, aunque flexibles, siempre deben alinearse con las funcionalidades centrales del sistema para un rendimiento óptimo.
2. **Supervisor AI:** El Supervisor actúa como el **orquestador del sistema**, supervisando todo el flujo de trabajo. Analiza las solicitudes del usuario, las descompone en una secuencia de sub-tareas, asigna estas sub-tareas a los agentes trabajadores especializados, agrega los resultados y finalmente presenta la salida procesada de vuelta al usuario.
3. **Equipo Worker AI:** Este equipo consiste en agentes AI especializados, o Workers, cada uno instruido - a través de mensajes prompt - para manejar una tarea específica dentro del flujo de trabajo. Estos Workers operan independientemente, recibiendo instrucciones y datos del Supervisor, **ejecutando sus funciones especializadas**, usando herramientas según sea necesario, y devolviendo los resultados al Supervisor.

<figure><img src="../../.gitbook/assets/multi-agent-diagram.svg" alt=""><figcaption></figcaption></figure>

### 2. Restricciones Operativas

Para mantener el orden y la simplicidad, este sistema multi-agente opera bajo dos restricciones importantes:

* **Una tarea a la vez:** El Supervisor está intencionalmente diseñado para enfocarse en una sola tarea a la vez. Espera a que el Worker activo complete su tarea y devuelva los resultados antes de analizar el siguiente paso y delegar la tarea subsiguiente. Esto asegura que cada paso se complete exitosamente antes de continuar, previniendo la sobrecomplejidad.
* **Un Supervisor por flujo:** Si bien es teóricamente posible implementar un conjunto de sistemas multi-agente anidados para formar una estructura jerárquica más sofisticada para flujos de trabajo altamente complejos, lo que LangChain define como "[Hierarchical Agent Teams](https://github.com/langchain-ai/langgraph/blob/main/examples/multi_agent/hierarchical_agent_teams.ipynb)", con un supervisor de nivel superior y supervisores de nivel medio gestionando equipos de workers, los sistemas multi-agente de Flowise actualmente operan con un solo Supervisor.

{% hint style="info" %}
Estas dos restricciones son importantes cuando **planificas el flujo de trabajo de tu aplicación**. Si intentas diseñar un flujo de trabajo donde el Supervisor necesita delegar múltiples tareas simultáneamente, en paralelo, el sistema no podrá manejarlo y encontrarás un error.
{% endhint %}

## El Supervisor

El Supervisor, como el agente que gobierna el flujo de trabajo general y es responsable de delegar tareas al Worker apropiado, requiere un conjunto de componentes para funcionar correctamente:

* **Chat Model capaz de function calling** para manejar las complejidades de la descomposición de tareas, delegación y agregación de resultados.
* **Agent Memory (opcional)**: Si bien el Supervisor puede funcionar sin Agent Memory, este nodo puede mejorar significativamente los flujos de trabajo que requieren acceso a estados pasados del Supervisor. Esta **preservación del estado** podría permitir al Supervisor reanudar el trabajo desde un punto específico o aprovechar datos pasados para mejorar la toma de decisiones.

<figure><img src="../../.gitbook/assets/mas07.png" alt=""><figcaption></figcaption></figure>

### Supervisor Prompt

Por defecto, el Supervisor Prompt está redactado de una manera que instruye al Supervisor para analizar las solicitudes del usuario, descomponerlas en una secuencia de sub-tareas y asignar estas sub-tareas a los agentes trabajadores especializados.

Si bien el Supervisor Prompt es personalizable para adaptarse a necesidades específicas de la aplicación, siempre requiere los siguientes dos elementos clave:

* **La Variable {team_members}:** Esta variable es crucial para la comprensión del Supervisor de la fuerza laboral disponible ya que proporciona al Supervisor una lista de nombres de Workers. Esto permite al Supervisor delegar diligentemente tareas al Worker más apropiado basado en su experiencia.
* **La Palabra Clave "FINISH":** Esta palabra clave sirve como una señal dentro del Supervisor Prompt. Indica cuándo el Supervisor debe considerar la tarea completa y presentar la salida final al usuario. Sin una directiva clara de "FINISH", el Supervisor podría continuar delegando tareas innecesariamente o fallar en entregar un resultado coherente y finalizado al usuario. Señala que todas las sub-tareas necesarias han sido ejecutadas y la solicitud del usuario ha sido cumplida.

<figure><img src="../../.gitbook/assets/mas06.png" alt="" width="375"><figcaption></figcaption></figure>

{% hint style="info" %}
Es importante entender que el Supervisor juega un rol muy distinto al de los Workers. A diferencia de los Workers, que pueden ser adaptados con instrucciones altamente específicas, el **Supervisor opera más efectivamente con directivas generales, que le permiten planificar y delegar tareas como lo considere apropiado.** Si eres nuevo en sistemas multi-agente, recomendamos apegarse al prompt de Supervisor por defecto
{% endhint %}

### Entendiendo el Recursion Limit en el nodo Supervisor:

Este parámetro restringe la profundidad máxima de llamadas a funciones anidadas dentro de nuestra aplicación. En nuestro contexto actual, **limita cuántas veces el Supervisor puede activarse a sí mismo dentro de una sola ejecución de flujo de trabajo**. Esto es importante para prevenir la recursión sin límites y asegurar que los recursos se usen eficientemente.

<figure><img src="../../.gitbook/assets/mas04.png" alt="" width="375"><figcaption></figcaption></figure>

### Cómo funciona el Supervisor

Al recibir una consulta del usuario, el Supervisor inicia el flujo de trabajo analizando la solicitud y discerniendo el resultado previsto por el usuario.

Luego, aprovechando la variable `{team_members}` en el Supervisor Prompt, que solo proporciona una lista de nombres de Worker AI disponibles, el Supervisor infiere la especialidad de cada Worker y selecciona estratégicamente el Worker más adecuado para cada tarea dentro del flujo de trabajo.

{% hint style="info" %}
Ya que el Supervisor solo tiene los nombres de los Workers para inferir su funcionalidad dentro del flujo de trabajo, es muy importante que esos nombres se establezcan adecuadamente. **Los nombres claros, concisos y descriptivos que reflejan con precisión el rol o área de experiencia del Worker son cruciales para que el Supervisor tome decisiones informadas al delegar tareas.** Esto asegura que el Worker correcto sea seleccionado para el trabajo correcto, maximizando la precisión del sistema en cumplir la solicitud del usuario.
{% endhint %}

***

## **El Worker**

El Worker, como un agente especializado instruido para manejar una tarea específica dentro del sistema, requiere dos componentes esenciales para funcionar correctamente:

* **Un Supervisor:** Cada Worker debe estar conectado al Supervisor para que pueda ser llamado cuando una tarea necesita ser delegada. Esta conexión establece la relación jerárquica esencial dentro del sistema multi-agente, asegurando que el Supervisor pueda distribuir eficientemente el trabajo a los Workers especializados apropiados.
* **Un nodo Chat Model capaz de function calling**: Por defecto, los Workers heredan el nodo Chat Model del Supervisor a menos que se les asigne uno directamente. Esta capacidad de function-calling permite al Worker interactuar con herramientas diseñadas para su tarea especializada.

<figure><img src="../../.gitbook/assets/mas05.png" alt="" width="375"><figcaption></figcaption></figure>

{% hint style="info" %}
La capacidad de asignar **diferentes Chat Models a cada Worker** proporciona flexibilidad significativa y oportunidades de optimización para nuestra aplicación. Al seleccionar [Chat Models](../../integrations/langchain/chat-models/) adaptados a tareas específicas, podemos aprovechar soluciones más rentables para tareas más simples y reservar modelos especializados, potencialmente más caros, cuando sea verdaderamente necesario.
{% endhint %}

### Entendiendo el parámetro Max Iteration en Workers

[LangChain](https://python.langchain.com/v0.1/docs/modules/agents/how_to/max_iterations/) se refiere a `Max Iterations Cap` como un mecanismo de control importante para prevenir el descontrol dentro de un sistema agéntico. En nuestro contexto actual, nos sirve como una barrera contra interacciones excesivas, potencialmente infinitas, entre el Supervisor y el Worker.

A diferencia del `Recursion Limit` del nodo Supervisor, que restringe cuántas veces el Supervisor puede llamarse a sí mismo, el parámetro `Max Iteration` del nodo Worker limita cuántas veces un Supervisor puede iterar o consultar a un Worker específico.

Al limitar el Max Iteration, aseguramos que los costos permanezcan bajo control, incluso en casos de comportamiento inesperado del sistema.

***

## Ejemplo: Un caso de uso práctico

Ahora que hemos establecido una comprensión fundamental de cómo funcionan los sistemas Multi-Agent dentro de Flowise, exploremos una aplicación práctica.

Imagina un **sistema multi-agente de Lead Outreach** (disponible en el Marketplace) diseñado para automatizar el proceso de identificar, calificar y comprometerse con potenciales leads. Este sistema utilizaría un Supervisor para orquestar los siguientes dos Workers:

* **Lead Researcher:** Este Worker, usando la Google Search Tool, será responsable de recopilar potenciales leads basados en criterios definidos por el usuario.
* **Lead Sales Generator:** Este Worker utilizará la información recopilada por el Lead Researcher para crear borradores de correos electrónicos personalizados para el equipo de ventas.

<figure><img src="../../.gitbook/assets/mas08.png" alt=""><figcaption></figcaption></figure>

**Antecedentes:** Un usuario que trabaja en Solterra Renewables quiere recopilar información disponible sobre Evergreen Energy Group, una empresa de energía renovable respetable ubicada en el Reino Unido, y dirigirse a su CEO, Amelia Croft, como un potencial lead.

**Solicitud del Usuario:** El empleado de Solterra Renewables proporciona la siguiente consulta al sistema multi-agente: "_Necesito información sobre Evergreen Energy Group y Amelia Croft como un potencial nuevo cliente para nuestro negocio._"

1. **Supervisor:**
   * El Supervisor recibe la solicitud del usuario y delega la tarea de "Lead Research" al `Lead Researcher Worker`.
2. **Lead Researcher Worker:**
   * El Lead Researcher Worker, usando la Google Search Tool, recopila información sobre Evergreen Energy Group, enfocándose en:
     * Antecedentes de la empresa, industria, tamaño y ubicación.
     * Noticias y desarrollos recientes.
     * Ejecutivos clave, incluyendo la confirmación del rol de Amelia Croft como CEO.
   * El Lead Researcher envía la información recopilada de vuelta al `Supervisor`.
3. **Supervisor:**
   * El Supervisor recibe los datos de investigación del Lead Researcher Worker y confirma que Amelia Croft es un lead relevante.
   * El Supervisor delega la tarea de "Generate Sales Email" al `Lead Sales Generator Worker`, proporcionando:
     * La información de investigación sobre Evergreen Energy Group.
     * El correo electrónico de Amelia Croft.
     * Contexto sobre Solterra Renewables.
4. **Lead Sales Generator Worker:**
   * El Lead Sales Generator Worker elabora un borrador de correo electrónico personalizado adaptado a Amelia Croft, teniendo en cuenta:
     * Su rol como CEO y la relevancia de los servicios de Solterra Renewables para su empresa.
     * Información de la investigación sobre el enfoque o proyectos actuales de Evergreen Energy Group.
   * El Lead Sales Generator Worker envía el borrador de correo electrónico completado de vuelta al `Supervisor`.
5. **Supervisor:**
   * El Supervisor recibe el borrador de correo electrónico generado y emite la directiva "FINISH".
   * El Supervisor envía el borrador de correo electrónico de vuelta al usuario, el `empleado de Solterra Renewables`.
6. **Usuario Recibe la Salida:** El empleado de Solterra Renewables recibe un borrador de correo electrónico personalizado listo para ser revisado y enviado a Amelia Croft.

## Tutoriales en Video

Aquí encontrarás una lista de tutoriales en video del [canal de YouTube de Leon](https://www.youtube.com/@leonvanzyl) que muestran cómo construir aplicaciones multi-agente en Flowise usando no-code.

{% embed url="https://www.youtube.com/watch?ab_channel=LeonvanZyl&v=284Z8k7yJRE" %}

{% embed url="https://www.youtube.com/watch?ab_channel=LeonvanZyl&v=MaqcO15y-Vs" %}

{% embed url="https://www.youtube.com/watch?ab_channel=LeonvanZyl&v=eAH7LDGMVEs" %}
