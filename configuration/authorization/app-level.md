---
description: Aprende cómo configurar el control de acceso a nivel de aplicación para tus instancias de Flowise
---

# App Level

***

La app level authorization protege tu instancia de Flowise mediante username y password. Esto protege tus apps de ser accesibles por cualquier persona cuando están deployed online.

<figure><img src="../../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

## How to Set Username & Password

### NPM

1. Install Flowise

```bash
npm install -g flowise
```

2. Start Flowise con username y password

```bash
npx flowise start --FLOWISE_USERNAME=user --FLOWISE_PASSWORD=1234
```

3. Open [http://localhost:3000](http://localhost:3000)

### Docker

1. Navigate a la carpeta `docker`

```
cd docker
```

2. Create un archivo `.env` y especifica el `PORT`, `FLOWISE_USERNAME`, y `FLOWISE_PASSWORD`

```sh
PORT=3000
FLOWISE_USERNAME=user
FLOWISE_PASSWORD=1234
```

3. Pass `FLOWISE_USERNAME` y `FLOWISE_PASSWORD` al archivo `docker-compose.yml`:

```
environment:
    - PORT=${PORT}
    - FLOWISE_USERNAME=${FLOWISE_USERNAME}
    - FLOWISE_PASSWORD=${FLOWISE_PASSWORD}
```

4. `docker compose up -d`
5. Open [http://localhost:3000](http://localhost:3000)
6. Puedes detener los containers usando `docker compose stop`

### Git clone

Para habilitar la app level authentication, add `FLOWISE_USERNAME` y `FLOWISE_PASSWORD` al archivo `.env` en `packages/server`:

```
FLOWISE_USERNAME=user
FLOWISE_PASSWORD=1234
```
