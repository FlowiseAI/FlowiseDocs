---
description: >-
  使用 pgvector 写入更新嵌入数据并根据查询执行相似性搜索
  在 Postgres 上。
---

# Postgres

<figure><img src="../../../.gitbook/assets/image (163).png" alt="" width="292"><figcaption><p>Postgres 节点</p></figcaption></figure>

根据实例的设置方式，有多种方法可以连接到 Postgres。下面是使用 pgvector 团队提供的预构建 Docker 映像进行本地配置的示例。

创建一个名为 `docker-compose.yml` 的文件，内容如下：

```yaml
# Run this command to start the database:
# docker-compose up --build
version: "3"
services:
  db:
    hostname: 127.0.0.1
    image: pgvector/pgvector:pg16
    ports:
      - 5432:5432
    restart: always
    environment:
      - POSTGRES_DB=api
      - POSTGRES_USER=myuser
      - POSTGRES_PASSWORD=ChangeMe
    volumes:
      - ./init.sql:/docker-entrypoint-initdb.d/init.sql
```

`docker compose up` 启动 Postgres 容器。

使用配置的用户和密码创建新凭据：

<figure><img src="../../../.gitbook/assets/image (50).png" alt="" width="526"><figcaption></figcaption></figure>

使用 `docker-compose.yml` 中配置的值填写节点字段。例如：

* 主机：**本地主机**
* 数据库：**api**
* 端口：**5432**

瞧！您现在已经成功设置了 Postgres Vector，可以使用了。

### 故障排除

如果 Flowise 和 Postgres 都在 Docker 上运行，您可能会看到以下错误： <mark style="color:red;">**聚合错误**</mark>。

尝试将主机值从 `localhost` 更改为 `host.docker.internal`

<figure><img src="../../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>
