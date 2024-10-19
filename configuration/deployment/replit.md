---
description: Learn how to deploy Flowise on Replit
---

# Replit

***

1. Sign in to [Replit](https://replit.com/\~)
2. Create a new **Repl**. Select **Node.js** as Template and fill in your preferred **Title**.

<figure><img src="../../.gitbook/assets/image (18) (1) (2) (1).png" alt="" width="551"><figcaption></figcaption></figure>

3. After a new Repl is created, on the left hand side bar, click Secret:

<figure><img src="../../.gitbook/assets/image (2) (4) (1).png" alt="" width="219"><figcaption></figcaption></figure>

4. Create 3 Secrets to skip Chromium download for Puppeteer and Playwright libraries.

<table><thead><tr><th width="403">Secrets</th><th>Value</th></tr></thead><tbody><tr><td>PLAYWRIGHT_SKIP_BROWSER_DOWNLOAD</td><td>1</td></tr><tr><td>PUPPETEER_SKIP_DOWNLOAD</td><td>true</td></tr><tr><td>PUPPETEER_SKIP_CHROMIUM_DOWNLOAD</td><td>true</td></tr></tbody></table>

<figure><img src="../../.gitbook/assets/image (5) (3).png" alt="" width="535"><figcaption></figcaption></figure>

5. You can now switch to Shell tab

<figure><img src="../../.gitbook/assets/image (13) (2) (1).png" alt="" width="539"><figcaption></figcaption></figure>

6. Type in `npm install -g flowise` into the Shell terminal window. If you are having error about incompatible node version, use the following command `yarn global add flowise --ignore-engines`

<figure><img src="../../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="530"><figcaption></figcaption></figure>

7. Then followed by `npx flowise start`

<figure><img src="../../.gitbook/assets/image (17) (1) (2).png" alt="" width="533"><figcaption></figcaption></figure>

8. You should now be able to see Flowise on Replit!

<figure><img src="../../.gitbook/assets/image (15) (3).png" alt="" width="545"><figcaption></figcaption></figure>

9. If you would like to turn on [app level authorization](broken-reference/), change the command to:

```bash
npx flowise start --FLOWISE_USERNAME=user --FLOWISE_PASSWORD=1234
```

10. You will now see a login page. Simply login with the username and password you've set.

<figure><img src="../../.gitbook/assets/image (12) (2) (1).png" alt=""><figcaption></figcaption></figure>
