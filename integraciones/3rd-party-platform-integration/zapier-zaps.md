---
description: Aprende cómo integrar Flowise y Zapier
---

# Zapier Zaps

***

## Prerequisitos

1. [Inicia sesión](https://zapier.com/app/login) o [regístrate](https://zapier.com/sign-up) en Zapier
2. Consulta [deployment](../../configuration/deployment/) para crear una versión alojada en la nube de Flowise.

## Configuración

1. Ve a [Zapier Zaps](https://zapier.com/app/zaps)
2. Haz clic en **Create**

<figure><img src="../../.gitbook/assets/zapier/zap/1.png" alt=""><figcaption></figcaption></figure>

### Recibir Mensaje Trigger

1.  Haz clic o busca **Discord**

    <figure><img src="../../.gitbook/assets/zapier/zap/2.png" alt="" width="563"><figcaption></figcaption></figure>
2.  Selecciona **New Message Posted to Channel** como Event y haz clic en **Continue**

    <figure><img src="../../.gitbook/assets/zapier/zap/3.png" alt="" width="563"><figcaption></figcaption></figure>
3.  **Inicia sesión** en tu cuenta de Discord

    <figure><img src="../../.gitbook/assets/zapier/zap/4.png" alt="" width="563"><figcaption></figcaption></figure>
4.  Añade **Zapier Bot** a tu servidor preferido

    <figure><img src="../../.gitbook/assets/zapier/zap/5.png" alt="" width="272"><figcaption></figcaption></figure>
5.  Otorga los permisos apropiados y haz clic en **Authorize**, luego haz clic en **Continue**

    <figure><img src="../../.gitbook/assets/zapier/zap/6.png" alt="" width="292"><figcaption></figcaption></figure>

    <figure><img src="../../.gitbook/assets/zapier/zap/7.png" alt="" width="290"><figcaption></figcaption></figure>
6.  Selecciona tu **canal preferido** para interactuar con Zapier Bot y haz clic en **Continue**

    <figure><img src="../../.gitbook/assets/zapier/zap/8.png" alt="" width="563"><figcaption></figcaption></figure>
7.  **Envía un mensaje** a tu canal seleccionado en el paso 8

    <figure><img src="../../.gitbook/assets/zapier/zap/9.png" alt="" width="563"><figcaption></figcaption></figure>
8.  Haz clic en **Test trigger**

    <figure><img src="../../.gitbook/assets/zapier/zap/10.png" alt="" width="563"><figcaption></figcaption></figure>
9.  Selecciona tu mensaje y haz clic en **Continue with the selected record**

    <figure><img src="../../.gitbook/assets/zapier/zap/11.png" alt="" width="563"><figcaption></figcaption></figure>

### Filtrar Mensajes del Zapier Bot

1.  Haz clic o busca **Filter**

    <figure><img src="../../.gitbook/assets/zapier/zap/12.png" alt="" width="563"><figcaption></figcaption></figure>
2.  Configura el **Filter** para no continuar si se recibe un mensaje del **Zapier Bot** y haz clic en **Continue**

    <figure><img src="../../.gitbook/assets/zapier/zap/13.png" alt="" width="563"><figcaption></figcaption></figure>

### FlowiseAI genera Mensaje de Resultado

1.  Haz clic en **+**, haz clic o busca **FlowiseAI**

    <figure><img src="../../.gitbook/assets/zapier/zap/14.png" alt="" width="563"><figcaption></figcaption></figure>
2.  Selecciona **Make Prediction** como Event y haz clic en **Continue**

    <figure><img src="../../.gitbook/assets/zapier/zap/15.png" alt="" width="563"><figcaption></figcaption></figure>
3.  Haz clic en **Sign in** e inserta tus datos, luego haz clic en **Yes, Continue to FlowiseAI**

    <figure><img src="../../.gitbook/assets/zapier/zap/16.png" alt="" width="563"><figcaption></figcaption></figure>

    <figure><img src="../../.gitbook/assets/zapier/zap/17.png" alt="" width="563"><figcaption></figcaption></figure>
4.  Selecciona **Content** de Discord y tu Flow ID, luego haz clic en **Continue**

    <figure><img src="../../.gitbook/assets/zapier/zap/18.png" alt="" width="563"><figcaption></figcaption></figure>
5.  Haz clic en **Test action** y espera tu resultado

    <figure><img src="../../.gitbook/assets/zapier/zap/19.png" alt="" width="563"><figcaption></figcaption></figure>

### Enviar Mensaje de Resultado

1.  Haz clic en **+**, haz clic o busca **Discord**

    <figure><img src="../../.gitbook/assets/zapier/zap/20.png" alt="" width="563"><figcaption></figcaption></figure>
2.  Selecciona **Send Channel Message** como Event y haz clic en **Continue**

    <figure><img src="../../.gitbook/assets/zapier/zap/21.png" alt="" width="563"><figcaption></figcaption></figure>
3.  Selecciona la cuenta de Discord en la que iniciaste sesión y haz clic en **Continue**

    <figure><img src="../../.gitbook/assets/zapier/zap/22.png" alt="" width="563"><figcaption></figcaption></figure>
4.  Selecciona tu Canal preferido para channel y selecciona **Text** y **String Source** (si está disponible) de FlowiseAI para Message Text, luego haz clic en **Continue**

    <figure><img src="../../.gitbook/assets/zapier/zap/23.png" alt="" width="563"><figcaption></figcaption></figure>
5.  Haz clic en **Test action**

    <figure><img src="../../.gitbook/assets/zapier/zap/24.png" alt=""><figcaption></figcaption></figure>
6.  ¡Listo! [🎉](https://emojipedia.org/party-popper/) Deberías ver el mensaje en tu Canal de Discord

    <figure><img src="../../.gitbook/assets/zapier/zap/25.png" alt=""><figcaption></figcaption></figure>
7.  Por último, renombra tu Zap y publícalo

    <figure><img src="../../.gitbook/assets/zapier/zap/26.png" alt=""><figcaption></figcaption></figure>
