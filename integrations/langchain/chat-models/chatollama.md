# ChatOllama

## Prerequisite

1. Download [Ollama](https://github.com/ollama/ollama) or run it on [Docker.](https://hub.docker.com/r/ollama/ollama)&#x20;
2.  For example, you can use the following command to spin up a Docker instance with llama3

    ```bash
    docker run -d -v ollama:/root/.ollama -p 11434:11434 --name ollama ollama/ollama
    docker exec -it ollama ollama run llama3
    ```

## Setup

1. **Chat Models** > drag **ChatOllama** node

<figure><img src="../../../.gitbook/assets/image (139).png" alt="" width="563"><figcaption></figcaption></figure>

2. Fill in the model that is running on Ollama. For example: `llama2`. You can also use additional parameters:

<figure><img src="../../../.gitbook/assets/image (140).png" alt=""><figcaption></figcaption></figure>

3. Voila [🎉](https://emojipedia.org/party-popper/), you can now use **ChatOllama node** in Flowise

<figure><img src="../../../.gitbook/assets/image (141).png" alt=""><figcaption></figcaption></figure>

### Additional

If you are running both Flowise and Ollama on docker. You'll have to change the Base URL for ChatOllama.

For Windows and MacOS Operating Systems specify [http://host.docker.internal:8000](http://host.docker.internal:8000/). For Linux based systems the default docker gateway should be used since host.docker.internal is not available: [http://172.17.0.1:8000](http://172.17.0.1:8000/)

<figure><img src="../../../.gitbook/assets/image (142).png" alt="" width="292"><figcaption></figcaption></figure>

## Resources

* [LangchainJS ChatOllama](https://js.langchain.com/docs/integrations/chat/ollama)
* [Ollama](https://github.com/ollama/ollama)
