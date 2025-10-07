# Mémoire zep

[Zep](https://github.com/getzep/zep)est un magasin de mémoire à long terme pour les applications LLM. Il stocke, résume, intègre, index et enrichit les histoires de l'application / chatbot LLM et les expose via des API simples à faible latence.

## Guide pour déployer ZEP à rendre

Vous pouvez facilement déployer ZEP sur des services cloud comme[Render](https://render.com/), [Flyio](https://fly.io/). Si vous préférez le tester localement, vous pouvez également tourner un conteneur Docker en suivant leur[quick guide](https://github.com/getzep/zep#quick-start).

Dans cet exemple, nous allons déployer pour rendre.

1. Se diriger vers[Zep Repo](https://github.com/getzep/zep#quick-start)et cliquez sur ** Déployer pour rendre **
2. Cela vous amènera à la page Blueprint de Render et cliquez simplement sur ** Créer de nouvelles ressources **

<gigne> <img src = "../../../. Gitbook / Assets / Image (21) (1) (1) (1) .png" alt = ""> <Figcaption> </ / Figcaption> </ Figure>

3. Lorsque le déploiement est terminé, vous devriez voir 3 applications créées sur votre tableau de bord

<gigne> <img src = "../../../. GitBook / Assets / Image (1) (2) .png" alt = ""> <figcaption> </gigcaption> </gigu

4. Cliquez simplement sur le premier appelé ** zep ** et copiez l'URL déployée

<gigne> <img src = "../../../. GitBook / Assets / Image (38) (1) .png" alt = ""> <figcaption> </figcaption> </gigust>

## Guide pour déployer ZEP vers Digital Ocean (via Docker)

1. Cloner le repo

```bash
git clone https://github.com/getzep/zep.git
cd zep
nano .env

```

2. Ajoutez votre clé API Openai dans.env

```bash
ZEP_OPENAI_API_KEY=

```

```bash
docker compose up -d --build
```

3. Autoriser l'accès au pare-feu au port 8000

```bash
sudo ufw allow from any to any port 8000 proto tcp
ufw status numbered
```

Si vous utilisez un pare-feu séparé de Digital Ocean du tableau de bord, assurez-vous que le port 8000 y est également ajouté

## Utiliser dans Flowise UI

1. Retour à l'application Flowise, créez simplement une nouvelle toile ou utilisez l'un des modèles de Marketplace. Dans cet exemple, nous allons utiliser ** une simple chaîne conversationnelle **

<gigne> <img src = "../../../. GitBook / Assets / Untitled (3) (1) .png" alt = ""> <Figcaption> </gigcaption> </gigust>

2. Remplacer ** Mémoire de tampon ** par ** Mémoire ZEP **. Remplacez ensuite l'URL de base ** ** par l'URL ZEP que vous avez copiée ci-dessus

<gigne> <img src = "../../../. GitBook / Assets / Untitled (5) .png" alt = ""> <Figcaption> </gigcaption> </ Figure>

3. Enregistrez le chatflow et testez-le pour voir si les conversations sont connues.

<gigne> <img src = "../../../. GitBook / Assets / Image (27) .png" alt = ""> <Figcaption> </gigcaption> </gigne>

4. Essayez maintenant d'effacer l'historique du chat, vous devriez voir qu'il est désormais incapable de vous souvenir des conversations précédentes.

<gigne> <img src = "../../../. Gitbook / Assets / Image (8) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) .png" alt = ""> <Figcaption> </figcaption> </ Figure>

## Authentification ZEP

ZEP vous permet de sécuriser votre instance à l'aide de l'authentification JWT. Nous utiliserons le`zepcli`utilitaire de ligne de commande[here](https://github.com/getzep/zepcli/releases).

#### 1. Générez un token secret et jwt <a href = "# id-1-generate-a-secret-and-the-jwt-token" id = "id-1-generate-a-secret-and-the-jwt-token"> </a>

Après avoir téléchargé le Zepcli:

Sur Linux ou macOS

```
./zepcli -i
```

Sous les fenêtres

```
zepcli.exe -i
```

Vous obtiendrez d'abord votre jeton secret:

<gigne> <img src = "../../../. Gitbook / Assets / image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1). Alt = ""> <Figcaption> </figcaption> </gigne>

Ensuite, vous obtiendrez un jeton JWT:

<Figure> <img src = "../../../. Gitbook / Assets / Image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1). Alt = ""> <Figcaption> </figcaption> </gigne>

#### 2. Configurer les variables de l'environnement AUTH <a href = "# id-2-configure-Auth-Environment-Variables" id = "id-2-Configure-Auth-Environment-Variables"> </a>

Définissez les variables d'environnement suivantes dans votre environnement de serveur ZEP:

```
ZEP_AUTH_REQUIRED=true
ZEP_AUTH_SECRET=<the secret you generated above>
```

#### 3. Configurer les informations d'identification sur Flowise <a href = "# id-2-configure-Auth-Environment-Variables" id = "id-2-Configure-Auth-Environment-Variables"> </a>

Ajoutez un nouvel diplôme pour ZEP et mettez le jeton JWT dans le champ Key API:

<gigne> <img src = "../../../. Gitbook / Assets / Image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) .png" Alt = "" width = "563"> <Figcaption> </gigcaption> </gigust>

#### 4. Utilisez les informations d'identification créées sur le nœud zep <a href = "# id-2-configure-Auth-Environment-Variables" id = "id-2-Configure-Auth-Environment-Variables"> </a>

Dans les informations d'identification ZEP Node Connect, sélectionnez les informations d'identification que vous venez de créer. Et c'est tout!

<gigne> <img src = "../../../. GitBook / Assets / Image (3) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).
