---
description: Aprende cómo consultar múltiples documentos correctamente
---

# QnA con Múltiples Documentos

***

Del último ejemplo de [QnA con Web Scraping](web-scrape-qna.md), solo estamos haciendo upsert y consultando 1 sitio web. ¿Qué pasa si tenemos múltiples sitios web o múltiples documentos? Veamos cómo podemos lograrlo.

En este ejemplo, vamos a realizar QnA en 2 PDFs, que son los FORM-10K de APPLE y TESLA.

<div align="left" data-full-width="false"><figure><img src="../.gitbook/assets/image (93).png" alt="" width="375"><figcaption></figcaption></figure> <figure><img src="../.gitbook/assets/image (94).png" alt="" width="375"><figcaption></figcaption></figure></div>

## Upsert

1. Encuentra el flujo de ejemplo llamado - **Conversational Retrieval QA Chain** en las plantillas del marketplace.
2. Vamos a usar [PDF File Loader](../integrations/langchain/document-loaders/pdf-file.md), y subir los archivos respectivos:

<figure><img src="../.gitbook/assets/multi-docs-upload.png" alt=""><figcaption></figcaption></figure>

3. Haz clic en **Additional Parameters** del PDF File Loader, y especifica el objeto metadata. Por ejemplo, el archivo PDF con el FORM-10K de Apple puede tener un objeto metadata `{source: apple}`, mientras que el archivo PDF con el FORM-10K de Tesla puede tener `{source: tesla}`. Esto se hace para segregar los documentos durante el tiempo de recuperación.

<div align="left"><figure><img src="../.gitbook/assets/multi-docs-apple.png" alt="" width="563"><figcaption></figcaption></figure> <figure><img src="../.gitbook/assets/multi-docs-tesla.png" alt="" width="563"><figcaption></figcaption></figure></div>

4. Después de completar las credenciales para Pinecone, haz clic en Upsert:

<figure><img src="../.gitbook/assets/multi-docs-upsert.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (98).png" alt=""><figcaption></figcaption></figure>

