# Hugging Face

### Create a new space

1. Sign in to [Hugging Face](https://huggingface.co/login)
2. Start creating a [new Space](https://huggingface.co/new-space) with your preferred name.&#x20;
3. Select **Docker** as **Space SDK** and choose **Blank** as the Docker template.
4. Select **CPU basic ∙ 2 vCPU ∙ 16GB ∙ FREE** as **Space hardware**.
5. Click **Create Space**.

### Set the environment variables

1. Go to **Settings** of your new space and find the **Variables and Secrets** section
2. Click on **New variable** and add the name as `PORT` with value `7860`.
3. Save it and now click on **New secret**
4. Fill in with your environment variables, such as database credentials, file paths, etc. You can check for valid fields in the `.env.example` [here](https://github.com/FlowiseAI/Flowise/blob/main/docker/.env.example).

### Create a Dockerfile

1. At the files tab, click on button _**+ Add file**_ and click on **Create a new file** (or Upload files if you prefer to)
2. Create a file called **Dockerfile** and paste the following:

```dockerfile
FROM node:18-alpine

USER root

RUN apk add --no-cache git
RUN apk add --no-cache python3 py3-pip make g++
# needed for pdfjs-dist
RUN apk add --no-cache build-base cairo-dev pango-dev

# Install Chromium
RUN apk add --no-cache chromium

ENV PUPPETEER_SKIP_DOWNLOAD=true
ENV PUPPETEER_EXECUTABLE_PATH=/usr/bin/chromium-browser

# You can install a specific version like: flowise@1.0.0
RUN npm install -g flowise

RUN mkdir -p /usr/local/lib/node_modules/flowise/uploads     && chmod -R 777 /usr/local/lib/node_modules/flowise/uploads
RUN mkdir -p /usr/local/lib/node_modules/flowise/logs        && chmod -R 777 /usr/local/lib/node_modules/flowise/logs
RUN touch /usr/local/lib/node_modules/flowise/api.json       && chmod 777 /usr/local/lib/node_modules/flowise/api.json
RUN touch /usr/local/lib/node_modules/flowise/encryption.key && chmod 777 /usr/local/lib/node_modules/flowise/encryption.key

WORKDIR /data

CMD ["npx", "flowise", "start"]
```

3. Click on **Commit file to `main`** and it will start to build your app

### Done 🎉

When the build finishes you can click on the **App** tab to see your app running.
