# ChatOllama

## Prerequisitos

1. Descarga [Ollama](https://github.com/ollama/ollama) o ejecútalo en [Docker.](https://hub.docker.com/r/ollama/ollama)&#x20;
2.  Por ejemplo, puedes usar el siguiente comando para iniciar una instancia de Docker con llama3

    ```bash
    docker run -d -v ollama:/root/.ollama -p 11434:11434 --name ollama ollama/ollama
    docker exec -it ollama ollama run llama3
    ```

## Configuración

1. **Chat Models** > arrastra el nodo **ChatOllama**

<figure><img src="../../../.gitbook/assets/image (139).png" alt="" width="563"><figcaption></figcaption></figure>

2. Completa el modelo que se está ejecutando en Ollama. Por ejemplo: `llama2`. También puedes usar parámetros adicionales:

<figure><img src="../../../.gitbook/assets/image (140).png" alt=""><figcaption></figcaption></figure>

3. ¡Voilà [🎉](https://emojipedia.org/party-popper/), ahora puedes usar el **nodo ChatOllama** en Flowise

<figure><img src="../../../.gitbook/assets/image (141).png" alt=""><figcaption></figcaption></figure>

### Adicional

Si estás ejecutando tanto Flowise como Ollama en docker, tendrás que cambiar la URL Base para ChatOllama.

Para sistemas operativos Windows y MacOS, especifica [http://host.docker.internal:8000](http://host.docker.internal:8000/). Para sistemas basados en Linux, se debe usar el gateway predeterminado de docker ya que host.docker.internal no está disponible: [http://172.17.0.1:8000](http://172.17.0.1:8000/)

<figure><img src="../../../.gitbook/assets/image (142).png" alt="" width="292"><figcaption></figcaption></figure>

## Recursos

* [LangchainJS ChatOllama](https://js.langchain.com/docs/integrations/chat/ollama)
* [Ollama](https://github.com/ollama/ollama)
