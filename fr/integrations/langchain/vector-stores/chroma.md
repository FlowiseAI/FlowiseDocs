# Chrome

## Condition préalable

Vous avez besoin d'un serveur Chroma. Tu peux:

1. Installez Chroma CLI et exécutez le serveur en utilisant`chroma run`
2. S'inscrire à[Chroma Cloud](https://trychroma.com/home).
3. Déployez votre propre instance chroma dans[Docker](https://docs.trychroma.com/guides/deploy/docker).

## Installation

| Entrée | Description | Par défaut | Cloud |
| --------------- | -------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------- | ----- |
| Document | Peut être connecté avec les nœuds de[Document Loader](../document-loaders/)                                                                           |                       |       |
| Intégres | Peut être connecté avec les nœuds de[Embeddings](../embeddings/)                                                                                      |                       |       |
| Nom de la collection | Nom de la collection de chroma. Se référer à[here](https://docs.trychroma.com/usage-guide#creating-inspecting-and-deleting-collections)Pour la convention de dénomination |                       |       |
| URL de chroma | Spécifiez l'URL de votre instance de chroma | http: // localhost: 8000 | https://api.trychroma.com:8000 |

Pour Chroma Cloud, vous devrez obtenir votre identifiant de locataire et créer votre base de données et votre clé API.

<gigne> <img src = "../../../. GitBook / Assets / Image (6) (1) (1) (1) (1) (2) (1) .png" alt = "" width = "238"> <Figcaption> </ Figcaption> </ Figure>

### Supplémentaire

Si vous exécutez à la fois Flowise et Chrom sur Docker, des étapes supplémentaires sont impliquées.

1. Tournez d'abord Chroma Docker

```bash
docker compose up -d --build
```

2. Ouvrir`docker-compose.yml`en flux

```bash
cd Flowise && cd docker
```

3. Modifiez le fichier en:

```sh
version: '3.1'

services:
    flowise:
        image: flowiseai/flowise
        restart: always
        environment:
            - PORT=${PORT}
            - DEBUG=${DEBUG}
            - DATABASE_PATH=${DATABASE_PATH}
            - SECRETKEY_PATH=${SECRETKEY_PATH}
            - FLOWISE_SECRETKEY_OVERWRITE=${FLOWISE_SECRETKEY_OVERWRITE}
            - LOG_PATH=${LOG_PATH}
            - LOG_LEVEL=${LOG_LEVEL}
            - EXECUTION_MODE=${EXECUTION_MODE}
        ports:
            - '${PORT}:${PORT}'
        volumes:
            - ~/.flowise:/root/.flowise
        networks:
            - flowise_net
        command: /bin/sh -c "sleep 3; flowise start"
networks:
    flowise_net:
        name: chroma_net
        external: true
```

4. Spin up Flowise Docker Image

```bash
docker compose up -d
```

5. Sur l'URL chroma, pour les systèmes d'exploitation Windows et MacOS spécifiez[http://host.docker.internal:8000](http://host.docker.internal:8000/). Pour les systèmes basés sur Linux, la passerelle Docker par défaut doit être utilisée car host.docker.internal n'est pas disponible:[http://172.17.0.1:8000](http://172.17.0.1:8000/)

<gigne> <img src = "../../../. GitBook / Assets / Image (5) (5) .png" alt = "" width = "256"> <Figcaption> </ Figcaption> </ Figure>

## Ressources

* [LangChain JS Chroma](https://js.langchain.com/docs/modules/indexes/vector_stores/integrations/chroma)
* [Chroma Getting Started](https://docs.trychroma.com/getting-started)
