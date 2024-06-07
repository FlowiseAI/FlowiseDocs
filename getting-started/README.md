---
description: Learn how to install Flowise locally
---

# Getting Started

{% hint style="info" %}
**Important:** Before you can get started, you'll need to ensure that you have the latest version of  [NodeJS](https://nodejs.org/en/download) installed on your computer. NodeJS is a fundamental requirement for this project, providing the runtime environment for our code.
{% endhint %}

## Quick Start

Install Flowise locally using npm.

1. Install Flowise:

```bash
npm install -g flowise
```

2. Start Flowise:

```bash
npx flowise start
```

3. Open: [http://localhost:3000](http://localhost:3000)

## Docker

There are two ways to install Flowise in Docker: using Docker Compose or a Docker image.

### Docker Compose

1. Go to `docker folder` at the root of the project
2. Copy the `.env.example` file and paste it as another file named `.env`
3. Run:

```bash
docker-compose up -d
```

4. Open: [http://localhost:3000](http://localhost:3000)
5. You can bring the containers down by running:

```bash
docker-compose stop
```

### Docker Image

1. Build the image locally:

```bash
docker build --no-cache -t flowise .
```

2. Run image:

```bash
docker run -d --name flowise -p 3000:3000 flowise
```

3. Stop image:

```bash
docker stop flowise
```

## For Developers

Flowise has 3 different modules in a single mono repository:

* **Server**: Node backend to serve API logics
* **UI**: React frontend
* **Components**: Integrations components

### Prerequisite

Install [PNPM](https://pnpm.io/installation)

```bash
npm i -g pnpm
```

### Setup

For a simple set up:

1. Clone the repository

```bash
git clone https://github.com/FlowiseAI/Flowise.git
```

2. Go into repository folder

```bash
cd Flowise
```

3. Install all dependencies of all modules:

```bash
pnpm install
```

4. Build all the code:

```bash
pnpm build
```

Start the app on [http://localhost:3000](http://localhost:3000)

```bash
pnpm start
```

### **Step by step**

For contrubutting to the project:

1. Fork the official [Flowise Github Repository](https://github.com/FlowiseAI/Flowise)
2. Clone your forked repository
3. Create a new branch, see [guide](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-and-deleting-branches-within-your-repository). Naming conventions:
   * For feature branch: `feature/<Your New Feature>`
   * For bug fix branch: `bugfix/<Your New Bugfix>`.
4. Switch to the newly created branch
5. Go into repository folder:&#x20;

```bash
cd Flowise
```

6. Install all dependencies of all modules:

```bash
pnpm install
```

7. Build all the code:

```bash
pnpm build
```

8. Start the app:

```bash
pnpm start
```

You can now access the app on [http://localhost:3000](http://localhost:3000)

9. For development build:

* Create `.env` file and specify the `PORT` (refer to `.env.example`) in `packages/ui`
* Create `.env` file and specify the `PORT` (refer to `.env.example`) in `packages/server`

```bash
pnpm dev
```

#### **Important:**

* Any changes made in `packages/ui` or `packages/server` will be reflected on [http://localhost:8080](http://localhost:8080/)
* For changes made in `packages/components`, you will need to build again to pickup the changes
*   After making all the changes, run:

    ```bash
    pnpm build
    ```

    and

    ```bash
    pnpm start
    ```

    to make sure everything works fine in production.

## Learn More

In this video tutorial, Leon provides a first introduction to Flowise and explains some ways to set up Flowise on your local machine.

{% embed url="https://youtu.be/nqAK_L66sIQ" %}
