---
description: Aprende a usar efectivamente el Chatflow Tool y el Custom Tool
---

# Llamando a Children Flows

***

Una de las características más potentes de Flowise es que puedes convertir flujos en tools. Por ejemplo, tener un flujo principal para orquestar qué/cuándo usar las tools necesarias. Y cada tool está diseñada para realizar una tarea específica.

Esto ofrece varios beneficios:

* Cada children flow como tool se ejecutará por sí mismo, con memoria separada para permitir una salida más limpia
* Agregar salidas detalladas de cada children flow a un agente final, a menudo resulta en una salida de mayor calidad

Puedes lograr esto usando las siguientes tools:

* Chatflow Tool
* Custom Tool

## Chatflow Tool

1. Ten un chatflow listo. En este caso, creamos un chatflow Chain of Thought que puede pasar por múltiples encadenamientos.

<figure><img src="../.gitbook/assets/image (169).png" alt=""><figcaption></figcaption></figure>

2. Crea otro chatflow con Tool Agent + Chatflow Tool. Selecciona el chatflow que quieres llamar desde la tool. En este caso, fue el chatflow Chain of Thought. Dale un nombre y una descripción apropiada para que el LLM sepa cuándo usar esta tool:

<figure><img src="../.gitbook/assets/image (35).png" alt="" width="245"><figcaption></figcaption></figure>

3. ¡Pruébalo!

<figure><img src="../.gitbook/assets/image (168).png" alt=""><figcaption></figcaption></figure>

4. En la respuesta, puedes ver la entrada y salida del Chatflow Tool:

<figure><img src="../.gitbook/assets/image (170).png" alt=""><figcaption></figcaption></figure>

## Custom Tool

Con el mismo ejemplo anterior, vamos a crear una custom tool que llamará a la [Prediction API](../using-flowise/api.md#prediction-api) del chatflow Chain of Thought.

1. Crea una nueva tool:

<table><thead><tr><th width="180">Tool Name</th><th>Tool Description</th></tr></thead><tbody><tr><td>ideas_flow</td><td>Usa esta tool cuando necesites alcanzar cierto objetivo</td></tr></tbody></table>

Input Schema:

<table><thead><tr><th>Property</th><th>Type</th><th>Description</th><th data-type="checkbox">Required</th></tr></thead><tbody><tr><td>input</td><td>string</td><td>pregunta de entrada</td><td>true</td></tr></tbody></table>

<figure><img src="../.gitbook/assets/image (95) (1).png" alt=""><figcaption></figcaption></figure>

Función Javascript de la tool:

```javascript
const fetch = require('node-fetch');
const url = 'http://localhost:3000/api/v1/prediction/<chatflow-id>'; // reemplazar con el id específico del chatflow

const body = {
	"question": $input
};

const options = {
	method: 'POST',
	headers: {
		'Content-Type': 'application/json'
	},
	body: JSON.stringify(body)
};

try {
	const response = await fetch(url, options);
	const resp = await response.json();
	return resp.text;
} catch (error) {
	console.error(error);
	return '';
}
```

2. Crea un Tool Agent + Custom Tool. Especifica la tool que hemos creado en el Paso 1 en el Custom Tool.

<figure><img src="../.gitbook/assets/image (97).png" alt=""><figcaption></figcaption></figure>

3. En la respuesta, puedes ver la entrada y salida del Custom Tool:

<figure><img src="../.gitbook/assets/image (99).png" alt=""><figcaption></figcaption></figure>

## Conclusión

En este ejemplo, hemos demostrado exitosamente 2 formas de convertir otros chatflows en tools, a través de Chatflow Tool y Custom Tool. Ambos están usando la misma lógica de código internamente.
