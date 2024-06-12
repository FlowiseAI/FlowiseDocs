---
description: Learn how to deploy Flowise on Zeabur
---

# Zeabur

***

{% hint style="warning" %}
Please note that the following template made by Zeabur is outdated (from 2024-01-24).
{% endhint %}

1. Click the following prebuilt [template](https://zeabur.com/templates/2JYZTR) or the button below.

[![Deploy on Zeabur](https://zeabur.com/button.svg)](https://zeabur.com/templates/2JYZTR)

2. Click Deploy

<figure><img src="../../.gitbook/assets/zeabur/1.png" alt="zeabur template"><figcaption></figcaption></figure>

3. Select your favorite region and continue

<figure><img src="../../.gitbook/assets/zeabur/2.png" alt="select region"><figcaption></figcaption></figure>

4. You will be redirected to Zeabur's dashboard and you will see the deployment process

<figure><img src="../../.gitbook/assets/zeabur/3.png" alt="deployment process"><figcaption></figcaption></figure>

5. To add authorization, navigate to Variables tab and add:

* FLOWISE\_USERNAME
* FLOWISE\_PASSWORD

<figure><img src="../../.gitbook/assets/zeabur/4.png" alt="authorization"><figcaption></figcaption></figure>

6. There are list of env variables you can configure. Refer to [environment-variables.md](../environment-variables.md "mention")

That's it! You now have a deployed Flowise on Zeabur [🎉](https://emojipedia.org/party-popper/)[🎉](https://emojipedia.org/party-popper/)

## Persistent Volume

Zeabur will automatically create a persistent volume for you so you don't have to worry about it.
