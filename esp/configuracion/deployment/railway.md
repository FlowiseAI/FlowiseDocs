---
description: Learn how to deploy Flowise on Railway
---

# Railway

## Deploy usando Railway

1. Haz click en el botón de abajo para hacer deploy de Flowise en Railway

[![Deploy on Railway](https://railway.app/button.svg)](https://railway.app/template/YK7J0v)

2. Haz click en **Deploy Now**

<figure><img src="../../.gitbook/assets/railway/1.png" alt=""><figcaption></figcaption></figure>

3. Selecciona **Configure**

<figure><img src="../../.gitbook/assets/railway/2.png" alt=""><figcaption></figcaption></figure>

4. Haz click en **Deploy**

<figure><img src="../../.gitbook/assets/railway/3.png" alt=""><figcaption></figcaption></figure>

5. Espera a que el deployment se complete

<figure><img src="../../.gitbook/assets/railway/4.png" alt=""><figcaption></figcaption></figure>

6. Haz click en **Settings** y luego en **Generate Domain**

<figure><img src="../../.gitbook/assets/railway/5.png" alt=""><figcaption></figcaption></figure>

## Persistent Storage

El filesystem por defecto para servicios corriendo en Railway es efímero. Los datos de Flowise no persisten entre deploys y reinicios. Para resolver este problema, podemos usar [Railway Volume](https://docs.railway.app/reference/volumes).

Para facilitar los pasos, tenemos un template de Railway con volumen montado: [https://railway.app/template/nEGbjR](https://railway.app/template/nEGbjR)

Solo haz click en Deploy y completa las Env Variables como abajo:

* DATABASE_PATH - `/opt/railway/.flowise`
* LOG_PATH - `/opt/railway/.flowise/logs`
* SECRETKEY_PATH - `/opt/railway/.flowise`
* BLOB_STORAGE_PATH - `/opt/railway/.flowise/storage`

<figure><img src="../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="420"><figcaption></figcaption></figure>

Ahora intenta crear un flujo y guardarlo en Flowise. Luego intenta reiniciar el servicio o hacer redeploy, deberías poder ver el flujo que guardaste previamente.
