# ChatLocalAI

## Configuración de LocalAI

[**LocalAI**](https://github.com/go-skynet/LocalAI) es una API REST de reemplazo compatible con las especificaciones de la API de OpenAI para inferencia local. Te permite ejecutar LLMs (y no solo eso) localmente o en las instalaciones con hardware de grado consumidor, soportando múltiples familias de modelos que son compatibles con el formato ggml.

Para usar ChatLocalAI dentro de Flowise, sigue los pasos a continuación:

1. ```bash
   git clone https://github.com/go-skynet/LocalAI
   ```
2. ```bash
   cd LocalAI
   ```
3. ```bash
   # copia tus modelos a models/
   cp your-model.bin models/
   ```

Por ejemplo:

Descarga uno de los modelos desde [gpt4all.io](https://gpt4all.io/index.html)

```bash
# Descarga gpt4all-j a models/
wget https://gpt4all.io/models/ggml-gpt4all-j.bin -O models/ggml-gpt4all-j
```

En la carpeta `/models`, deberías poder ver el modelo descargado:

<figure><img src="../../../.gitbook/assets/image (22) (1).png" alt=""><figcaption></figcaption></figure>

Consulta [aquí](https://localai.io/model-compatibility/index.html) la lista de modelos soportados.

4. ```bash
   docker compose up -d --pull always
   ```
5. Ahora la API es accesible en localhost:8080

```bash
# Prueba la API
curl http://localhost:8080/v1/models
# {"object":"list","data":[{"id":"ggml-gpt4all-j.bin","object":"model"}]}
```

## Configuración en Flowise

Arrastra y suelta un nuevo componente ChatLocalAI al lienzo:

<figure><img src="../../../.gitbook/assets/image (39).png" alt=""><figcaption></figcaption></figure>

Completa los campos:

* **Base Path**: La URL base de LocalAI como [http://localhost:8080/v1](http://localhost:8080/v1)
* **Model Name**: El modelo que quieres usar. Ten en cuenta que debe estar dentro de la carpeta `/models` del directorio de LocalAI. Por ejemplo: `ggml-gpt4all-j.bin`

{% hint style="info" %}
Si estás ejecutando tanto Flowise como LocalAI en Docker, es posible que necesites cambiar la ruta base a [http://host.docker.internal:8080/v1](http://host.docker.internal:8080/v1). Para sistemas basados en Linux, se debe usar el gateway predeterminado de docker ya que host.docker.internal no está disponible: [http://172.17.0.1:8080/v1](http://172.17.0.1:8080/v1)
{% endhint %}

¡Eso es todo! Para más información, consulta la [documentación](https://localai.io/basics/getting_started/index.html) de LocalAI.

Mira cómo puedes usar LocalAI en Flowise:

{% embed url="https://youtu.be/0B0oIs8NS9k" %}
