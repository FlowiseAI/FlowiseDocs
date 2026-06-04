# ChatOpenAI Custom

Use the **ChatOpenAI Custom** node when you want to connect Flowise to an OpenAI-compatible chat completion API, including custom deployments, proxy gateways, and third-party providers.

This node lets you set the model name directly and override the default API base path, so it is useful when the provider exposes the same request shape as OpenAI but uses different model IDs or a different endpoint.

## Setup

1. In the Flowise canvas, open **Chat Models** and drag the **ChatOpenAI Custom** node into your flow.
2. Click **Connect Credential** and create a new OpenAI credential.
3. Paste the API key from your OpenAI-compatible provider.
4. Enter the provider's model ID in **Model Name**.
5. Open **Additional Parameters**.
6. Set **Base Path** to the provider's OpenAI-compatible `/v1` endpoint.
7. Connect the node to a chain, agent, or other component that accepts a chat model.

## WaveSpeed LLM

[WaveSpeed LLM](https://wavespeed.ai/llm) provides access to models from multiple vendors through an OpenAI-compatible API.

Use these values in **ChatOpenAI Custom**:

| Field | Value |
| --- | --- |
| Credential API key | Your WaveSpeed API key |
| Model Name | A WaveSpeed model ID such as `deepseek/deepseek-v4-flash` |
| Base Path | `https://llm.wavespeed.ai/v1` |

Model IDs use a `vendor/model` format. Copy the model ID from the [WaveSpeed LLM catalog](https://wavespeed.ai/llm), because model names are case-sensitive and provider-prefixed.

## Troubleshooting

* If requests return `401 Unauthorized`, confirm that the credential contains a valid API key for your provider.
* If requests return `404 Not Found` or `model not found`, use the full model ID required by your provider.
* If requests fail to connect, confirm that **Base Path** is correctly configured for your provider's endpoint.
* If tool calling or image input does not work as expected, verify that the selected model supports that capability before enabling it in your flow.
