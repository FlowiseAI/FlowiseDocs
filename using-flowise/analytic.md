# Analytic

There are several analytic providers Flowise integrates with:

* [Langsmith](https://smith.langchain.com/)
* [Langfuse](https://langfuse.com/)
* [LLMonitor](https://lunary.ai/)

## UI

1. At the top right corner, click Analyse Chatflow:

<figure><img src="../.gitbook/assets/image (80).png" alt="" width="224"><figcaption></figcaption></figure>

2. You will see the list of providers along with configuration:

<figure><img src="../.gitbook/assets/image (82).png" alt="" width="563"><figcaption></figcaption></figure>

3. Simply fill in the credential and other configuration, then turn it ON:

<figure><img src="../.gitbook/assets/image (83).png" alt="" width="563"><figcaption></figcaption></figure>

## API

Once the analytic has been turned ON from the UI, you can override or provide additional configuration  in the body of the [Prediction API](api.md#prediction-api):

```json
{
    "question": "hi there",
    "overrideConfig": {
        "analytics": {
            "langFuse": { // langSmith, langFuse, llmonitor
                "userId": "user1"
            }
        }
    }
}
```
