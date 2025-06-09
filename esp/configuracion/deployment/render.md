---
description: Learn how to deploy Flowise on Render
---

# Render

## Deploy usando Render

1. Haz click en el botón de abajo para hacer deploy de Flowise en Render

[![Deploy to Render](https://render.com/images/deploy-to-render-button.svg)](https://render.com/deploy?repo=https://github.com/FlowiseAI/Flowise)

2. Haz click en **Connect**

<figure><img src="../../.gitbook/assets/render/1.png" alt=""><figcaption></figcaption></figure>

3. Selecciona **Configure Account**

<figure><img src="../../.gitbook/assets/render/2.png" alt=""><figcaption></figcaption></figure>

4. Haz click en **Connect**

<figure><img src="../../.gitbook/assets/render/3.png" alt=""><figcaption></figcaption></figure>

5. Haz click en **Configure**

<figure><img src="../../.gitbook/assets/render/4.png" alt=""><figcaption></figcaption></figure>

6. Haz click en **Create Web Service**

<figure><img src="../../.gitbook/assets/render/5.png" alt=""><figcaption></figcaption></figure>

7. Espera a que el deployment se complete

<figure><img src="../../.gitbook/assets/render/6.png" alt=""><figcaption></figcaption></figure>

8. Haz click en **Environment** y agrega las siguientes variables:

<figure><img src="../../.gitbook/assets/render/7.png" alt=""><figcaption></figcaption></figure>

9. Haz click en **Add Environment Variable**

<figure><img src="../../.gitbook/assets/render/8.png" alt=""><figcaption></figcaption></figure>

10. Agrega las siguientes variables:

* HOST - `0.0.0.0`
* DATABASE_PATH - `/opt/render/.flowise`
* LOG_PATH - `/opt/render/.flowise/logs`
* SECRETKEY_PATH - `/opt/render/.flowise`
* BLOB_STORAGE_PATH - `/opt/render/.flowise/storage`

<figure><img src="../../.gitbook/assets/image (1) (5).png" alt=""><figcaption></figcaption></figure>

11. Haz click en **Manual Deploy** y luego selecciona **Clear build cache & deploy**

<figure><img src="../../.gitbook/assets/render/11.png" alt=""><figcaption></figcaption></figure>

12. Ahora intenta crear un flujo y guardarlo en Flowise. Luego intenta reiniciar el servicio o hacer redeploy, deberías poder ver el flujo que guardaste previamente.

Mira cómo hacer deploy en Render

{% embed url="https://youtu.be/Fxyc6-frgrI" %}

{% embed url="https://youtu.be/l-0NzOMeCco" %}
