# Running Flowise using Queue

By default, Flowise runs in a NodeJS main thread. However, with large number of predictions, this does not scale well. Therefore there are 2 modes you can configure: `main` (default) and `queue`.

## Queue Mode

With the following environment variables, you can run Flowise in `queue` mode.

<table><thead><tr><th width="263">Variable</th><th>Description</th><th>Type</th><th>Default</th></tr></thead><tbody><tr><td>MODE</td><td>Mode to run Flowise</td><td>Enum String: <code>main</code>, <code>queue</code></td><td><code>main</code></td></tr><tr><td>WORKER_CONCURRENCY</td><td>How many jobs are allowed to be processed in parallel for a worker. If you have 1 worker, that means how many concurrent prediction tasks it can handle. More <a href="https://docs.bullmq.io/guide/workers/concurrency">info</a></td><td>Number</td><td>10000</td></tr><tr><td>QUEUE_NAME</td><td>The name of the message queue</td><td>String</td><td>flowise-queue</td></tr><tr><td>QUEUE_REDIS_EVENT_STREAM_MAX_LEN</td><td>Event stream is auto-trimmed so that its size does not grow too much. More <a href="https://docs.bullmq.io/guide/events">info</a></td><td>Number</td><td>10000</td></tr><tr><td>REDIS_HOST</td><td>Redis host</td><td>String</td><td>localhost</td></tr><tr><td>REDIS_PORT</td><td>Redis port</td><td>Number</td><td>6379</td></tr><tr><td>REDIS_USERNAME</td><td>Redis username (optional)</td><td>String</td><td></td></tr><tr><td>REDIS_PASSWORD</td><td>Redis password (optional)</td><td>String</td><td></td></tr><tr><td>REDIS_TLS</td><td>Redis TLS connection (optional) More <a href="https://redis.io/docs/latest/operate/oss_and_stack/management/security/encryption/">info</a></td><td>Boolean</td><td>false</td></tr><tr><td>REDIS_CERT</td><td>Redis self-signed certificate</td><td>String</td><td></td></tr><tr><td>REDIS_KEY</td><td>Redis self-signed certificate key file</td><td>String</td><td></td></tr><tr><td>REDIS_CA</td><td>Redis self-signed certificate CA file</td><td>String</td><td></td></tr></tbody></table>

In `queue` mode, the main server will be responsible for processing requests, sending jobs to message queue. Main server will not execute the job. One or multiple workers receive jobs from the queue, execute them and send the results back.

This allows for dynamic scaling: you can add workers to handle increased workloads or remove them during lighter periods.

Here's how it works:

1. The main server receive prediction or other requests from the web, adding them as jobs to the queue.
2. These job queues are essential lists of tasks waiting to be processed. Workers, which are essentially separate processes or threads, pick up these jobs and execute them.
3. Once the job is completed, the worker:
   * Write the results in the database.
   * Send an event to indicate the completion of the job.
4. Main server receive the event, and send the result back to UI.
5. Redis pub/sub is also used for streaming data back to UI.

<figure><img src="../.gitbook/assets/Untitled-2025-01-23-1520.png" alt=""><figcaption></figcaption></figure>

## Start Redis

Before starting main server and workers, Redis need to be running first. You can run Redis on a separate machine, but make sure that it's accessible by the server and worker instances.

For example, you can get Redis running on your Docker following this [guide](https://www.docker.com/blog/how-to-use-the-redis-docker-official-image/).

## Configure Main Server

This is the same as you were to run Flowise by default, with the exceptions of configuring the environment variables mentioned above.

## Configure Worker

Same as main server, environment variables above must be configured. We recommend just using the same `.env` file for both main and worker instances. The only difference is how to run the workers.

{% hint style="warning" %}
Main server and worker need to share the same secret key. Refer to [#for-credentials](environment-variables.md#for-credentials "mention"). For production, we recommend using Postgres as database for perfomance.
{% endhint %}

### Running Flowise locally using NPM

```bash
npx flowise worker # remember to pass in the env vars!
```

### Docker Compose

You can either use the `docker-compose.yml` provided [here](https://github.com/FlowiseAI/Flowise/tree/main/docker/worker) or reuse the same `docker-compose.yml` you were using for main server, but change the entrypoint from `flowise start`to `flowise worker`:

```docker
version: '3.1'

services:
    flowise:
        image: flowiseai/flowise
        restart: always
        environment:
            - PORT=${PORT}
            ....
            - MODE=${MODE}
            - WORKER_CONCURRENCY=${WORKER_CONCURRENCY}
            ....
        ports:
            - '${PORT}:${PORT}'
        volumes:
            - ~/.flowise:/root/.flowise
        entrypoint: /bin/sh -c "sleep 3; flowise worker"
```

### Git Clone

Open 1st terminal to run main server

```bash
pnpm start
```

Other terminals to run worker

```bash
pnpm start-worker
```

### AWS Terraform

_Coming soon_

## Queue Dashboard

You can view all the jobs, their status, result, data by navigating to `<your-flowise-url.com>/admin/queues`

<figure><img src="../.gitbook/assets/image (253).png" alt=""><figcaption></figcaption></figure>

