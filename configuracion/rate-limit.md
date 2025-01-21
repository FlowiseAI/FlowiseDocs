---
description: Aprende cómo gestionar las API requests en Flowise
---

# Rate Limit

***

Cuando compartes tu chatflow al público sin API authorization a través de API o embedded chat, cualquiera puede acceder al flow. Para prevenir el spamming, puedes establecer el rate limit en tu chatflow.

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="462"><figcaption></figcaption></figure>

* **Message Limit per Duration**: Cuántos messages pueden ser recibidos en una duración específica. Ej: 20
* **Duration in Seconds**: La duración especificada. Ej: 60
* **Limit Message**: Qué message retornar cuando se excede el límite. Ej: Quota Exceeded

Usando el ejemplo anterior, significa que solo 20 messages están permitidos ser recibidos en 60 seconds. El rate limitation es tracked por IP-address. Si has deployed Flowise en cloud service, tendrás que establecer la environment variable `NUMBER_OF_PROXIES`.

## Cloud-Hosted Rate Limit Setup Guide

1. **Cloud Host Flowise:** Comienza haciendo hosting de Flowise en la cloud.
2. **Set Environment Variable:** Create una environment variable llamada `NUMBER_OF_PROXIES` y set su value a `0` en tu hosting environment.
3. **Restart Cloud-Hosted Flowise Service:** Esto permite a Flowise aplicar los changes de environment variables.
4. **Check IP Address:** Para verificar la IP address, accede a la siguiente URL: `{{hosted_url}}/api/v1/ip`. Puedes hacer esto ya sea ingresando la URL en tu web browser o haciendo una API request.
5. **Compare IP Address** Después de hacer la request, compara la IP address retornada con tu current IP address. Puedes encontrar tu current IP address visitando cualquiera de estos websites:
   * [http://ip.nfriedly.com/](http://ip.nfriedly.com/)
   * [https://api.ipify.org/](https://api.ipify.org/)
6. **Incorrect IP Address:** Si la returned IP address no coincide con tu current IP address, incrementa `NUMBER_OF_PROXIES` en 1 y restart Cloud-Hosted Flowise. Repite este proceso hasta que la IP address coincida con la tuya.
