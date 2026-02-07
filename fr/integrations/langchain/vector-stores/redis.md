# Redis

## Condition préalable

1. Tourner un[Redis-Stack Server](https://redis.io/docs/latest/operate/oss_and_stack/install/install-stack/docker/)Utilisation de Docker

```bash
docker run -d --name redis-stack-server -p 6379:6379 redis/redis-stack-server:latest
```

## Installation

1. Ajoutez un nouveau nœud ** redis ** sur toile.
2. Créez de nouveaux informations d'identification Redis.

<gigne> <img src = "../../../. Gitbook / Assets / Image (1) (1) (3) (1) (1) .png" alt = "" width = "257"> <figCaption> </ Figcaption> </ Figure>

3. Sélectionnez le type d'identification Redis. Choisissez l'API Redis si vous avez un nom d'utilisateur et un mot de passe, sinon redis URL:

<gigne> <img src = "../../../. GitBook / Assets / Image (2) (1) (1) (2) .png" alt = "" width = "563"> <figCaption> </ Figcaption> </ Figure>

4. Remplissez l'URL:

<gigne> <img src = "../../../. GitBook / Assets / Image (3) (1) (1) (1) (2) (1) .png" alt = "" width = "542"> <Figcaption> </ FigCaption> </ Figure>

5. Vous pouvez maintenant démarrer des données avec Redis:

<gigne> <img src = "../../../. GitBook / Assets / Image (8) (1) (1) (1) (1) (1) (1) (1) (1) (1) .png" alt = ""> <figCaption> </gigcaption> </pigu

<gigne> <img src = "../../../. Gitbook / Assets / Image (9) (2) (1) .png" alt = ""> <Figcaption> </ Figcaption> </gigust>

6. Accédez au portail Redis Insight et à votre base de données, vous pourrez voir toutes les données qui ont été renversées:

<gigne> <img src = "../../../. GitBook / Assets / Image (138) .png" alt = ""> <Figcaption> </gigcaption> </gigne>
