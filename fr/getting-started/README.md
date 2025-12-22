# Commencer

***

## Cloud

L'auto-hébergement nécessite plus de compétences techniques pour configurer l'instance, sauvegarder la base de données et maintenir les mises à jour. Si vous n'êtes pas expérimenté dans la gestion des serveurs et que vous souhaitez simplement utiliser l'application web, nous vous recommandons d'utiliser [Flowise Cloud](https://cloud.flowiseai.com).

## Démarrage rapide

{% hint style="info" %}
Prérequis : assurez-vous que [NodeJS](https://nodejs.org/en/download) est installé sur la machine. Node `v18.15.0` ou `v20` et supérieur est pris en charge.
{% endhint %}

Installez Flowise localement en utilisant NPM.

1. Installez Flowise:

```bash
npm install -g flowise
```

Vous pouvez également installer une version spécifique. Consultez les [versions](https://www.npmjs.com/package/flowise?activeTab=versions) disponibles.

```
npm install -g flowise@x.x.x
```

2. Démarrer Flowise :

```bash
npx flowise start
```

3. Ouvrez : [http://localhost:3000](http://localhost:3000)

***

## Docker

Il existe deux façons de déployer Flowise avec Docker. Tout d'abord, clonez le projet : [https://github.com/FlowiseAI/Flowise](https://github.com/FlowiseAI/Flowise)

### Docker Compose

1. Allez dans le dossier `docker` à la racine du projet
2. Copiez le fichier `.env.example` et collez-le sous un autre nom de fichier `.env`
3. Exécutez :

```bash
docker compose up -d
```

4. Ouvrez : [http://localhost:3000](http://localhost:3000)  
5. Vous pouvez arrêter les conteneurs en exécutant :

```bash
docker compose stop
```

### Image Docker

1. Construisez l'image :

```bash
docker build --no-cache -t flowise .
```

2. Exécuter l'image :

```bash
docker run -d --name flowise -p 3000:3000 flowise
```

3. Arrêter l'image :

```bash
docker stop flowise
```

***

## Pour les développeurs

Flowise dispose de 4 modules différents dans un seul dépôt mono :

* **Serveur** : backend Node pour gérer la logique de l'API
* **UI** : frontend React
* **Composants** : composants d'intégration
* **Documentation de l'API** : spécification Swagger pour les APIs Flowise

### Prérequis

Installez [PNPM](https://pnpm.io/installation).

```bash
npm i -g pnpm
```

### Configuration 1

Configuration simple utilisant PNPM :

1. Clonez le dépôt

```bash
git clone https://github.com/FlowiseAI/Flowise.git
```

2. Accédez au dossier du dépôt

```bash
cd Flowise
```

3. Installez toutes les dépendances de tous les modules :

```bash
pnpm install
```

4. Construire le code :

```bash
pnpm build
```

Démarrez l'application à [http://localhost:3000](http://localhost:3000)

```bash
pnpm start
```

### Configuration 2

Instructions étape par étape pour les contributeurs du projet :

1. Forkez le [dépôt Github officiel de Flowise](https://github.com/FlowiseAI/Flowise)
2. Clonez votre dépôt forké
3. Créez une nouvelle branche, consultez le [guide](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-and-deleting-branches-within-your-repository). Conventions de nommage :
   * Pour la branche de fonctionnalité : `feature/<Votre Nouvelle Fonctionnalité>`
   * Pour la branche de correction de bogue : `bugfix/<Votre Nouvelle Correction>`.
4. Passez à la branche que vous venez de créer
5. Accédez au dossier du dépôt :

```bash
cd Flowise
```

6. Installez toutes les dépendances de tous les modules :

```bash
pnpm install
```

7. Construire le code :

```bash
pnpm build
```

8. Démarrez l'application à [http://localhost:3000](http://localhost:3000)

```bash
pnpm start
```

9. Pour la construction de développement :

* Créez un fichier `.env` et spécifiez le `PORT` (voir `.env.example`) dans `packages/ui`
* Créez un fichier `.env` et spécifiez le `PORT` (voir `.env.example`) dans `packages/server`

```bash
pnpm dev
```

* Toute modification apportée dans `packages/ui` ou `packages/server` sera reflétée sur [http://localhost:8080](http://localhost:8080/)
* Pour les modifications apportées dans `packages/components`, vous devrez reconstruire pour prendre en compte les changements
*   Après avoir effectué toutes les modifications, exécutez :

    ```bash
    pnpm build
    ```

    et

    ```bash
    pnpm start
    ```

    pour vous assurer que tout fonctionne correctement en production.

***

## Pour les entreprises

Avant de démarrer l'application, les utilisateurs d'entreprise doivent remplir les valeurs des paramètres d'entreprise dans le fichier `.env`. Consultez `.env.example` pour les modifications requises.

Contactez support@flowiseai.com pour la valeur des variables d'environnement suivantes :

```
LICENSE_URL
FLOWISE_EE_LICENSE_KEY
```

***

## En savoir plus

Dans ce tutoriel vidéo, Leon présente une introduction à Flowise et explique comment l'installer sur votre machine locale.

{% embed url="https://youtu.be/nqAK_L66sIQ" %}

## Guide de la communauté

* [Introduction à la construction d'applications LLM avec Flowise / LangChain \[Pratique\]](https://volcano-ice-cd6.notion.site/Introduction-to-Practical-Building-LLM-Applications-with-Flowise-LangChain-03d6d75bfd20495d96dfdae964bea5a5)
* [Introduction à la construction d'applications LLM avec Flowise / LangChain \[Pratique\]](https://volcano-ice-cd6.notion.site/Flowise-LangChain-LLM-e106bb0f7e2241379aad8fa428ee064a)
