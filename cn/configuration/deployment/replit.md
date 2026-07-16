---
description: 了解如何在 Replit 上部署 Flowise
---

# 重复

***

1. 登录[Replit](https://replit.com/~)
2. 创建一个新的 **Repl**。选择 **Node.js** 作为模板并填写您喜欢的 **标题**。

<figure><img src="../../.gitbook/assets/image (18) (1) (2) (1).png" alt="" width="551"><figcaption></figcaption></figure>

3. 创建新的 Repl 后，在左侧栏上单击 Secret：

<figure><img src="../../.gitbook/assets/image (2) (4) (1).png" alt="" width="219"><figcaption></figcaption></figure>

4. 创建 3 个秘密以跳过 Puppeteer 和 Playwright 库的 Chromium 下载。

<table><thead><tr><th width="403">秘密</th><th>价值</th></tr></thead><tbody><tr><td>PLAYWRIGHT_SKIP_BROWSER_DOWNLOAD</td><td>1</td></tr><tr><td>PUPPETEER_SKIP_DOWNLOAD</td><td>真实</td></tr><tr><td>PUPPETEER_SKIP_CHROMIUM_DOWNLOAD</td><td>真实</td></tr></tbody></table>

<figure><img src="../../.gitbook/assets/image (5) (3).png" alt="" width="535"><figcaption></figcaption></figure>

5. 现在可以切换到 Shell 选项卡

<figure><img src="../../.gitbook/assets/image (13) (2) (1).png" alt="" width="539"><figcaption></figcaption></figure>

6. 在 Shell 终端窗口中输入 `npm install -g flowise`。如果您遇到有关节点版本不兼容的错误，请使用以下命令 `yarn global add flowise --ignore-engines`

<figure><img src="../../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="530"><figcaption></figcaption></figure>

7. 然后是 `npx flowise start`

<figure><img src="../../.gitbook/assets/image (17) (1) (2).png" alt="" width="533"><figcaption></figcaption></figure>

8. 您现在应该能够在 Replit 上看到 Flowise！

<figure><img src="../../.gitbook/assets/image (15) (3).png" alt="" width="545"><figcaption></figcaption></figure>

9. 您现在将看到登录页面。只需使用您设置的用户名和密码登录即可。

<figure><img src="../../.gitbook/assets/image (12) (2) (1).png" alt=""><figcaption></figcaption></figure>
