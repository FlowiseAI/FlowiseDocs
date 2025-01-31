---
description: Aprende cómo usar variables en Flowise
---

# Variables

***

Flowise permite a los usuarios crear variables que pueden ser utilizadas en los nodos. Las variables pueden ser Estáticas o de Tiempo de Ejecución.

### Estáticas

La variable estática se guardará con el valor especificado y se recuperará tal cual.

<figure><img src="../.gitbook/assets/image (13) (1) (1) (1).png" alt="" width="542"><figcaption></figcaption></figure>

### Tiempo de Ejecución

El valor de la variable se obtendrá del archivo **.env** usando `process.env`

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="537"><figcaption></figcaption></figure>

### Sobrescribir o establecer variables a través de la API

Para sobrescribir el valor de una variable, el usuario debe habilitarlo explícitamente desde la pestaña **Configuración del Flujo de Chat** -> **Seguridad**:

<figure><img src="../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

Si existe una variable creada, el valor de la variable proporcionado en la API sobrescribirá el valor existente.

```json
{
    "question": "hola",
    "overrideConfig": {
        "vars": {
            "var": "algun-valor-sobrescrito"
        }
    }
}
```

### Usando Variables

Las variables pueden ser utilizadas por los nodos en Flowise. Por ejemplo, se crea una variable llamada **`character`**:

<figure><img src="../.gitbook/assets/image (96).png" alt=""><figcaption></figcaption></figure>

Luego podemos usar esta variable como **`$vars.<nombre-variable>`** en la Función de los siguientes nodos:

* [Herramienta Personalizada](../integrations/langchain/tools/custom-tool.md)
* [Función Personalizada](../integrations/utilities/custom-js-function.md)
* [Cargador Personalizado](../integrations/langchain/document-loaders/custom-document-loader.md)
* [Si No](../integrations/utilities/if-else.md)

<figure><img src="../.gitbook/assets/image (105).png" alt="" width="283"><figcaption></figcaption></figure>

Además, el usuario también puede usar la variable en la entrada de texto de cualquier nodo con el siguiente formato:

**`{{$vars.<nombre-variable>}}`**

Por ejemplo, en el Mensaje del Sistema del Agente:

<figure><img src="../.gitbook/assets/image (1) (1) (1) (2) (1).png" alt="" width="508"><figcaption></figcaption></figure>

En la Plantilla de Prompt:

<figure><img src="../.gitbook/assets/image (157).png" alt=""><figcaption></figcaption></figure>

## Recursos

* [Pasar Variables a Función](../integrations/langchain/tools/custom-tool.md#pass-variables-to-function)