5. En la [consola de Pinecone](https://app.pinecone.io) podrás ver los nuevos vectores que se agregaron.

<figure><img src="../.gitbook/assets/multi-docs-console.png" alt=""><figcaption></figcaption></figure>

## Consulta

1. ¡Después de verificar que los datos se han insertado en Pinecone, ahora podemos empezar a hacer preguntas en el chat!

<figure><img src="../.gitbook/assets/image (100).png" alt=""><figcaption></figcaption></figure>

2. Sin embargo, el contexto recuperado usado para devolver la respuesta es una mezcla de documentos tanto de APPLE como de TESLA. Como puedes ver en los Source Documents:

<div align="left"><figure><img src="../.gitbook/assets/Untitled (7).png" alt="" width="563"><figcaption></figcaption></figure> <figure><img src="../.gitbook/assets/Untitled (8).png" alt="" width="563"><figcaption></figcaption></figure></div>

3. Podemos arreglar esto especificando un filtro de metadata desde el nodo Pinecone. Por ejemplo, si solo queremos recuperar contexto del FORM-10K de APPLE, podemos mirar el metadata que especificamos anteriormente en el paso [#upsert](multiple-documents-qna.md#upsert "mention"), y luego usar lo mismo en el Metadata Filter a continuación:

<figure><img src="../.gitbook/assets/image (102).png" alt=""><figcaption></figcaption></figure>

4. Hagamos la misma pregunta de nuevo, ahora deberíamos ver que todo el contexto recuperado es efectivamente del FORM-10K de APPLE:

<figure><img src="../.gitbook/assets/image (103).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
Cada proveedor de base de datos vectorial tiene diferente formato de sintaxis de filtrado, se recomienda leer la documentación respectiva de la base de datos vectorial
{% endhint %}

5. Sin embargo, el problema con esto es que el filtrado de metadata está de alguna manera _**"hard-coded"**_. Idealmente, deberíamos dejar que el LLM decida qué documento recuperar basado en la pregunta.

## Tool Agent

Podemos resolver el problema del filtro de metadata _**"hard-coded"**_ usando [Tool Agent](../integrations/langchain/agents/tool-agent.md).

Al proporcionar tools al agent, podemos dejar que el agent decida qué tool es adecuada para usar dependiendo de la pregunta.

1. Crea un [Retriever Tool](../integrations/langchain/tools/retriever-tool.md) con el siguiente nombre y descripción:

<table><thead><tr><th width="178">Name</th><th>Description</th></tr></thead><tbody><tr><td>search_apple</td><td>Usa esta función para responder preguntas del usuario sobre Apple Inc (APPL). Contiene un archivo SEC Form 10K que describe las finanzas de Apple Inc (APPL) para el período 2022.</td></tr></tbody></table>

2. Conéctalo al nodo Pinecone con el filtro de metadata `{source: apple}`

<figure><img src="../.gitbook/assets/image (104).png" alt="" width="563"><figcaption></figcaption></figure>

3. Repite lo mismo para Tesla:

<table><thead><tr><th width="175">Name</th><th width="322">Description</th><th>Pinecone Metadata Filter</th></tr></thead><tbody><tr><td>search_tsla</td><td>Usa esta función para responder preguntas del usuario sobre Tesla Inc (TSLA). Contiene un archivo SEC Form 10K que describe las finanzas de Tesla Inc (TSLA) para el período 2022.</td><td><code>{source: tesla}</code></td></tr></tbody></table>

{% hint style="info" %}
Es importante especificar una descripción clara y concisa. Esto permite que el LLM decida mejor cuándo usar qué tool
{% endhint %}

Tu flujo debería verse así:

<figure><img src="../.gitbook/assets/image (154).png" alt=""><figcaption></figcaption></figure>

4. Ahora, necesitamos crear una instrucción general para el Tool Agent. Haz clic en **Additional Parameters** del nodo y especifica el **System Message**. Por ejemplo:

```
Eres un analista financiero experto que siempre responde preguntas con la información más relevante usando las tools a tu disposición.
Estas tools tienen información sobre las empresas en las que el usuario ha expresado interés.
Aquí hay algunas pautas que debes seguir:
* Para preguntas financieras, debes usar las tools para encontrar la respuesta y luego escribir una respuesta.
* Incluso si parece que tus tools no podrán responder la pregunta, debes usarlas para encontrar la información e ideas más relevantes. No usarlas parecerá como si no estuvieras haciendo tu trabajo.
* Puedes asumir que las preguntas financieras de los usuarios están relacionadas con los documentos que han seleccionado.
* Para cualquier mensaje del usuario que no esté relacionado con el análisis financiero, rechaza respetuosamente responder y sugiere que el usuario haga una pregunta relevante.
* Si tus tools no pueden encontrar una respuesta, debes decir que no has encontrado una respuesta pero aún así transmitir cualquier información útil que las tools hayan encontrado.
* No hagas preguntas aclaratorias, simplemente devuelve la respuesta.

Las tools a tu disposición tienen acceso a los siguientes documentos SEC que el usuario ha seleccionado para discutir contigo:
- Apple Inc (APPL) FORM 10K 2022
- Tesla Inc (TSLA) FORM 10K 2022

La fecha actual es: 2024-01-28
```

5. ¡Guarda el Chatflow y empieza a hacer preguntas!

<figure><img src="../.gitbook/assets/image (110).png" alt=""><figcaption></figcaption></figure>

<div align="left"><figure><img src="../.gitbook/assets/Untitled (9).png" alt="" width="375"><figcaption></figcaption></figure> <figure><img src="../.gitbook/assets/Untitled (10).png" alt="" width="375"><figcaption></figcaption></figure></div>

6. Continúa con Tesla:

<figure><img src="../.gitbook/assets/image (111).png" alt=""><figcaption></figcaption></figure>

7. Ahora podemos hacer preguntas sobre cualquier documento que hayamos insertado previamente en la base de datos vectorial sin "hard-coding" del filtrado de metadata usando tools + agent.

## Metadata Retriever

Con el enfoque de Tool Agent, el usuario tiene que crear múltiples retriever tools para recuperar documentos de diferentes fuentes. Esto podría ser un problema si hay un gran número de fuentes de documentos con diferentes metadata. Usando el ejemplo anterior con solo Apple y Tesla, potencialmente podríamos expandirnos a otras compañías como Disney, Amazon, etc. Sería una tarea tediosa crear una retriever tool para cada compañía.

Aquí es donde entra en juego Metadata Retriever. La idea es que el LLM extraiga el metadata de la pregunta del usuario, y luego lo use como filtro al buscar en las bases de datos vectoriales.

Por ejemplo, si un usuario está haciendo preguntas relacionadas con Apple, un filtro de metadata `{source: apple}` se aplicará automáticamente en la búsqueda de la base de datos vectorial.

<div align="left"><figure><img src="../.gitbook/assets/image (235).png" alt="" width="297"><figcaption></figcaption></figure> <figure><img src="../.gitbook/assets/Screenshot 2024-11-29 155926.png" alt="" width="526"><figcaption></figcaption></figure></div>

En este escenario, podemos tener una sola retriever tool, y colocar el **Metadata Retriever** entre la base de datos vectorial y la retriever tool.

<figure><img src="../.gitbook/assets/image (236).png" alt=""><figcaption></figcaption></figure>

## XML Agent

Para algunos LLMs, las capacidades de function callings no están soportadas. En este caso, podemos usar XML Agent para indicar al LLM en un formato/sintaxis más estructurado, con el objetivo de usar las tools proporcionadas.

Tiene el prompt subyacente:

```xml
Eres un asistente útil. Ayuda al usuario a responder cualquier pregunta.

Tienes acceso a las siguientes tools:

{tools}

Para usar una tool, puedes usar las etiquetas <tool></tool> y <tool_input></tool_input>. Luego recibirás una respuesta en forma de <observation></observation>
Por ejemplo, si tienes una tool llamada 'search' que podría ejecutar una búsqueda en Google, para buscar el clima en SF responderías:

<tool>search</tool><tool_input>weather in SF</tool_input>
<observation>64 degrees</observation>

Cuando hayas terminado, responde con una respuesta final entre <final_answer></final_answer>. Por ejemplo:

<final_answer>El clima en SF es de 64 grados</final_answer>

¡Comienza!

Conversación Anterior:
{chat_history}

Pregunta: {input}
{agent_scratchpad}
```

<figure><img src="../.gitbook/assets/image (20) (1) (1).png" alt=""><figcaption></figcaption></figure>

## Conclusión

Hemos cubierto el uso de Conversational Retrieval QA Chain y sus limitaciones al consultar múltiples documentos. Y pudimos superar el problema usando OpenAI Function Agent/XML Agent + Tools. Puedes encontrar las plantillas a continuación:

{% file src="../.gitbook/assets/ToolAgent Chatflow.json" %}

{% file src="../.gitbook/assets/XMLAgent Chatflow.json" %}
