---
description: Aprende a usar eficazmente la Herramienta Chatflow y la Herramienta Personalizada
---

# Llamando Flujos Secundarios

***

Una de las características más poderosas de Flowise es que puedes convertir flujos en herramientas. Por ejemplo, tener un flujo principal para orquestar qué herramientas usar y cuándo usarlas. Cada herramienta está diseñada para realizar una tarea específica.

Esto ofrece varios beneficios:

* Cada flujo secundario como herramienta se ejecutará de forma independiente, con memoria separada para permitir una salida más limpia.  
* Agregar resultados detallados de cada flujo secundario a un agente final a menudo resulta en una salida de mayor calidad.  

Puedes lograr esto utilizando las siguientes herramientas:

* Herramienta Chatflow  
* Herramienta Personalizada  

## Herramienta Chatflow

1. Ten un chatflow listo. En este caso, creamos un chatflow de Cadena de Pensamientos que puede pasar por múltiples encadenamientos.  

<figure><img src="../.gitbook/assets/image (169).png" alt=""><figcaption></figcaption></figure>

2. Crea otro chatflow con el Agente de Herramientas + Herramienta Chatflow. Selecciona el chatflow que deseas llamar desde la herramienta. En este caso, era el chatflow de Cadena de Pensamientos. Dale un nombre y una descripción adecuada para que LLM sepa cuándo usar esta herramienta:  

<figure><img src="../.gitbook/assets/image (35).png" alt="" width="245"><figcaption></figcaption></figure>

3. ¡Pruébalo!  

<figure><img src="../.gitbook/assets/image (168).png" alt=""><figcaption></figcaption></figure>

4. Desde la respuesta, puedes ver la entrada y salida de la Herramienta Chatflow:  

<figure><img src="../.gitbook/assets/image (170).png" alt=""><figcaption></figcaption></figure>

## Herramienta Personalizada

Con el mismo ejemplo anterior, vamos a crear una herramienta personalizada que llamará a la [API de Predicción](../using-flowise/api.md#prediction-api) del chatflow de Cadena de Pensamientos.

1. Crea una nueva herramienta:

<table><thead><tr><th width="180">Nombre de la Herramienta</th><th>Descripción de la Herramienta</th></tr></thead><tbody><tr><td>ideas_flow</td><td>Usa esta herramienta cuando necesites lograr un objetivo específico</td></tr></tbody></table>

Esquema de Entrada:

<table><thead><tr><th>Propiedad</th><th>Tipo</th><th>Descripción</th><th data-type="checkbox">Obligatorio</th></tr></thead><tbody><tr><td>input</td><td>string</td><td>pregunta de entrada</td><td>true</td></tr></tbody></table>

<figure><img src="../.gitbook/assets/image (95) (1).png" alt=""><figcaption></figcaption></figure>

Función JavaScript de la herramienta:

```javascript
const fetch = require('node-fetch');
const url = 'http://localhost:3000/api/v1/prediction/<chatflow-id>'; // reemplaza con el ID específico del chatflow

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

2. Crea un Agente de Herramientas + Herramienta Personalizada. Especifica la herramienta creada en el Paso 1 en la Herramienta Personalizada.  

<figure><img src="../.gitbook/assets/image (97).png" alt=""><figcaption></figcaption></figure>

3. Desde la respuesta, puedes ver la entrada y salida de la Herramienta Personalizada:  

<figure><img src="../.gitbook/assets/image (99).png" alt=""><figcaption></figcaption></figure>

## Conclusión

En este ejemplo, hemos demostrado exitosamente dos formas de convertir otros chatflows en herramientas, a través de la Herramienta Chatflow y la Herramienta Personalizada. Ambas utilizan la misma lógica de código internamente.
