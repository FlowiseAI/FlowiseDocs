# API

### description: >- Aprende más sobre los detalles de algunas de las APIs más utilizadas: prediction, vector-upsert

## API

Consulta la [Referencia de API](../api-reference/) para ver la lista completa de APIs públicas

### Prediction

<div data-full-width="false"><figure><img src="../.gitbook/assets/image (16) (1) (1) (1).png" alt=""><figcaption></figcaption></figure></div>

{% openapi src="../.gitbook/assets/swagger (1) (1) (1).yml" path="/prediction/{id}" method="post" %}
[swagger (1) (1) (1).yml](<../.gitbook/assets/swagger (1) (1) (1).yml>)
{% endopenapi %}

#### Usando Python/TS Library

Flowise proporciona 2 librerías:

* [Python](https://pypi.org/project/flowise/): `pip install flowise`
* [Typescript](https://www.npmjs.com/package/flowise-sdk): `npm install flowise-sdk`

{% tabs %}
{% tab title="Python SDK" %}
```python
from flowise import Flowise, PredictionData

def test_non_streaming():
    client = Flowise()

    # Test de predicción sin streaming
    completion = client.create_prediction(
        PredictionData(
            chatflowId="<chatflow-id>",
            question="¿Cuál es la capital de Francia?",
            streaming=False
        )
    )

    # Procesar e imprimir la respuesta
    for response in completion:
        print("Respuesta sin streaming:", response)

def test_streaming():
    client = Flowise()

    # Test de predicción con streaming
    completion = client.create_prediction(
        PredictionData(
            chatflowId="<chatflow-id>",
            question="¡Cuéntame un chiste!",
            streaming=True
        )
    )

    # Procesar e imprimir cada fragmento del stream
    print("Respuesta con streaming:")
    for chunk in completion:
        print(chunk)


if __name__ == "__main__":
    # Ejecutar test sin streaming
    test_non_streaming()

    # Ejecutar test con streaming
    test_streaming()
```
{% endtab %}

{% tab title="Typescript SDK" %}
```javascript
import { FlowiseClient } from 'flowise-sdk'

async function test_streaming() {
  const client = new FlowiseClient({ baseUrl: 'http://localhost:3000' });

  try {
    // Para predicción con streaming
    const prediction = await client.createPrediction({
      chatflowId: 'fe1145fa-1b2b-45b7-b2ba-bcc5aaeb5ffd',
      question: '¿Cuál es el ingreso de Apple?',
      streaming: true,
    });

    for await (const chunk of prediction) {
        console.log(chunk);
    }
    
  } catch (error) {
    console.error('Error:', error);
  }
}

async function test_non_streaming() {
    const client = new FlowiseClient({ baseUrl: 'http://localhost:3000' });
  
    try {
      // Para predicción sin streaming
      const prediction = await client.createPrediction({
        chatflowId: 'fe1145fa-1b2b-45b7-b2ba-bcc5aaeb5ffd',
        question: '¿Cuál es el ingreso de Apple?',
      });
  
      console.log(prediction);
      
    } catch (error) {
      console.error('Error:', error);
    }
}

// Ejecutar test sin streaming
test_non_streaming()

// Ejecutar test con streaming
test_streaming()
```
{% endtab %}
{% endtabs %}

#### Override Config

Sobrescribe la configuración de entrada existente del chatflow con la propiedad **overrideConfig**.

Por razones de seguridad, la sobrescritura de configuración está deshabilitada por defecto. El usuario debe habilitarla yendo a **Chatflow Configuration** -> pestaña **Security**. Luego seleccionar la propiedad que puede ser sobrescrita.

<figure><img src="../.gitbook/assets/image (188).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (45).png" alt=""><figcaption></figcaption></figure>

{% tabs %}
{% tab title="Python API" %}
```python
import requests
API_URL = "http://localhost:3000/api/v1/prediction/<chatflowId>"

def query(payload):
    response = requests.post(API_URL, json=payload)
    return response.json()
    
output = query({
    "question": "Hola, ¿cómo estás?",
    "overrideConfig": {
        "sessionId": "123",
        "returnSourceDocuments": true
    }
})
```
{% endtab %}

{% tab title="Javascript API" %}
```javascript
async function query(data) {
    const response = await fetch(
        "http://localhost:3000/api/v1/prediction/<chatflowId>",
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
    "question": "Hola, ¿cómo estás?",
    "overrideConfig": {
        "sessionId": "123",
        "returnSourceDocuments": true
    }
}).then((response) => {
    console.log(response);
});
```
{% endtab %}
{% endtabs %}

#### History

Puedes anteponer mensajes del historial para dar contexto al LLM. Por ejemplo, si quieres que el LLM recuerde el nombre del usuario:

{% tabs %}
{% tab title="Python API" %}
```python
import requests
API_URL = "http://localhost:3000/api/v1/prediction/<chatflowId>"

def query(payload):
    response = requests.post(API_URL, json=payload)
    return response.json()
    
output = query({
    "question": "Hola, ¿cómo estás?",
    "history": [
        {
            "role": "apiMessage",
            "content": "¿Hola, en qué puedo ayudarte?"
        },
        {
            "role": "userMessage",
            "content": "Hola, mi nombre es Brian"
        },
        {
            "role": "apiMessage",
            "content": "Hola Brian, ¿en qué puedo ayudarte?"
        },
    ]
})
```
{% endtab %}

{% tab title="Javascript API" %}
```javascript
async function query(data) {
    const response = await fetch(
        "http://localhost:3000/api/v1/prediction/<chatflowId>",
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
    "question": "Hola, ¿cómo estás?",
    "history": [
        {
            "role": "apiMessage",
            "content": "¿Hola, en qué puedo ayudarte?"
        },
        {
            "role": "userMessage",
            "content": "Hola, mi nombre es Brian"
        },
        {
            "role": "apiMessage",
            "content": "Hola Brian, ¿en qué puedo ayudarte?"
        },
    ]
}).then((response) => {
    console.log(response);
});
```
{% endtab %}
{% endtabs %}

#### Persists Memory

Puedes pasar un `sessionId` para persistir el estado de la conversación, de modo que cada llamada API posterior tendrá contexto sobre la conversación anterior. De lo contrario, se generará una nueva sesión cada vez.

{% tabs %}
{% tab title="Python API" %}
```python
import requests
API_URL = "http://localhost:3000/api/v1/prediction/<chatflowId>"

def query(payload):
    response = requests.post(API_URL, json=payload)
    return response.json()
    
output = query({
    "question": "Hola, ¿cómo estás?",
    "overrideConfig": {
        "sessionId": "123"
    } 
})
```
{% endtab %}

{% tab title="Javascript API" %}
```javascript
async function query(data) {
    const response = await fetch(
        "http://localhost:3000/api/v1/prediction/<chatflowId>",
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
    "question": "Hola, ¿cómo estás?",
    "overrideConfig": {
        "sessionId": "123"
    }
}).then((response) => {
    console.log(response);
});
```
{% endtab %}
{% endtabs %}

#### Variables

Pasa variables en la API para ser utilizadas por los nodos en el flujo. Ver más: [Variables](api.md#variables)

{% tabs %}
{% tab title="Python API" %}
```python
import requests
API_URL = "http://localhost:3000/api/v1/prediction/<chatflowId>"

def query(payload):
    response = requests.post(API_URL, json=payload)
    return response.json()
    
output = query({
    "question": "Hola, ¿cómo estás?",
    "overrideConfig": {
        "vars": {
            "foo": "bar"
        }
    }
})
```
{% endtab %}

{% tab title="Javascript API" %}
```javascript
async function query(data) {
    const response = await fetch(
        "http://localhost:3000/api/v1/prediction/<chatflowId>",
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
    "question": "Hola, ¿cómo estás?",
    "overrideConfig": {
        "vars": {
            "foo": "bar"
        }
    }
}).then((response) => {
    console.log(response);
});
```
{% endtab %}
{% endtabs %}

#### Image Uploads

Cuando **Allow Image Upload** está habilitado, las imágenes pueden ser cargadas desde la interfaz de chat.

<div align="left" data-full-width="false"><figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="255"><figcaption></figcaption></figure> <figure><img src="../.gitbook/assets/Screenshot 2024-02-29 011714.png" alt="" width="290"><figcaption></figcaption></figure></div>

{% tabs %}
{% tab title="Python API" %}
```python
import requests
API_URL = "http://localhost:3000/api/v1/prediction/<chatflowId>"

def query(payload):
    response = requests.post(API_URL, json=payload)
    return response.json()
    
output = query({
    "question": "¿Puedes describir la imagen?",
    "uploads": [
        {
            "data": 'data:image/png;base64,iVBORw0KGgdM2uN0', # base64 string or url
            "type": 'file', # file | url
            "name": 'Flowise.png',
            "mime": 'image/png'
        }
    ]
})
```
{% endtab %}

{% tab title="Javascript API" %}
```javascript
async function query(data) {
    const response = await fetch(
        "http://localhost:3000/api/v1/prediction/<chatflowId>",
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
    "question": "¿Puedes describir la imagen?",
    "uploads": [
        {
            "data": 'data:image/png;base64,iVBORw0KGgdM2uN0', //base64 string or url
            "type": 'file', //file | url
            "name": 'Flowise.png',
            "mime": 'image/png'
        }
    ]
}).then((response) => {
    console.log(response);
});
```
{% endtab %}
{% endtabs %}

#### Speech to Text

Cuando **Speech to Text** está habilitado, los usuarios pueden hablar directamente en el micrófono y la voz se transcribirá en texto.

<div align="left"><figure><img src="../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure> <figure><img src="../.gitbook/assets/Screenshot 2024-02-29 012538.png" alt="" width="431"><figcaption></figcaption></figure></div>

{% tabs %}
{% tab title="Python API" %}
```python
import requests
API_URL = "http://localhost:3000/api/v1/prediction/<chatflowId>"

def query(payload):
    response = requests.post(API_URL, json=payload)
    return response.json()
    
output = query({
    "uploads": [
        {
            "data": 'data:audio/webm;codecs=opus;base64,GkXf', #base64 string
            "type": 'audio',
            "name": 'audio.wav',
            "mime": 'audio/webm'
        }
    ]
})
```
{% endtab %}

{% tab title="Javascript API" %}
```javascript
async function query(data) {
    const response = await fetch(
        "http://localhost:3000/api/v1/prediction/<chatflowId>",
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
    "uploads": [
        {
            "data": 'data:audio/webm;codecs=opus;base64,GkXf', //base64 string
            "type": 'audio',
            "name": 'audio.wav',
            "mime": 'audio/webm'
        }
    ]
}).then((response) => {
    console.log(response);
});
```
{% endtab %}
{% endtabs %}

### Vector Upsert API

{% openapi src="../.gitbook/assets/swagger (1) (1) (1).yml" path="/vector/upsert/{id}" method="post" %}
[swagger (1) (1) (1).yml](<../.gitbook/assets/swagger (1) (1) (1).yml>)
{% endopenapi %}

#### Document Loaders with File Upload

Algunos document loaders en Flowise permiten al usuario cargar archivos:

* [CSV File](../integrations/langchain/document-loaders/csv-file.md)
* [Docx File](../integrations/langchain/document-loaders/docx-file.md)
* [Json File](../integrations/langchain/document-loaders/json-file.md)
* [Json Lines File](../integrations/langchain/document-loaders/json-lines-file.md)
* [PDF File](../integrations/langchain/document-loaders/pdf-file.md)
* [Text File](../integrations/langchain/document-loaders/text-file.md)
* [Unstructured File](../integrations/langchain/document-loaders/unstructured-file-loader.md)

<div data-full-width="false"><figure><img src="../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure></div>

Si el flujo contiene [Document Loaders](../integrations/langchain/document-loaders/) con la funcionalidad de carga de archivo, la API se ve ligeramente diferente. En lugar de pasar el cuerpo como JSON, **form data** se utiliza. Esto te permite enviar archivos a la API. If the flow contains [Document Loaders](../integrations/langchain/document-loaders/) with Upload File functionality, the API looks slightly different. Instead of passing body as JSON, **form data** is being used. This allows you to send files to the API.

{% hint style="info" %}
Make sure the sent file type is compatible with the expected file type from document loader. For example, if a PDF File Loader is being used, you should only send **.pdf** files.

To avoid having separate loaders for different file types, we recommend to use [File Loader](../integrations/langchain/document-loaders/file-loader.md)
{% endhint %}

{% tabs %}
{% tab title="Python API" %}
```python
import requests

API_URL = "http://localhost:3000/api/v1/vector/upsert/<chatflowId>"

# use form data to upload files
form_data = {
    "files": ('state_of_the_union.txt', open('state_of_the_union.txt', 'rb'))
}

body_data = {
    "returnSourceDocuments": True
}

def query(form_data):
    response = requests.post(API_URL, files=form_data, data=body_data)
    print(response)
    return response.json()

output = query(form_data)
print(output)
```
{% endtab %}

{% tab title="Javascript API" %}
```javascript
// use FormData to upload files
let formData = new FormData();
formData.append("files", input.files[0]);
formData.append("returnSourceDocuments", true);

async function query(formData) {
    const response = await fetch(
        "http://localhost:3000/api/v1/vector/upsert/<chatflowId>",
        {
            method: "POST",
            body: formData
        }
    );
    const result = await response.json();
    return result;
}

query(formData).then((response) => {
    console.log(response);
});
```
{% endtab %}
{% endtabs %}

#### Document Loaders without Upload

For other [Document Loaders](../integrations/langchain/document-loaders/) nodes without Upload File functionality, the API body is in **JSON** format similar to [Prediction API](api.md#prediction-api).

{% tabs %}
{% tab title="Python API" %}
```python
import requests

API_URL = "http://localhost:3000/api/v1/vector/upsert/<chatflowId>"

def query(payload):
    response = requests.post(API_URL, json=payload)
    print(response)
    return response.json()

output = query({
    "overrideConfig": { # optional
        "returnSourceDocuments": true
    }
})
print(output)
```
{% endtab %}

{% tab title="Javascript API" %}
```javascript
async function query(data) {
    const response = await fetch(
        "http://localhost:3000/api/v1/vector/upsert/<chatflowId>",
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
    "overrideConfig": { // optional
        "returnSourceDocuments": true
    }
}).then((response) => {
    console.log(response);
});
```
{% endtab %}
{% endtabs %}

### Document Upsert/Refresh API

Refer to [Document Stores](document-stores.md#id-10.-api) section for more information about how to use the API.

{% openapi src="../.gitbook/assets/swagger (2).yml" path="/document-store/upsert/{id}" method="post" %}
[swagger (2).yml](<../.gitbook/assets/swagger (2).yml>)
{% endopenapi %}

{% openapi src="../.gitbook/assets/swagger (2).yml" path="/document-store/refresh/{id}" method="post" %}
[swagger (2).yml](<../.gitbook/assets/swagger (2).yml>)
{% endopenapi %}

### Video Tutorials

Those video tutorials cover the main use cases for implementing the Flowise API.

{% embed url="https://youtu.be/9R5zo3IVkqU?si=y1v_aCQLE_70WBnA" %}

{% embed url="https://youtu.be/LhN560DhlzU" %}

{% embed url="https://youtu.be/kOwmPe8aLAA" %}
