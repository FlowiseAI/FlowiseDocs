---
description: >-
  Upsert embedded data and perform similarity search upon query using pgvector
  on Postgres.
---

# Postgres

<figure><img src="../../../.gitbook/assets/image (163).png" alt="" width="292"><figcaption><p>Postgres Node</p></figcaption></figure>

There are multiple methods to connect to Postgres based on how your instance is set up. Below is an example of a local configuration using a prebuilt Docker image provided by the pgvector team.

Create a file named `docker-compose.yml` with the content below:

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

`docker compose up` to start the Postgres container.

Create new credential with the configured user and password:

<figure><img src="../../../.gitbook/assets/image (50).png" alt="" width="526"><figcaption></figcaption></figure>

Fill in the node's field with values configured in `docker-compose.yml`. For example:

* Host: **localhost**
* Database: **api**
* Port: **5432**

Voila! You have now successfully setup Postgres Vector ready to be used.
