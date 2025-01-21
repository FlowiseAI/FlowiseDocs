# LocalAI Embeddings

## Configuración de LocalAI

[**LocalAI** ](https://github.com/go-skynet/LocalAI)es un reemplazo directo de REST API que es compatible con las especificaciones de OpenAI API para inferencia local. Te permite ejecutar LLMs (y no solo eso) localmente o en las instalaciones con hardware de grado consumidor, soportando múltiples familias de modelos que son compatibles con el formato ggml.

Para usar LocalAI Embeddings dentro de Flowise, sigue los pasos a continuación:

1. ```bash
   git clone https://github.com/go-skynet/LocalAI
   ```
2. <pre class="language-bash"><code class="lang-bash"><strong>cd LocalAI
   </strong></code></pre>
3. LocalAI proporciona un [endpoint de API](https://localai.io/api-endpoints/index.html#applying-a-model---modelsapply) para descargar/instalar el modelo. En este ejemplo, vamos a usar el modelo BERT Embeddings:

<figure><img src="../../../.gitbook/assets/image (27) (1).png" alt=""><figcaption></figcaption></figure>

4. En la carpeta `/models`, deberías poder ver el modelo descargado:

<figure><img src="../../../.gitbook/assets/image (23) (1).png" alt=""><figcaption></figcaption></figure>

5. Ahora puedes probar los embeddings:

```bash
curl http://localhost:8080/v1/embeddings -H "Content-Type: application/json" -d '{
    "input": "Test",
    "model": "text-embedding-ada-002"
  }'
```

6. La respuesta debería verse así:

<figure><img src="../../../.gitbook/assets/image (29).png" alt="" width="375"><figcaption></figcaption></figure>

## Configuración de Flowise

Arrastra y suelta un nuevo componente LocalAIEmbeddings al lienzo:

<figure><img src="../../../.gitbook/assets/image (21) (1) (2).png" alt=""><figcaption></figcaption></figure>

Completa los campos:

* **Base Path**: La URL base de LocalAI, por ejemplo [http://localhost:8080/v1](http://localhost:8080/v1)
* **Model Name**: El modelo que quieres usar. Ten en cuenta que debe estar dentro de la carpeta `/models` del directorio de LocalAI. Por ejemplo: `text-embedding-ada-002`

¡Eso es todo! Para más información, consulta la [documentación](https://localai.io/models/index.html#embeddings-bert) de LocalAI.
