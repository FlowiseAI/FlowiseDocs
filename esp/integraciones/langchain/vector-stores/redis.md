# Redis

## Prerequisitos

1. Inicia un [Redis-Stack Server](https://redis.io/docs/latest/operate/oss_and_stack/install/install-stack/docker/) usando Docker

```bash
docker run -d --name redis-stack-server -p 6379:6379 redis/redis-stack-server:latest
```

## Configuración

1. Añade un nuevo nodo **Redis** en el canvas.
2. Crea una nueva credencial de Redis.

<figure><img src="../../../.gitbook/assets/image (1) (1) (3) (1) (1).png" alt="" width="257"><figcaption></figcaption></figure>

3. Selecciona el tipo de credencial de Redis. Elige Redis API si tienes nombre de usuario y contraseña, de lo contrario elige Redis URL:

<figure><img src="../../../.gitbook/assets/image (2) (1) (1) (2).png" alt="" width="563"><figcaption></figcaption></figure>

4. Completa la URL:

<figure><img src="../../../.gitbook/assets/image (3) (1) (1) (1) (2) (1).png" alt="" width="542"><figcaption></figcaption></figure>

5. Ahora puedes comenzar a hacer upsert de datos con Redis:

<figure><img src="../../../.gitbook/assets/image (8) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (9) (2).png" alt=""><figcaption></figcaption></figure>

6. Navega al portal de Redis Insight, y en tu base de datos, podrás ver todos los datos que se han insertado:

<figure><img src="../../../.gitbook/assets/image (138).png" alt=""><figcaption></figcaption></figure>
