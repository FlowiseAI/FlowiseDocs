---
description: >-
  Realiza upsert de datos embedidos y ejecuta búsquedas de similitud sobre consultas usando pgvector
  en Postgres.
---

# Postgres

<figure><img src="../../../.gitbook/assets/image (163).png" alt="" width="292"><figcaption><p>Nodo Postgres</p></figcaption></figure>

Hay múltiples métodos para conectarse a Postgres según cómo esté configurada tu instancia. A continuación se muestra un ejemplo de una configuración local usando una imagen Docker precompilada proporcionada por el equipo de pgvector.

Crea un archivo llamado `docker-compose.yml` con el siguiente contenido:

```yaml
# Ejecuta este comando para iniciar la base de datos:
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

Usa `docker compose up` para iniciar el contenedor de Postgres.

Crea una nueva credencial con el usuario y contraseña configurados:

<figure><img src="../../../.gitbook/assets/image (50).png" alt="" width="526"><figcaption></figcaption></figure>

Completa los campos del nodo con los valores configurados en `docker-compose.yml`. Por ejemplo:

* Host: **localhost**
* Database: **api**
* Port: **5432**

¡Voilà! Ahora has configurado exitosamente Postgres Vector listo para ser usado.
