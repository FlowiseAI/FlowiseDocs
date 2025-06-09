---
description: Aprende cómo hacer deployment de Flowise en Replit
---

# Replit

***

1. Inicia sesión en [Replit](https://replit.com/~)
2. Crea un nuevo **Repl**. Selecciona **Node.js** como Template y completa con tu **Title** preferido.

<figure><img src="../../.gitbook/assets/image (18) (1) (2) (1).png" alt="" width="551"><figcaption></figcaption></figure>

3. Después de que se cree un nuevo Repl, en la barra lateral izquierda, haz click en Secret:

<figure><img src="../../.gitbook/assets/image (2) (4) (1).png" alt="" width="219"><figcaption></figcaption></figure>

4. Crea 3 Secrets para omitir la descarga de Chromium para las librerías Puppeteer y Playwright.

<table><thead><tr><th width="403">Secrets</th><th>Value</th></tr></thead><tbody><tr><td>PLAYWRIGHT_SKIP_BROWSER_DOWNLOAD</td><td>1</td></tr><tr><td>PUPPETEER_SKIP_DOWNLOAD</td><td>true</td></tr><tr><td>PUPPETEER_SKIP_CHROMIUM_DOWNLOAD</td><td>true</td></tr></tbody></table>

<figure><img src="../../.gitbook/assets/image (5) (3).png" alt="" width="535"><figcaption></figcaption></figure>

5. Ahora puedes cambiar a la pestaña Shell

<figure><img src="../../.gitbook/assets/image (13) (2) (1).png" alt="" width="539"><figcaption></figcaption></figure>

6. Escribe `npm install -g flowise` en la ventana de terminal Shell. Si tienes un error sobre una versión de node incompatible, usa el siguiente comando `yarn global add flowise --ignore-engines`

<figure><img src="../../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="530"><figcaption></figcaption></figure>

7. Luego sigue con `npx flowise start`

<figure><img src="../../.gitbook/assets/image (17) (1) (2).png" alt="" width="533"><figcaption></figcaption></figure>

8. ¡Ahora deberías poder ver Flowise en Replit!

<figure><img src="../../.gitbook/assets/image (15) (3).png" alt="" width="545"><figcaption></figcaption></figure>

9. Ahora verás una página de inicio de sesión. Simplemente inicia sesión con el nombre de usuario y contraseña que has configurado.

<figure><img src="../../.gitbook/assets/image (12) (2) (1).png" alt=""><figcaption></figcaption></figure>
