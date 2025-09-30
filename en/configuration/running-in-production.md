# Running in Production

## Mode

When running in production, we highly recommend using [Queue](running-flowise-using-queue.md) mode with the following settings:

* 2 main servers with load balancing, each starting from 4vCPU 8GB RAM
* 4 workers, each starting from 4vCPU 8GB RAM

You can configure auto scaling depending on the traffic and volume.

## Database

By default, Flowise will use SQLite as the database. However when running at scale, its recommended to use PostgresQL.

## Storage

Currently Flowise only supports [AWS S3](https://aws.amazon.com/s3/) with plan to support more blob storage providers. This will allow files and logs to be stored on S3, instead of local file path. Refer [#for-storage](environment-variables.md#for-storage "mention")

## Encryption

Flowise uses an encryption key to encrypt/decrypt credentials you use such as OpenAI API keys. [AWS Secret Manager](https://aws.amazon.com/secrets-manager/) is recommended to be used in production for better security control and key rotation. Refer [#for-credentials](environment-variables.md#for-credentials "mention")

## Rate Limit

When deployed to cloud/on-prem, most likely the instances are behind a proxy/load balancer. The IP address of the request might be the IP of the load balancer/reverse proxy, making the rate limiter effectively a global one and blocking all requests once the limit is reached or `undefined`. Setting the correct `NUMBER_OF_PROXIES` can resolve the issue. Refer [#rate-limit-setup](rate-limit.md#rate-limit-setup "mention")

## Load Testing

Artillery can be used to load testing your deployed Flowise application. Example script can be found [here](https://github.com/FlowiseAI/Flowise/blob/main/artillery-load-test.yml).
