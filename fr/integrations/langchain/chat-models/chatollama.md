# ChatOllama

## Prérequis

1. Téléchargez [Ollama](https://github.com/ollama/ollama) ou exécutez-le sur [Docker.](https://hub.docker.com/r/ollama/ollama)&#x20;
2. Par exemple, vous pouvez utiliser la commande suivante pour lancer une instance Docker avec llama3

    ```bash
    docker run -d -v ollama:/root/.ollama -p 11434:11434 --name ollama ollama/ollama
    docker exec -it ollama ollama run llama3
    ```

## Configuration

1. **Modèles de chat** > faites glisser le nœud **ChatOllama**

<figure><img src="../../../.gitbook/assets/image (139).png" alt="" width="563"><figcaption></figcaption></figure>

2. Remplissez le modèle qui fonctionne sur Ollama. Par exemple : `llama2`. Vous pouvez également utiliser des paramètres supplémentaires :

<figure><img src="../../../.gitbook/assets/image (140).png" alt=""><figcaption></figcaption></figure>

3. Voilà [🎉](https://emojipedia.org/party-popper/), vous pouvez maintenant utiliser le **nœud ChatOllama** dans Flowise

<figure><img src="../../../.gitbook/assets/image (141).png" alt=""><figcaption></figcaption></figure>

### Supplémentaire

Si vous exécutez à la fois Flowise et Ollama sur Docker, vous devrez changer l'URL de base pour ChatOllama.

Pour les systèmes d'exploitation Windows et MacOS, spécifiez [http://host.docker.internal:8000](http://host.docker.internal:8000/). Pour les systèmes basés sur Linux, la passerelle Docker par défaut doit être utilisée car host.docker.internal n'est pas disponible : [http://172.17.0.1:8000](http://172.17.0.1:8000/)

<figure><img src="../../../.gitbook/assets/image (142).png" alt="" width="292"><figcaption></figcaption></figure>

## Ressources

* [LangchainJS ChatOllama](https://js.langchain.com/docs/integrations/chat/ollama)
* [Ollama](https://github.com/ollama/ollama)