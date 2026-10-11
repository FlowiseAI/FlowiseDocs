---
description: 了解如何在 Hugging Face 上部署 Flowise
---

# 拥抱脸

***

### 创建一个新空间

1. 登录[抱脸](https://huggingface.co/login)
2. 开始使用您喜欢的名称创建[新空间](https://huggingface.co/new-space)。
3. 选择 **Docker** 作为 **Space SDK** 并选择 **Blank** 作为 Docker 模板。
4. 选择 **CPU basic ∙ 2 vCPU ∙ 16GB ∙ FREE** 作为 **Space 硬件**。
5. 单击“**创建空间**”。

### 设置环境变量

1. 转到新空间的 **设置** 并找到 **变量和秘密** 部分
2. 单击 **新变量** 并将名称添加为 `PORT`，值为 `7860`
3. 单击“**保存**”
4._（可选）_单击**新秘密**
5._（可选）_填写您的环境变量，例如数据库凭据、文件路径等。您可以检查 `.env.example` [此处](https://github.com/FlowiseAI/Flowise/blob/main/docker/.env.example) 中的有效字段

### 创建一个 Dockerfile

1. 在文件选项卡中，单击按钮 _**+ 添加文件**_ 并单击 **创建新文件**（如果您愿意，也可以上传文件）
2. 创建一个名为 **Dockerfile** 的文件并粘贴以下内容：

```Dockerfile
FROM node:20-alpine
USER root

# Arguments that can be passed at build time
ARG FLOWISE_PATH=/usr/local/lib/node_modules/flowise
ARG BASE_PATH=/root/.flowise
ARG DATABASE_PATH=$BASE_PATH
ARG SECRETKEY_PATH=$BASE_PATH
ARG LOG_PATH=$BASE_PATH/logs
ARG BLOB_STORAGE_PATH=$BASE_PATH/storage

# Install dependencies
RUN apk add --no-cache git python3 py3-pip make g++ build-base cairo-dev pango-dev chromium

ENV PUPPETEER_SKIP_DOWNLOAD=true
ENV PUPPETEER_EXECUTABLE_PATH=/usr/bin/chromium-browser

# Install Flowise globally
RUN npm install -g flowise

# Configure Flowise directories using the ARG
RUN mkdir -p $LOG_PATH $FLOWISE_PATH/uploads && chmod -R 777 $LOG_PATH $FLOWISE_PATH

WORKDIR /data

CMD ["npx", "flowise", "start"]
```

3. 单击 **Commit file to `main`**，它将开始构建您的应用程序。

### 完成🎉

构建完成后，您可以单击 **App** 选项卡来查看您的应用程序正在运行。
