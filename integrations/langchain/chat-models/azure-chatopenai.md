# ChatOpenAI

## Prerequisite

1. An [OpenAI](https://openai.com/) account
2. Create an [API key](https://platform.openai.com/api-keys)

## Setup

1. **Chat Models** > drag **ChatOpenAI** node

<figure><img src="../../../.gitbook/assets/image (10).png" alt="" width="563"><figcaption></figcaption></figure>

2. **Connect Credential** > click **Create New**

<figure><img src="../../../.gitbook/assets/image (1) (1).png" alt="" width="278"><figcaption></figcaption></figure>

3. Fill in the **ChatOpenAI** credential

<figure><img src="../../../.gitbook/assets/image (2) (1).png" alt="" width="563"><figcaption></figcaption></figure>

4. Voila [🎉](https://emojipedia.org/party-popper/), you can now use **ChatOpenAI node** in Flowise

<figure><img src="../../../.gitbook/assets/image (3) (1).png" alt=""><figcaption></figcaption></figure>

## Custom base URL and headers

Flowise supports using custom base URL and headers for Chat OpenAI. Users can easily use integrations like OpenRouter, TogetherAI and others that support OpenAI API compatibility.

### TogetherAI

1. Refer to official [docs](https://docs.together.ai/docs/openai-api-compatibility#nodejs) from TogetherAI
2. Create a new credential with TogetherAI API key
3. Click **Additional Parameters** on ChatOpenAI node.
4. Change the Base Path:

<figure><img src="../../../.gitbook/assets/image (4) (1).png" alt="" width="563"><figcaption></figcaption></figure>

### Open Router

1. Refer to official [docs](https://openrouter.ai/docs#quick-start) from OpenRouter
2. Create a new credential with OpenRouter API key
3. Click Additional Parameters on ChatOpenAI node
4. Change the Base Path and Base Options:

<figure><img src="../../../.gitbook/assets/image (5) (1).png" alt="" width="563"><figcaption></figcaption></figure>

## Custom Model

For models that are not supported on ChatOpenAI node, you can use ChatOpenAI Custom for that. This allow users to fill in model name such as `mistralai/Mixtral-8x7B-Instruct-v0.1`

<figure><img src="../../../.gitbook/assets/image (84).png" alt=""><figcaption></figcaption></figure>
