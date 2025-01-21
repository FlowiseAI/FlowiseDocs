# Custom Tool

Mira cómo usar custom tools

{% embed url="https://youtu.be/HSp9LkkTVY0" %}

## Problema

Las funciones generalmente toman datos de entrada estructurados. Digamos que quieres que el LLM pueda llamar a la API de Airtable Create Record [API](https://airtable.com/developers/web/api/create-records), los parámetros del body tienen que estar estructurados de una manera específica. Por ejemplo:

```json
"records": [
  {
    "fields": {
      "Address": "some address",
      "Name": "some name",
      "Visited": true
    }
  }
]
```

Idealmente, queremos que el LLM devuelva datos estructurados adecuadamente como esto:

```json
{
  "Address": "some address",
  "Name": "some name",
  "Visited": true
}
```

Así podemos extraer el valor y analizarlo en el body necesario para la API. Sin embargo, instruir al LLM para que genere el patrón exacto es difícil.

Con los nuevos modelos de [OpenAI Function Calling](https://openai.com/blog/function-calling-and-other-api-updates), ahora es posible. `gpt-4-0613` y `gpt-3.5-turbo-0613` están específicamente entrenados para devolver datos estructurados. El modelo elegirá de manera inteligente generar un objeto JSON que contenga los argumentos para llamar a esas funciones.

## Tutorial

**Objetivo**: Hacer que el agent obtenga automáticamente el movimiento del precio de las acciones, recupere noticias relacionadas con las acciones y agregue un nuevo registro a Airtable.

Empecemos[🚀](https://emojipedia.org/rocket/)

### Crear Tools

Necesitamos 3 tools para lograr el objetivo:

* Get Stock Price Movement
* Get Stock News
* Add Airtable Record

#### Get Stock Price Movement

Crea una nueva Tool con los siguientes detalles (puedes cambiarlos como desees):

* Name: get_stock_movers
* Description: Get the stocks that has biggest price/volume moves, e.g. actives, gainers, losers, etc.

La descripción es una parte importante ya que ChatGPT se basa en esto para decidir cuándo usar esta tool.

<figure><img src="../../../.gitbook/assets/image (6) (3).png" alt=""><figcaption></figcaption></figure>

* JavaScript Function: Vamos a usar la API [Morning Star](https://rapidapi.com/apidojo/api/morning-star) `/market/v2/get-movers` para obtener datos. Primero debes hacer clic en Subscribe to Test si aún no lo has hecho, luego copia el código y pégalo en JavaScript Function.
  * Agrega `const fetch = require('node-fetch');` al principio para importar la librería. Puedes importar cualquier [módulo](https://www.w3schools.com/nodejs/ref_modules.asp) integrado de NodeJS y [librerías externas](https://github.com/FlowiseAI/Flowise/blob/main/packages/components/src/utils.ts#L289).
  * Retorna el `result` al final.

<figure><img src="../../../.gitbook/assets/Untitled (4) (1).png" alt=""><figcaption></figcaption></figure>

El código final debería ser:

```javascript
const fetch = require('node-fetch');
const url = 'https://morning-star.p.rapidapi.com/market/v2/get-movers';
const options = {
	method: 'GET',
	headers: {
		'X-RapidAPI-Key': 'replace with your api key',
		'X-RapidAPI-Host': 'morning-star.p.rapidapi.com'
	}
};

try {
	const response = await fetch(url, options);
	const result = await response.text();
	console.log(result);
	return result;
} catch (error) {
	console.error(error);
	return '';
}
```

Ahora puedes guardarlo.

#### Get Stock News

Crea una nueva Tool con los siguientes detalles (puedes cambiarlos como desees):

* Name: get_stock_news
* Description: Get latest news for a stock
* Input Schema:
  * Property: performanceId
  * Type: string
  * Description: id of the stock, which is referred as performanceID in the API
  * Required: true

Input Schema le indica al LLM qué debe devolver como objeto JSON. En este caso, esperamos un objeto JSON como el siguiente:

<pre class="language-json"><code class="lang-json"><strong>{ "performanceId": "SOME TICKER" }
</strong></code></pre>

<figure><img src="../../../.gitbook/assets/image (4) (2).png" alt=""><figcaption></figcaption></figure>

* JavaScript Function: Vamos a usar la API [Morning Star](https://rapidapi.com/apidojo/api/morning-star) `/news/list` para obtener los datos. Primero debes hacer clic en Subscribe to Test si aún no lo has hecho, luego copia el código y pégalo en JavaScript Function.
  * Agrega `const fetch = require('node-fetch');` al principio para importar la librería. Puedes importar cualquier [módulo](https://www.w3schools.com/nodejs/ref_modules.asp) integrado de NodeJS y [librerías externas](https://github.com/FlowiseAI/Flowise/blob/main/packages/components/src/utils.ts#L289).
  * Retorna el `result` al final.
* Luego, reemplaza el parámetro performanceId codificado en la URL: `0P0000OQN8` por la variable de propiedad especificada en Input Schema: `$performanceId`
* Puedes usar cualquier propiedad especificada en Input Schema como variables en la JavaScript Function agregando el prefijo `$` al principio del nombre de la variable.

<figure><img src="../../../.gitbook/assets/Untitled (2) (1) (1).png" alt=""><figcaption></figcaption></figure>

Código final:

```javascript
const fetch = require('node-fetch');
const url = 'https://morning-star.p.rapidapi.com/news/list?performanceId=' + $performanceId;
const options = {
	method: 'GET',
	headers: {
		'X-RapidAPI-Key': 'replace with your api key',
		'X-RapidAPI-Host': 'morning-star.p.rapidapi.com'
	}
};

try {
	const response = await fetch(url, options);
	const result = await response.text();
	console.log(result);
	return result;
} catch (error) {
	console.error(error);
	return '';
}
```

Ahora puedes guardarlo.

#### Add Airtable Record

Crea una nueva Tool con los siguientes detalles (puedes cambiarlos como desees):

* Name: add_airtable
* Description: Add the stock, news summary & price move to Airtable
* Input Schema:
  * Property: stock
  * Type: string
  * Description: stock ticker
  * Required: true
  * Property: move
  * Type: string
  * Description: price move in %
  * Required: true
  * Property: news_summary
  * Type: string
  * Description: news summary of the stock
  * Required: true

ChatGPT devolverá un objeto JSON como este:

```json
{ "stock": "SOME TICKER", "move": "20%", "news_summary": "Some summary" }
```

<figure><img src="../../../.gitbook/assets/image (36).png" alt=""><figcaption></figcaption></figure>

* JavaScript Function: Vamos a usar [Airtable Create Record API](https://airtable.com/developers/web/api/create-records) para crear un nuevo registro en una tabla existente. Puedes encontrar el tableId y baseId [aquí](https://www.highviewapps.com/kb/where-can-i-find-the-airtable-base-id-and-table-id/). También necesitarás crear un token de acceso personal, encuentra cómo hacerlo [aquí](https://www.highviewapps.com/kb/how-do-i-create-an-airtable-personal-access-token/).

El código final debería verse como se muestra a continuación. Observa cómo pasamos `$stock`, `$move` y `$news_summary` como variables:

```javascript
const fetch = require('node-fetch');
const baseId = 'your-base-id';
const tableId = 'your-table-id';
const token = 'your-token';

const body = {
	"records": [
		{
			"fields": {
				"stock": $stock,
				"move": $move,
				"news_summary": $news_summary,
			}
		}
	]
};

const options = {
	method: 'POST',
	headers: {
		'Authorization': `Bearer ${token}`,
		'Content-Type': 'application/json'
	},
	body: JSON.stringify(body)
};

const url = `https://api.airtable.com/v0/${baseId}/${tableId}`

try {
	const response = await fetch(url, options);
	const text = await response.text();
	return text;
} catch (error) {
	console.error(error);
	return '';
}
```

Ahora puedes guardarlo.

Deberías ver 3 tools creadas:

<figure><img src="../../../.gitbook/assets/image (3) (3) (1).png" alt=""><figcaption></figcaption></figure>

### Crear Chatflow

Puedes usar la plantilla **OpenAI Function** **Agent** del marketplace, y reemplazar las tools con **Custom Tool**. Selecciona la tool que has creado.

Nota: OpenAI Function Agent solo soporta modelos 0613 actualmente.

<figure><img src="../../../.gitbook/assets/image (15) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Guarda el chatflow y comienza a probarlo. Para empezar, puedes intentar preguntar:

_<mark style="color:blue;">¿Qué acción tiene el mayor movimiento de precio hoy?</mark>_

_<mark style="color:orange;">La acción que tiene el mayor movimiento de precio hoy es Overstock.com (OSTK) con un movimiento de precio del 17.47%.</mark>_

Luego puedes hacer una pregunta de seguimiento para obtener las noticias de esa acción en particular:

_<mark style="color:blue;">¿Cuáles son las últimas noticias sobre esta acción que podrían haber causado el movimiento del precio?</mark>_

_<mark style="color:orange;">Aquí están las últimas noticias sobre Overstock.com (OSTK) que podrían haber causado el movimiento del precio:</mark>_

1. _<mark style="color:orange;">Título: "Las acciones de Overstock se disparan tras la exitosa oferta de 21.5 millones por los activos de Bed Bath & Beyond" Fuente: MarketWatch Fecha de publicación: 22 de junio de 2023 Resumen: Las acciones de Overstock.com experimentaron un aumento significativo después de ganar exitosamente una oferta por los activos de Bed Bath & Beyond valorados en 21.5 millones.</mark>_
2. _<mark style="color:orange;">Título: "¿Pensando en operar opciones o acciones de Meta Platforms, Overstock.com, Walmart, Home Depot o United Parcel Service?" Fuente: PR Newswire Fecha de publicación: 22 de junio de 2023 Resumen: Este artículo de noticias discute las potenciales opciones de trading y acciones, incluyendo Overstock.com, que los inversores podrían considerar.</mark>_

_<mark style="color:orange;">Ten en cuenta que estos artículos de noticias son solo para fines informativos y pueden no ser la única razón del movimiento del precio. Siempre se recomienda realizar una investigación y análisis exhaustivos antes de tomar cualquier decisión de inversión.</mark>_

Por último, puedes pedirle a ChatGPT que agregue un nuevo registro a Airtable:

_<mark style="color:blue;">¿Puedes agregar un registro a Airtable con el ticker de la acción, el movimiento del precio y el resumen de noticias?</mark>_

_<mark style="color:orange;">He agregado un registro a Airtable con los siguientes detalles:</mark>_

_<mark style="color:orange;">Ticker de la acción: OSTK Movimiento del precio: 17.47% Resumen de noticias: Las acciones de Overstock.com experimentaron un aumento significativo después de ganar exitosamente una oferta por los activos de Bed Bath & Beyond valorados en $21.5 millones.</mark>_

[🎉](https://emojipedia.org/party-popper/)[🎉](https://emojipedia.org/party-popper/)**¡Voilà!** ¡Así es como puedes crear tu propia custom tool y usarla con el OpenAI Function Agent!

## Adicional

### Pasar Session ID a la Function

Por defecto, la Function en custom tool tiene acceso a las siguientes configuraciones de flow:

```json5
$flow.sessionId 
$flow.chatId
$flow.chatflowId
$flow.input
```

A continuación se muestra un ejemplo de envío del sessionId a un webhook de Discord:

{% tabs %}
{% tab title="Javascript" %}
```javascript
const fetch = require('node-fetch');
const webhookUrl = "https://discord.com/api/webhooks/1124783587267";
const content = $content; // capturado desde input schema
const sessionId = $flow.sessionId;

const body = {
	"content": `${mycontent} and the sessionid is ${sessionId}`
};

const options = {
	method: 'POST',
	headers: {
		'Content-Type': 'application/json'
	},
	body: JSON.stringify(body)
};

const url = `${webhookUrl}?wait=true`

try {
	const response = await fetch(url, options);
	const text = await response.text();
	return text;
} catch (error) {
	console.error(error);
	return '';
}
```
{% endtab %}
{% endtabs %}

### Pasar variables a la Function

En algunos casos, te gustaría pasar variables a la función de custom tool.

Por ejemplo, estás creando un chatbot que usa una custom tool. La custom tool está ejecutando una llamada HTTP POST y se necesita una API key para una solicitud autenticada exitosa. Puedes pasarla como una variable.

Por defecto, la Function en custom tool tiene acceso a las variables:

```
$vars.<variable-name>
```

Ejemplo de cómo pasar variables en Flowise usando API y Embedded:

{% tabs %}
{% tab title="Javascript API" %}
```javascript
async function query(data) {
    const response = await fetch(
        "http://localhost:3000/api/v1/prediction/<chatflow-id>",
        {
            method: "POST",
            headers: {
                "Content-Type": "application/json"
            },
            body: JSON.stringify(data)
        }
    );
    const result = await response.json();
    return result;
}

query({
    "question": "Hey, how are you?",
    "overrideConfig": {
        "vars": {
            "apiKey": "abc"
        }
    }
}).then((response) => {
    console.log(response);
});
```
{% endtab %}

{% tab title="Embed" %}
```html
<script type="module">
    import Chatbot from 'https://cdn.jsdelivr.net/npm/flowise-embed/dist/web.js';
    Chatbot.init({
        chatflowid: 'chatflow-id',
        apiHost: 'http://localhost:3000',
        chatflowConfig: {
          vars: {
            apiKey: 'def'
          }
        }
    });
</script>
```
{% endtab %}
{% endtabs %}

Ejemplo de cómo recibir las variables en custom tool:

{% tabs %}
{% tab title="Javascript" %}
```javascript
const fetch = require('node-fetch');
const webhookUrl = "https://discord.com/api/webhooks/1124783587267";
const content = $content; // capturado desde input schema
const sessionId = $flow.sessionId;
const apiKey = $vars.apiKey;

const body = {
	"content": `${mycontent} and the sessionid is ${sessionId}`
};

const options = {
	method: 'POST',
	headers: {
		'Content-Type': 'application/json',
		'Authorization': `Bearer ${apiKey}`
	},
	body: JSON.stringify(body)
};

const url = `${webhookUrl}?wait=true`

try {
	const response = await fetch(url, options);
	const text = await response.text();
	return text;
} catch (error) {
	console.error(error);
	return '';
}
```
{% endtab %}
{% endtabs %}

### Sobrescribir Custom Tool

Los siguientes parámetros pueden ser sobrescritos

| Parámetro        | Descripción      |
| ---------------- | ---------------- |
| customToolName   | nombre de la tool |
| customToolDesc   | descripción de la tool |
| customToolSchema | schema de la tool |
| customToolFunc   | función de la tool |

Ejemplo de una llamada API para sobrescribir parámetros de custom tool:

{% tabs %}
{% tab title="Javascript API" %}
```javascript
async function query(data) {
    const response = await fetch(
        "http://localhost:3000/api/v1/prediction/<chatflow-id>",
        {
            method: "POST",
            headers: {
                "Content-Type": "application/json"
            },
            body: JSON.stringify(data)
        }
    );
    const result = await response.json();
    return result;
}

query({
    "question": "Hey, how are you?",
    "overrideConfig": {
        "customToolName": "example_tool",
        "customToolSchema": "z.object({title: z.string()})"
    }
}).then((response) => {
    console.log(response);
});
```
{% endtab %}
{% endtabs %}

### Importar Dependencias Externas

Puedes importar cualquier [módulo](https://www.w3schools.com/nodejs/ref_modules.asp) integrado de NodeJS y [librerías externas](https://github.com/FlowiseAI/Flowise/blob/main/packages/components/src/utils.ts#L289) soportadas en la Function.

1. Para importar cualquier librería no soportada, puedes agregar fácilmente el nuevo paquete npm al `package.json` en la carpeta `packages/components`.

```bash
cd Flowise && cd packages && cd components
pnpm add <your-library>
cd .. && cd ..
pnpm install
pnpm build
```

2. Luego, agrega las librerías importadas a la variable de entorno `TOOL_FUNCTION_EXTERNAL_DEP`. Consulta [#builtin-and-external-dependencies](../../../configuration/environment-variables.md#builtin-and-external-dependencies "mention") para más detalles.
3. Inicia la aplicación

```bash
pnpm start
```

4. Entonces podrás usar la librería recién agregada en la **JavaScript Function** así:

```javascript
const axios = require('axios')
```

Mira cómo agregar dependencias adicionales e importar librerías

{% embed url="https://youtu.be/0H1rrisc0ok" %}