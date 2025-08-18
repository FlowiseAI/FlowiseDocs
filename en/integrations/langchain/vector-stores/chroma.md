# Chroma

## Prerequisite

You need a Chroma server. You can:

1. Install Chroma CLI and run the server using `chroma run`
2. Sign up for [Chroma Cloud](https://trychroma.com/home).
3. Deploy your own Chroma instance in [Docker](https://docs.trychroma.com/guides/deploy/docker).

## Setup

| Input           | Description                                                                                                                                        | Default               | Cloud |
| --------------- | -------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------- | ----- |
| Document        | Can be connected with nodes from [Document Loader](../document-loaders/)                                                                           |                       |       |
| Embeddings      | Can be connected with nodes from [Embeddings](../embeddings/)                                                                                      |                       |       |
| Collection Name | Chroma collection name. Refer to [here](https://docs.trychroma.com/usage-guide#creating-inspecting-and-deleting-collections) for naming convention |                       |       |
| Chroma URL      | Specify the URL of your chroma instance                                                                                                            | http://localhost:8000 | https://api.trychroma.com:8000 |

For Chroma Cloud, you will need to get your tenant ID, and create your database and API key.

<figure><img src="../../../.gitbook/assets/image (6) (1) (1) (1) (1) (2) (1).png" alt="" width="238"><figcaption></figcaption></figure>

### Additional

If you are running both Flowise and Chroma on Docker, there are additional steps involved.

1. Spin up Chroma docker first

```bash
docker compose up -d --build
```

2. Open `docker-compose.yml` in Flowise

```bash
cd Flowise && cd docker
```

3. Modify the file to:

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

4. Spin up Flowise docker image

```bash
docker compose up -d
```

5. On the Chroma URL, for Windows and MacOS Operating Systems specify [http://host.docker.internal:8000](http://host.docker.internal:8000/). For Linux based systems the default docker gateway should be used since host.docker.internal is not available: [http://172.17.0.1:8000](http://172.17.0.1:8000/)

<figure><img src="../../../.gitbook/assets/image (5) (5).png" alt="" width="256"><figcaption></figcaption></figure>

## Resources

* [LangChain JS Chroma](https://js.langchain.com/docs/modules/indexes/vector_stores/integrations/chroma)
* [Chroma Getting Started](https://docs.trychroma.com/getting-started)
