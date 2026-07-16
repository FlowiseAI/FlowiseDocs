# 色度

## 先决条件

您需要一台 Chroma 服务器。您可以：

1. 安装 Chroma CLI 并使用 `chroma run` 运行服务器
2. 注册[Chroma Cloud](https://trychroma.com/home)。
3. 在 [Docker](https://docs.trychroma.com/guides/deploy/docker) 中部署您自己的 Chroma 实例。

## 设置

|输入 |描述 |默认 |云|
| --------------- | -------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------- | ----- |
|文件|可以与 [文档加载器](../document-loaders/) 中的节点连接 |                       |       |
|嵌入 |可以与 [Embeddings](../embeddings/) 中的节点连接 |                       |       |
|收藏名称 |色度集合名称。请参阅[此处](https://docs.trychroma.com/usage-guide#creating-inspecting-and-deleting-collections)了解命名约定 |                       |       |
|色度 URL |指定色度实例的 URL | http://localhost:8000 | https://api.trychroma.com:8000 |

对于 Chroma Cloud，您需要获取租户 ID，并创建数据库和 API 密钥。

<figure><img src="../../../.gitbook/assets/image (6) (1) (1) (1) (1) (2) (1).png" alt="" width="238"><figcaption></figcaption></figure>

### 附加

如果您在 Docker 上运行 Flowise 和 Chroma，则还需要执行其他步骤。

1. 首先启动 Chroma 泊坞窗

```bash
docker compose up -d --build
```

2. 在 Flowise 中打开 `docker-compose.yml`

```bash
cd Flowise && cd docker
```

3、修改文件为：

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

4. 启动 Flowise docker 镜像

```bash
docker compose up -d
```

5. 在 Chroma URL 上，对于 Windows 和 MacOS 操作系统，指定 [http://host.docker.internal:8000](http://host.docker.internal:8000/). 对于基于 Linux 的系统，应使用默认 docker 网关，因为 host.docker.internal 不可用：[http://172.17.0.1:8000](http://172.17.0.1:8000/)

<figure><img src="../../../.gitbook/assets/image (5) (5).png" alt="" width="256"><figcaption></figcaption></figure>

## 资源

* [LangChain JS Chroma](https://js.langchain.com/docs/modules/indexes/vector_stores/integrations/chroma)
* [Chroma 入门](https://docs.trychroma.com/getting-started)
