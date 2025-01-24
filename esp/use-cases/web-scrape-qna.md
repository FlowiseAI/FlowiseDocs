---
description: Aprende cómo hacer scraping, upsert y consultas a un sitio web
---

# Web Scrape QnA

***

Digamos que tienes un sitio web (podría ser una tienda, un sitio de comercio electrónico, un blog), y quieres hacer scraping de todos los enlaces relativos de ese sitio web y hacer que el LLM responda cualquier pregunta sobre tu sitio web. En este tutorial, vamos a ver cómo lograr eso.

Puedes encontrar el flujo de ejemplo llamado - **WebPage QnA** en las plantillas del marketplace.

## Configuración

Vamos a usar el nodo **Cheerio Web Scraper** para hacer scraping de enlaces desde una URL dada y el **HtmlToMarkdown Text Splitter** para dividir el contenido extraído en piezas más pequeñas.

<figure><img src="../.gitbook/assets/image (86).png" alt=""><figcaption></figcaption></figure>

Si no especificas nada, por defecto solo se hará scraping de la página de la URL proporcionada. Si quieres rastrear el resto de enlaces relativos, haz clic en **Additional Parameters** de Cheerio Web Scraper.

## 1. Rastrear Múltiples Páginas

1. Selecciona `Web Crawl` o `Scrape XML Sitemap` en **Get Relative Links Method**.
2. Ingresa `0` en **Get Relative Links Limit** para recuperar todos los enlaces disponibles desde la URL proporcionada.

<figure><img src="../.gitbook/assets/image (87).png" alt="" width="563"><figcaption></figcaption></figure>

### Gestionar Enlaces (Opcional)

1. Ingresa la URL deseada para rastrear.
2. Haz clic en **Fetch Links** para recuperar enlaces basados en las entradas de **Get Relative Links Method** y **Get Relative Links Limit** en **Additional Parameters**.
3. En la sección **Crawled Links**, elimina los enlaces no deseados haciendo clic en el **Icono de Papelera Roja**.
4. Por último, haz clic en **Save**.

<figure><img src="../.gitbook/assets/image (88).png" alt="" width="563"><figcaption></figcaption></figure>

## 2. Upsert

1. En la esquina superior derecha, notarás un botón verde:

<figure><img src="../.gitbook/assets/Untitled (2).png" alt=""><figcaption></figcaption></figure>

2. Se mostrará un diálogo que permite a los usuarios hacer upsert de datos a Pinecone:

<figure><img src="../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (2).png" alt=""><figcaption></figcaption></figure>

**Nota:** Bajo el capó, se ejecutarán las siguientes acciones:

* Scraping de todos los datos HTML usando Cheerio Web Scraper
* Conversión de todos los datos extraídos de HTML a Markdown, luego división
* Los datos divididos se recorrerán y se convertirán a embeddings vectoriales usando OpenAI Embeddings
* Los embeddings vectoriales se harán upsert a Pinecone

3. En la [consola de Pinecone](https://app.pinecone.io) podrás ver los nuevos vectores que se agregaron.

<figure><img src="../.gitbook/assets/web-scrape-pinecone.png" alt=""><figcaption></figcaption></figure>

## 3. Consulta

La consulta es relativamente directa. Después de haber verificado que los datos se han hecho upsert a la base de datos vectorial, puedes comenzar a hacer preguntas en el chat:

<figure><img src="../.gitbook/assets/image (4) (1) (1) (1) (1) (1) (1) (1) (1) (1) (2).png" alt=""><figcaption></figcaption></figure>

En los Additional Parameters de Conversational Retrieval QA Chain, puedes especificar 2 prompts:

* **Rephrase Prompt:** Usado para reformular la pregunta dado el historial de conversación pasado
* **Response Prompt:** Usando la pregunta reformulada, recupera el contexto de la base de datos vectorial y devuelve una respuesta final

<figure><img src="../.gitbook/assets/image (91).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
Se recomienda especificar un mensaje de prompt de respuesta detallado. Por ejemplo, puedes especificar el nombre de la IA, el idioma para responder, la respuesta cuando no se encuentra una respuesta (para prevenir la alucinación).
{% endhint %}

También puedes activar la opción Return Source Documents para devolver una lista de chunks de documentos de donde proviene la respuesta de la IA.

<figure><img src="../.gitbook/assets/Untitled (1) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

## Web Scraping Adicional

Además de Cheerio Web Scraper, hay otros nodos que también pueden realizar web scraping:

* **Puppeteer:** Puppeteer es una biblioteca de Node.js que proporciona una API de alto nivel para controlar Chrome o Chromium sin cabeza. Puedes usar Puppeteer para automatizar interacciones con páginas web, incluyendo la extracción de datos de páginas web dinámicas que requieren JavaScript para renderizar.
* **Playwright:** Playwright es una biblioteca de Node.js que proporciona una API de alto nivel para controlar múltiples motores de navegador, incluyendo Chromium, Firefox y WebKit. Puedes usar Playwright para automatizar interacciones con páginas web, incluyendo la extracción de datos de páginas web dinámicas que requieren JavaScript para renderizar.
* **Apify:** [Apify](https://apify.com/) es una plataforma en la nube para web scraping y extracción de datos, que proporciona un [ecosistema](https://apify.com/store) de más de mil aplicaciones listas para usar llamadas _Actors_ para varios casos de uso de web scraping, rastreo y extracción de datos.

<figure><img src="../.gitbook/assets/image (92).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
¡La misma lógica se puede aplicar a cualquier caso de uso de documentos, no solo limitado a web scraping!
{% endhint %}

Si tienes alguna sugerencia sobre cómo mejorar el rendimiento, ¡nos encantaría tu [contribución](../contributing/)!
