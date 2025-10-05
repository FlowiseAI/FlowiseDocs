---
description: Apprenez à déployer Flowise sur Hugging Face
---

# Hugging Face

***

### Créer un nouvel espace

1. Connectez-vous à [Hugging Face](https://huggingface.co/login)
2. Commencez à créer un [nouvel espace](https://huggingface.co/new-space) avec le nom de votre choix.
3. Sélectionnez **Docker** comme **SDK de l'espace** et choisissez **Vide** comme modèle Docker.
4. Sélectionnez **CPU de base ∙ 2 vCPU ∙ 16 Go ∙ GRATUIT** comme **matériel de l'espace**.
5. Cliquez sur **Créer l'espace**.

### Définir les variables d'environnement

1. Allez dans **Paramètres** de votre nouvel espace et trouvez la section **Variables et Secrets**
2. Cliquez sur **Nouvelle variable** et ajoutez le nom `PORT` avec la valeur `7860`
3. Cliquez sur **Sauvegarder**
4. _(Optionnel)_ Cliquez sur **Nouveau secret**
5. _(Optionnel)_ Remplissez avec vos variables d'environnement, telles que les identifiants de base de données, les chemins de fichiers, etc. Vous pouvez vérifier les champs valides dans le fichier `.env.example` [ici](https://github.com/FlowiseAI/Flowise/blob/main/docker/.env.example)

### Créer un Dockerfile

1. Dans l'onglet des fichiers, cliquez sur le bouton _**+ Ajouter un fichier**_ et cliquez sur **Créer un nouveau fichier** (ou téléchargez des fichiers si vous préférez)
2. Créez un fichier appelé **Dockerfile** et collez ce qui suit :

```Dockerfile
FROM node:18-alpine
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

3. Cliquez sur **Valider le fichier dans `main`** et cela commencera à construire votre application.

### Terminé 🎉

Lorsque la construction est terminée, vous pouvez cliquer sur l'onglet **Application** pour voir votre application en cours d'exécution.
