---
description: >-
  Upsert embedded data and perform similarity search upon query using pgvector
  on Postgres.
---

# Postgres

<gigne> <img src = "../../../. GitBook / Assets / Image (163) .png" alt = "" width = "292"> <figcaption> <p> Node postgres </p> </gigcaption> </ figure>

Il existe plusieurs méthodes pour se connecter à Postgres en fonction de la configuration de votre instance. Vous trouverez ci-dessous un exemple de configuration locale à l'aide d'une image Docker prédéfinie fournie par l'équipe PGVector.

Créer un fichier nommé`docker-compose.yml`avec le contenu ci-dessous:

```yaml
# Run this command to start the database:
# docker-compose up --build
version: "3"
services:
  db:
    hostname: 127.0.0.1
    image: pgvector/pgvector:pg16
    ports:
      - 5432:5432
    restart: always
    environment:
      - POSTGRES_DB=api
      - POSTGRES_USER=myuser
      - POSTGRES_PASSWORD=ChangeMe
    volumes:
      - ./init.sql:/docker-entrypoint-initdb.d/init.sql
```

`docker compose up`Pour démarrer le conteneur Postgres.

Créez de nouvelles informations d'identification avec l'utilisateur et le mot de passe configurés:

<gigne> <img src = "../../../. GitBook / Assets / Image (50) .png" alt = "" width = "526"> <Figcaption> </gigcaption> </gigust>

Remplissez le champ du nœud avec des valeurs configurées dans`docker-compose.yml`. Par exemple:

* Hôte: ** LocalHost **
* Base de données: ** API **
* Port: ** 5432 **

Le tour est joué! Vous avez désormais réussi à configurer le vecteur postgres prêt à être utilisé.
