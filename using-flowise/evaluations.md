# Evaluaciones

{% hint style="info" %}
Las Evaluaciones solo están disponibles para los planes Cloud y Enterprise
{% endhint %}

Las evaluaciones te ayudan a monitorear y entender el rendimiento de tu aplicación de Flujo de Chat/Flujo de Agente. A alto nivel, una evaluación es un proceso que toma un conjunto de entradas y salidas correspondientes de tu Flujo de Chat/Flujo de Agente, y genera puntuaciones. Estas puntuaciones pueden derivarse comparando salidas con resultados de referencia, como a través de coincidencia de cadenas, comparación numérica, o incluso aprovechando un LLM como juez. Estas evaluaciones se realizan usando Conjuntos de Datos y Evaluadores.

## Conjuntos de Datos

Los conjuntos de datos son las entradas que se utilizarán para ejecutar tu Flujo de Chat/Flujo de Agente, junto con las salidas correspondientes para comparación. El usuario puede agregar la entrada y la salida anticipada manualmente, o cargar un archivo CSV con 2 columnas: Entrada y Salida.

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

| Entrada                           | Salida                        |
| --------------------------------- | ---------------------------- |
| ¿Cuál es la capital del Reino Unido? | La capital del Reino Unido es Londres |
| ¿Cuántos días hay en un año?      | Hay 365 días en un año       |

## Evaluadores

Los evaluadores son como pruebas unitarias. Durante una evaluación, las entradas de los Conjuntos de Datos se ejecutan en los flujos seleccionados y las salidas se evalúan usando los evaluadores seleccionados. Hay 3 tipos de evaluadores:

* **Basado en Texto**: comprobación basada en cadenas:
  * Contiene Alguno
  * Contiene Todos
  * No Contiene Ninguno
  * No Contiene Todos
  * Comienza Con
  * No Comienza Con

<figure><img src="../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

* **Basado en Números:** comprobación de tipos numéricos:
  * Total de Tokens
  * Tokens de Prompt
  * Tokens de Completado
  * Latencia de API
  * Latencia de LLM
  * Latencia de Flujo de Chat
  * Latencia de Flujo de Agente (próximamente)
  * Longitud de Caracteres de Salida

<figure><img src="../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

* **Basado en LLM**: usando otro LLM para calificar la salida
  * Alucinación
  * Corrección

<figure><img src="../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

## Evaluaciones

Ahora que tenemos los Conjuntos de Datos y Evaluadores preparados, podemos comenzar a ejecutar una evaluación.

1.) Selecciona el conjunto de datos y el flujo de chat a evaluar. Puedes seleccionar múltiples conjuntos de datos y flujos de chat. Usando el ejemplo a continuación, cada entrada del Conjunto de Datos 1 se ejecutará contra 2 flujos de chat. Como el Conjunto de Datos 1 tiene 2 entradas, se producirán y evaluarán un total de 4 salidas.

<figure><img src="../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

2.) Selecciona los evaluadores. Solo los evaluadores basados en cadenas y números están disponibles para ser seleccionados en esta etapa.

<figure><img src="../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

3.) (Opcional) Selecciona el evaluador Basado en LLM. Inicia la Evaluación:

<figure><img src="../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

4.) Espera a que se complete la evaluación:

<figure><img src="../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

5.) Después de que se complete la evaluación, haz clic en el icono de gráfico en el lado derecho para ver los detalles:

<figure><img src="../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

Los 3 gráficos anteriores muestran el resumen de la evaluación:

* Tasa de aprobación/fallo
* Promedio de tokens de prompt y completado utilizados
* Latencia promedio de la solicitud

La tabla debajo de los gráficos muestra los detalles de cada ejecución.

<figure><img src="../.gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (16).png" alt="" width="355"><figcaption></figcaption></figure>

### Volver a ejecutar evaluación

Cuando los flujos utilizados en la evaluación han sido actualizados/modificados, se mostrará un mensaje de advertencia:

<figure><img src="../.gitbook/assets/image (17).png" alt=""><figcaption></figcaption></figure>

Puedes volver a ejecutar la misma evaluación usando el botón Volver a Ejecutar Evaluación en la esquina superior derecha. Podrás ver las diferentes versiones:

<figure><img src="../.gitbook/assets/image (18).png" alt=""><figcaption></figcaption></figure>

También puedes ver y comparar los resultados de diferentes versiones:

<figure><img src="../.gitbook/assets/image (19).png" alt=""><figcaption></figcaption></figure>

## Tutorial en Video

{% embed url="https://youtu.be/kgUttHMkGFg?si=3rLplEp_0TI0p6UV&t=486" %}
