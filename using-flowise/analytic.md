---
description: Learn how to analyze and troubleshoot your chatflows and agentflows
---

# Analytic

***

There are several analytic providers Flowise integrates with:

* [Langsmith](https://smith.langchain.com/)
* [Langfuse](https://langfuse.com/)
* [LunaryAI](https://lunary.ai/)

## Setup

1. At the top right corner of your Chatflow or Agentflow, click **Settings** > **Configuration**

<figure><img src="../.gitbook/assets/analytic-1.webp" alt="" width="375"><figcaption></figcaption></figure>

2. Then go to the Analyse Chatflow section

<figure><img src="../.gitbook/assets/analytic-2 (1).webp" alt=""><figcaption></figcaption></figure>

3. You will see a list of providers, along with their configuration fields

<figure><img src="../.gitbook/assets/image (82).png" alt="" width="563"><figcaption></figcaption></figure>

3. Fill in the credentials and other configuration details, then turn the provider **ON**

<figure><img src="../.gitbook/assets/image (83).png" alt="" width="563"><figcaption></figcaption></figure>

## API

Once the analytic has been turned ON from the UI, you can override or provide additional configuration in the body of the [Prediction API](api.md#prediction-api):

```json
{
    "question": "hi there",
    "overrideConfig": {
        "analytics": {
            "langFuse": { // langSmith, langFuse, lunary
                "userId": "user1"
            }
        }
    }
}
```
