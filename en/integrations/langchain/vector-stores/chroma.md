# Chroma

## Prerequisite

1. Download & Install [Docker ](https://www.docker.com/)and [Git](https://git-scm.com/)
2. Clone [Chroma's repository](https://github.com/chroma-core/chroma) with your terminal

```bash
git clone https://github.com/chroma-core/chroma.git
```

3. Change directory path to your cloned Chroma

```bash
cd chroma
```

4. Run docker compose to build up Chroma image and container

```bash
docker compose up -d --build
```

5. If success, you will be able to see the docker images spun up:

<figure><img src="../../../.gitbook/assets/image (4) (1) (3).png" alt="" width="390"><figcaption></figcaption></figure>

## Setup

| Input           | Description                                                                                                                                        | Default               |
| --------------- | -------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------- |
| Document        | Can be connected with nodes from [Document Loader](../document-loaders/)                                                                           |                       |
| Embeddings      | Can be connected with nodes from [Embeddings](../embeddings/)                                                                                      |                       |
| Collection Name | Chroma collection name. Refer to [here](https://docs.trychroma.com/usage-guide#creating-inspecting-and-deleting-collections) for naming convention |                       |
| Chroma URL      | Specify the URL of your chroma instance                                                                                                            | http://localhost:8000 |

<figure><img src="../../../.gitbook/assets/image (6) (1) (1) (1) (1) (2).png" alt="" width="238"><figcaption></figcaption></figure>

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
            - FLOWISE_USERNAME=${FLOWISE_USERNAME}
            - FLOWISE_PASSWORD=${FLOWISE_PASSWORD}
            - DEBUG=${DEBUG}
            - DATABASE_PATH=${DATABASE_PATH}
            - APIKEY_PATH=${APIKEY_PATH}
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

* [LangChain JS Chroma](https://js.langchain.com/docs/modules/indexes/vector\_stores/integrations/chroma)
* [Chroma Getting Started](https://docs.trychroma.com/getting-started)
