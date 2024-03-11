# Hugging Face

### Create a new space

1. Sign in to [Hugging Face](https://huggingface.co/login)
2. Start creating a [new Space](https://huggingface.co/new-space) with your preferred name.
3. Select **Docker** as **Space SDK** and choose **Blank** as the Docker template.
4. Select **CPU basic ∙ 2 vCPU ∙ 16GB ∙ FREE** as **Space hardware**.
5. Click **Create Space**.

### Set the environment variables

1. Go to **Settings** of your new space and find the **Variables and Secrets** section
2. Click on **New variable** and add the name as `PORT` with value `7860`
3. Click on **Save**
4. _(Optional)_ Click on **New secret**
5. _(Optional)_ Fill in with your environment variables, such as database credentials, file paths, etc. You can check for valid fields in the `.env.example` [here](https://github.com/FlowiseAI/Flowise/blob/main/docker/.env.example)

### Create a Dockerfile

1. At the files tab, click on button _**+ Add file**_ and click on **Create a new file** (or Upload files if you prefer to)
2. Create a file called **Dockerfile** and paste the following:

```Dockerfile
FROM node:18-alpine
# Create the /data directory with appropriate permissions
RUN mkdir -p /data && chmod -R 777 /data
USER root

# Arguments that can be passed at build time
ARG FLOWISE_PATH=/usr/local/lib/node_modules/flowise
ARG BASE_PATH=/data/.flowise
ARG DATABASE_PATH=$BASE_PATH
ARG APIKEY_PATH=$BASE_PATH
ARG SECRETKEY_PATH=$BASE_PATH
ARG LOG_PATH=$BASE_PATH/logs

# Set the ARGs as ENV
ENV FLOWISE_PATH=$FLOWISE_PATH
ENV BASE_PATH=$BASE_PATH
ENV DATABASE_PATH=$DATABASE_PATH
ENV APIKEY_PATH=$APIKEY_PATH
ENV SECRETKEY_PATH=$SECRETKEY_PATH
ENV LOG_PATH=$LOG_PATH

# Install dependencies
RUN apk add --no-cache git python3 py3-pip make g++ build-base cairo-dev pango-dev chromium

ENV PUPPETEER_SKIP_DOWNLOAD=true
ENV PUPPETEER_EXECUTABLE_PATH=/usr/bin/chromium-browser

# Install Flowise globally
RUN npm install -g flowise

# # Configure Flowise directories using the ARG
RUN mkdir -p $FLOWISE_PATH/uploads && chmod -R 777 $FLOWISE_PATH
CMD ["sh", "-c", "mkdir -p $LOG_PATH && chmod -R 777 $LOG_PATH && npx flowise start"]
```

3. Click on **Commit file to `main`** and it will start to build your app.

### Done 🎉

When the build finishes you can click on the **App** tab to see your app running.
