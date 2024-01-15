# Getting Started

## Prerequisite

Latest [NodeJS](https://nodejs.org/en/download) installed

## ⚡Quick Start

1. Install Flowise

```bash
npm install -g flowise
```

2. Start Flowise

<pre class="language-bash"><code class="lang-bash"><strong>npx flowise start
</strong></code></pre>

3. Open [http://localhost:3000](http://localhost:3000)

## 🐳 Docker

### Docker Compose

1. Go to `docker` folder at the root of the project
2. Copy the `.env.example` file and paste it as another file named `.env`
3. `docker-compose up -d`
4. Open [http://localhost:3000](http://localhost:3000)
5. You can bring the containers down by `docker-compose stop`

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

## 👨‍💻 Developers

Flowise has 3 different modules in a single mono repository.

* `server`: Node backend to serve API logics
* `ui`: React frontend
* `components`: Integrations components

#### Prerequisite

Install Yarn

```bash
npm i -g yarn
```

#### Setup

1. Clone the repository

```bash
git clone https://github.com/FlowiseAI/Flowise.git
```

2. Go into repository folder

<pre class="language-bash"><code class="lang-bash"><strong>cd Flowise
</strong></code></pre>

3. Install all dependencies of all modules:

```bash
yarn install
```

4. Build all the code:

```bash
yarn build
```

5. Start the app:

```bash
yarn start
```

You can now access the app on [http://localhost:3000](http://localhost:3000)

6. For development build:

* Create `.env` file and specify the `PORT` (refer to `.env.example`) in `packages/ui`
* Create `.env` file and specify the `PORT` (refer to `.env.example`) in `packages/server`

```bash
yarn dev
```

Any code changes will reload the app automatically on [http://localhost:8080](http://localhost:8080)

Watch an introduction & setup tutorial on Flowise, shoutout to Leon!

{% embed url="https://youtu.be/tD6fwQyUIJE" %}

