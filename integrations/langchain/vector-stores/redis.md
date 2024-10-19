# Redis

## Prerequisite

1. Spin up a [Redis-Stack Server](https://redis.io/docs/latest/operate/oss\_and\_stack/install/install-stack/docker/) using Docker

```bash
docker run -d --name redis-stack-server -p 6379:6379 redis/redis-stack-server:latest
```

## Setup

1. Add a new **Redis** node on canvas.
2. Create new Redis credential.

<figure><img src="../../../.gitbook/assets/image (1) (1) (3) (1) (1).png" alt="" width="257"><figcaption></figcaption></figure>

3. Select type of Redis Credential. Choose Redis API if you have username and password, otherwise Redis URL:

<figure><img src="../../../.gitbook/assets/image (2) (1) (1) (2).png" alt="" width="563"><figcaption></figcaption></figure>

4. Fill in the url:

<figure><img src="../../../.gitbook/assets/image (3) (1) (1) (1) (2) (1).png" alt="" width="542"><figcaption></figcaption></figure>

5. Now you can start upserting data with Redis:

<figure><img src="../../../.gitbook/assets/image (8) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (9) (2).png" alt=""><figcaption></figcaption></figure>

6. Navigate to Redis Insight portal, and to your database, you will be able to see all the data that has been upserted:

<figure><img src="../../../.gitbook/assets/image (138).png" alt=""><figcaption></figcaption></figure>
