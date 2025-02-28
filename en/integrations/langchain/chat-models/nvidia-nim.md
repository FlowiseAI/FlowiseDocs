# Nvidia NIM

## Prerequisite

1. Log in or sign up to [Nvidia](https://build.nvidia.com/).
2. From the top navigation bar, click NIM:

<figure><img src="../../../.gitbook/assets/image (247).png" alt=""><figcaption></figcaption></figure>

3. Search for the model you would like to use. To download it locally, we will be using Docker:

<figure><img src="../../../.gitbook/assets/image (248).png" alt=""><figcaption></figcaption></figure>

4. Follow the instructions from the Docker setup. You must first get an API Key to pull the Docker image:

<figure><img src="../../../.gitbook/assets/image (249).png" alt="" width="563"><figcaption></figcaption></figure>

## Flowise

1. **Chat Models** > drag **Chat NvidiaNIM** node

<figure><img src="../../../.gitbook/assets/image (250).png" alt=""><figcaption></figcaption></figure>

2. If you are using Nvidia hosted endpoint, you must have your API key. **Connect Credential** > click **Create New.** However if you are using local setup, this is optional.

<div align="left"><figure><img src="../../../.gitbook/assets/image (251).png" alt=""><figcaption></figcaption></figure> <figure><img src="../../../.gitbook/assets/Screenshot 2024-12-23 180712.png" alt=""><figcaption></figcaption></figure></div>

3. Put in the model name and voila [🎉](https://emojipedia.org/party-popper/), your **Nvidia NIM node** is now ready to be used in Flowise!

<figure><img src="../../../.gitbook/assets/image (252).png" alt=""><figcaption></figcaption></figure>

## Resources

* [Nvidia LLM Getting Started](https://docs.nvidia.com/nim/large-language-models/latest/getting-started.html)
* [Nvidia NIM](https://build.nvidia.com/microsoft/phi-3-mini-4k?snippet_tab=Docker)
