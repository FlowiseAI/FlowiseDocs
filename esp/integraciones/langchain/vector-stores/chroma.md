# Chroma

## Prerequisitos

1. Descarga e Instala [Docker ](https://www.docker.com/)y [Git](https://git-scm.com/)
2. Clona el [repositorio de Chroma](https://github.com/chroma-core/chroma) con tu terminal

```bash
git clone https://github.com/chroma-core/chroma.git
```

3. Cambia el directorio a tu Chroma clonado

```bash
cd chroma
```

4. Ejecuta docker compose para construir la imagen y el contenedor de Chroma

```bash
docker compose up -d --build
```

5. Si tiene éxito, podrás ver las imágenes de docker iniciadas:

<figure><img src="../../../.gitbook/assets/image (4) (1) (3).png" alt="" width="390"><figcaption></figcaption></figure>

## Configuración

| Input           | Descripción                                                                                                                                        | Valor por defecto     |
| --------------- | -------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------- |
| Document        | Puede conectarse con nodos de [Document Loader](../document-loaders/)                                                                              |                       |
| Embeddings      | Puede conectarse con nodos de [Embeddings](../embeddings/)                                                                                         |                       |
| Collection Name | Nombre de la collection de Chroma. Consulta [aquí](https://docs.trychroma.com/usage-guide#creating-inspecting-and-deleting-collections) las convenciones de nombres |                       |
| Chroma URL      | Especifica la URL de tu instancia de Chroma                                                                                                        | http://localhost:8000 |

<figure><img src="../../../.gitbook/assets/image (6) (1) (1) (1) (1) (2).png" alt="" width="238"><figcaption></figcaption></figure>

### Adicional

Si estás ejecutando tanto Flowise como Chroma en Docker, hay pasos adicionales involucrados.

1. Inicia primero el docker de Chroma

```bash
docker compose up -d --build
```

2. Abre `docker-compose.yml` en Flowise

```bash
cd Flowise && cd docker
```

3. Modifica el archivo a:

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

4. Inicia la imagen docker de Flowise

```bash
docker compose up -d
```

5. En la URL de Chroma, para sistemas operativos Windows y MacOS especifica [http://host.docker.internal:8000](http://host.docker.internal:8000/). Para sistemas basados en Linux, se debe usar el gateway por defecto de docker ya que host.docker.internal no está disponible: [http://172.17.0.1:8000](http://172.17.0.1:8000/)

<figure><img src="../../../.gitbook/assets/image (5) (5).png" alt="" width="256"><figcaption></figcaption></figure>

## Recursos

* [LangChain JS Chroma](https://js.langchain.com/docs/modules/indexes/vector_stores/integrations/chroma)
* [Chroma Getting Started](https://docs.trychroma.com/getting-started)
