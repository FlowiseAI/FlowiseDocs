---
description: Learn how to set up chatflow-level access control for your Flowise instances
---

# Flows

***

After you have a chatflow / agentflow constructed, by default, your flow is available to public. Anyone that has access to the Chatflow ID is able to run prediction through Embed or API.

In cases where you might want to allow certain people to be able to access and interact with it, you can do so by assigning an API key for that specific chatflow.

## API Key

In dashboard, navigate to API Keys section, and you should be able to see a DefaultKey created. You can also add or delete any keys.

<figure><img src="../../.gitbook/assets/image (6) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

## Chatflow

Navigate to the chatflow, and now you can select the API Key you want to use to protect the chatflow.

<figure><img src="../../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

After assigning an API key, one can only access the chatflow API when the Authorization header is provided with the correct API key specified during a HTTP call.

```json
"Authorization": "Bearer <your-api-key>"
```

An example of calling the API using POSTMAN

<figure><img src="../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

