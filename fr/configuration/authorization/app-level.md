---
description: Apprenez à configurer le contrôle d'accès au niveau de l'application pour vos instances Flowise
---

# Application

***

## Email & Mot de passe

À partir de la version v3.0.1, une nouvelle méthode d'authentification a été introduite. Flowise utilise un [**système d'authentification basé sur Passport.js**](https://www.passportjs.org/) avec des tokens JWT stockés dans des cookies sécurisés HTTP-only. Lorsqu'un utilisateur se connecte, le système valide son email/mot de passe contre la base de données en utilisant une comparaison de hachage bcrypt, puis génère deux tokens JWT : un token d'accès à durée limitée (par défaut 60 minutes) et un token de rafraîchissement à longue durée (par défaut 90 jours). Ces tokens sont stockés en tant que cookies sécurisés. Pour les requêtes suivantes, le système extrait le JWT des cookies, valide la signature et les revendications en utilisant la stratégie JWT de Passport, et vérifie que la session utilisateur existe toujours. Le système prend également en charge le rafraîchissement automatique des tokens lorsque le token d'accès expire, et maintient les sessions en utilisant soit Redis, soit le stockage de base de données selon la configuration.

Pour les utilisateurs existants qui utilisaient [Nom d'utilisateur & Mot de passe (Obsolète)](app-level.md#username-and-password-deprecated), vous devez configurer un nouveau compte administrateur. Pour éviter les revendications de propriété non autorisées, vous devez d'abord vous authentifier en utilisant le nom d'utilisateur et le mot de passe existants configurés comme `FLOWISE_USERNAME` et `FLOWISE_PASSWORD`.

<figure><img src="../../.gitbook/assets/image (18) (1) (1).png" alt="" width="387"><figcaption></figcaption></figure>

Les variables d'environnement suivantes peuvent être modifiées :

### URL de l'application

* `APP_URL` - L'URL de votre application Flowise hébergée. Par défaut `http://localhost:3000`

### Configuration des variables d'environnement JWT

Pour configurer les paramètres d'authentification JWT de Flowise, l'utilisateur peut modifier les variables d'environnement suivantes :

* `JWT_AUTH_TOKEN_SECRET` - La clé secrète pour signer les tokens d'accès
* `JWT_REFRESH_TOKEN_SECRET` - Secret pour les tokens de rafraîchissement (par défaut, utilise le secret du token d'authentification s'il n'est pas défini)
* `JWT_TOKEN_EXPIRY_IN_MINUTES` - Durée de vie du token d'accès (par défaut : 60 minutes)
* `JWT_REFRESH_TOKEN_EXPIRY_IN_MINUTES` - Durée de vie du token de rafraîchissement (par défaut : 129 600 minutes ou 90 jours)
* `JWT_AUDIENCE` - Revendication d'audience pour la validation du token (par défaut : 'AUDIENCE')
* `JWT_ISSUER` - Revendication d'émetteur pour la validation du token (par défaut : 'ISSUER')
* `EXPRESS_SESSION_SECRET` - Secret de chiffrement de session (par défaut : 'flowise')
* `EXPIRE_AUTH_TOKENS_ON_RESTART` - Défini sur 'true' pour invalider tous les tokens au redémarrage du serveur (utile pour le développement)

### Configuration SMTP pour les emails

Configurez ces variables pour activer la fonctionnalité d'email pour les réinitialisations de mot de passe et les notifications :

* `SMTP_HOST` - Le nom d'hôte de votre serveur SMTP (par exemple, `smtp.gmail.com`, `smtp.host.com`)
* `SMTP_PORT` - Le numéro de port pour la connexion SMTP (valeurs courantes : `587` pour TLS, `465` pour SSL, `25` pour non chiffré)
* `SMTP_USER` - Nom d'utilisateur pour l'authentification SMTP (généralement votre adresse email)
* `SMTP_PASSWORD` - Mot de passe ou mot de passe spécifique à l'application pour l'authentification SMTP
* `SMTP_SECURE` - Défini sur `true` pour le chiffrement SSL/TLS, `false` pour les connexions non chiffrées
* `ALLOW_UNAUTHORIZED_CERTS` - Défini sur `true` pour autoriser les certificats auto-signés (non recommandé pour la production)
* `SENDER_EMAIL` - L'adresse email "de" qui apparaîtra sur les emails sortants

### Configuration de la sécurité et des tokens

Ces variables contrôlent la sécurité de l'authentification, l'expiration des tokens et le hachage des mots de passe :

* `PASSWORD_RESET_TOKEN_EXPIRY_IN_MINS` - Temps d'expiration pour les tokens de réinitialisation de mot de passe (par défaut : 15 minutes)
* `PASSWORD_SALT_HASH_ROUNDS` - Nombre de tours de sel bcrypt pour le hachage des mots de passe (par défaut : 10, plus élevé = plus sécurisé mais plus lent)
* `TOKEN_HASH_SECRET` - Clé secrète utilisée pour hacher les tokens et les données sensibles (utilisez une chaîne forte et aléatoire)

### Meilleures pratiques de sécurité

* Utilisez des valeurs fortes et uniques pour `TOKEN_HASH_SECRET` et stockez-les en toute sécurité
* Pour la production, utilisez `SMTP_SECURE=true` et `ALLOW_UNAUTHORIZED_CERTS=false`
* Définissez des temps d'expiration de token appropriés en fonction de vos exigences de sécurité
* Utilisez des valeurs plus élevées pour `PASSWORD_SALT_HASH_ROUNDS` (12-15) pour une meilleure sécurité en production

## Nom d'utilisateur et mot de passe (Obsolète)

L'autorisation au niveau de l'application protège votre instance Flowise par un nom d'utilisateur et un mot de passe. Cela empêche vos applications d'être accessibles par quiconque lorsqu'elles sont déployées en ligne.

<figure><img src="../../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

### Comment définir un nom d'utilisateur et un mot de passe

#### Npm

1. Installez Flowise

```bash
npm install -g flowise
```

2. Démarrer Flowise avec nom d'utilisateur et mot de passe

```bash
npx flowise start --FLOWISE_USERNAME=user --FLOWISE_PASSWORD=1234
```

3. Ouvrez [http://localhost:3000](http://localhost:3000)

#### Docker

1. Accédez au dossier `docker`

```
cd docker
```

2. Créez un fichier `.env` et spécifiez le `PORT`, le `FLOWISE_USERNAME` et le `FLOWISE_PASSWORD`

```sh
PORT=3000
FLOWISE_USERNAME=user
FLOWISE_PASSWORD=1234
```

3. Passez `FLOWISE_USERNAME` et `FLOWISE_PASSWORD` au fichier `docker-compose.yml` :

```
environment:
    - PORT=${PORT}
    - FLOWISE_USERNAME=${FLOWISE_USERNAME}
    - FLOWISE_PASSWORD=${FLOWISE_PASSWORD}
```

4. `docker compose up -d`  
5. Ouvrez [http://localhost:3000](http://localhost:3000)  
6. Vous pouvez arrêter les conteneurs avec `docker compose stop`  

#### Clonage Git

Pour activer l'authentification au niveau de l'application, ajoutez `FLOWISE_USERNAME` et `FLOWISE_PASSWORD` au fichier `.env` dans `packages/server` :

```
FLOWISE_USERNAME=user
FLOWISE_PASSWORD=1234
```
