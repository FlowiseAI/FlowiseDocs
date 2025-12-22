# SSO

{% hint style="info" %}
Le SSO est uniquement disponible pour le plan Entreprise
{% endhint %}

Flowise prend en charge [OIDC](https://openid.net/) qui permet aux utilisateurs d'utiliser _l'authentification unique_ (_SSO_) pour accéder à l'application. Actuellement, seul l'[Administrateur de l'organisation](../using-flowise/workspaces.md#setting-up-admin-account) peut configurer les paramètres SSO.

## Microsoft

1. Dans le portail Azure, recherchez Microsoft Entra ID :

<figure><img src="../.gitbook/assets/image (193).png" alt=""><figcaption></figcaption></figure>

2. Dans la barre latérale gauche, cliquez sur Enregistrements d'applications, puis sur Nouvelle inscription :

<figure><img src="../.gitbook/assets/image (194).png" alt=""><figcaption></figcaption></figure>

3. Entrez un nom d'application et sélectionnez Locataire unique :

<figure><img src="../.gitbook/assets/image (195).png" alt=""><figcaption></figcaption></figure>

4. Après la création de l'application, notez l'ID d'application (client) et l'ID de répertoire (locataire) :

<figure><img src="../.gitbook/assets/image (196).png" alt=""><figcaption></figcaption></figure>

5. Dans la barre latérale gauche, cliquez sur Certificats et secrets -> Nouveau secret client -> Ajouter :

<figure><img src="../.gitbook/assets/image (198).png" alt=""><figcaption></figcaption></figure>

6. Après la création du secret, copiez la Valeur, <mark style="color:red;">pas</mark> l'ID de secret :

<figure><img src="../.gitbook/assets/image (199).png" alt=""><figcaption></figcaption></figure>

7. Dans la barre latérale gauche, cliquez sur Authentification -> Ajouter une plateforme -> Web :

<figure><img src="../.gitbook/assets/image (201).png" alt=""><figcaption></figcaption></figure>

8. Remplissez les URI de redirection. Cela devra être modifié en fonction de la façon dont vous l'hébergez : `http[s]://[votre-instance-flowise.com]/api/v1/azure/callback` :

<figure><img src="../.gitbook/assets/image (218).png" alt="" width="514"><figcaption></figcaption></figure>

9. Vous devriez voir la nouvelle URI de redirection créée :

<figure><img src="../.gitbook/assets/image (219).png" alt=""><figcaption></figcaption></figure>

10. Retournez à l'application Flowise, connectez-vous en tant qu'Administrateur de l'organisation. Accédez à la configuration SSO depuis la barre latérale gauche. Remplissez l'ID de locataire Azure et l'ID client de l'étape 4, et le secret client de l'étape 6. Cliquez sur Tester la configuration pour voir si la connexion peut être établie avec succès :

<figure><img src="../.gitbook/assets/image (220).png" alt="" width="563"><figcaption></figcaption></figure>

11. Enfin, activez et enregistrez :

<figure><img src="../.gitbook/assets/image (221).png" alt="" width="563"><figcaption></figcaption></figure>

12. Avant que les utilisateurs puissent se connecter en utilisant le SSO, ils doivent d'abord être invités. Consultez [Inviter des utilisateurs pour la connexion SSO](sso.md#inviting-users-for-sso-sign-in) pour un guide étape par étape. Les utilisateurs invités doivent également faire partie des utilisateurs du répertoire dans Azure.

<figure><img src="../.gitbook/assets/image (2) (1) (2).png" alt=""><figcaption></figcaption></figure>

## Google

Pour activer la connexion avec Google sur votre site web, vous devez d'abord configurer votre ID client API Google. Pour ce faire, suivez les étapes suivantes :

1. Ouvrez la page **Identifiants** de la [console API Google](https://console.developers.google.com/apis).
2. Cliquez sur **Créer des identifiants** > **ID client OAuth**

<figure><img src="../.gitbook/assets/image (224).png" alt="" width="563"><figcaption></figcaption></figure>

3\. Sélectionnez **Application Web** :

<figure><img src="../.gitbook/assets/image (225).png" alt="" width="504"><figcaption></figcaption></figure>

4\. Remplissez les URI de redirection. Cela devra être modifié en fonction de la façon dont vous l'hébergez : `http[s]://[votre-instance-flowise.com]/api/v1/google/callback` :

<figure><img src="../.gitbook/assets/image (226).png" alt="" width="563"><figcaption></figcaption></figure>

5\. Après la création, récupérez l'ID client et le secret :

<figure><img src="../.gitbook/assets/image (227).png" alt=""><figcaption></figcaption></figure>

6\. Retournez à l'application Flowise, ajoutez l'ID client et le secret. Testez la connexion et enregistrez-le.

<figure><img src="../.gitbook/assets/image (228).png" alt="" width="563"><figcaption></figcaption></figure>

## Auth0

1. Inscrivez-vous sur [Auth0](https://auth0.com/), puis créez une nouvelle application

<figure><img src="../.gitbook/assets/image (229).png" alt=""><figcaption></figcaption></figure>

2. Sélectionnez **Application Web régulière** :

<figure><img src="../.gitbook/assets/image (230).png" alt=""><figcaption></figcaption></figure>

3. Configurez les champs tels que Nom, Description. Prenez note du **Domaine**, de l'**ID client** et du **Secret client**.

<figure><img src="../.gitbook/assets/image (231).png" alt=""><figcaption></figcaption></figure>

4\. Remplissez les URI de l'application. Cela devra être modifié en fonction de la façon dont vous l'hébergez : `http[s]://[votre-instance-flowise.com]/api/v1/auth0/callback` :

<figure><img src="../.gitbook/assets/image (232).png" alt=""><figcaption></figcaption></figure>

5. Dans l'onglet API, assurez-vous que l'API de gestion Auth0 est activée avec les permissions suivantes
   * read:users
   * read:client\_grants

<figure><img src="../.gitbook/assets/image (233).png" alt=""><figcaption></figcaption></figure>

6\. Retournez à l'application Flowise, remplissez le Domaine, l'ID client et le Secret. Testez et enregistrez la configuration.

<figure><img src="../.gitbook/assets/image (234).png" alt="" width="563"><figcaption></figcaption></figure>

## Inviter des utilisateurs pour la connexion SSO

Pour qu'un nouvel utilisateur puisse se connecter, vous devez inviter de nouveaux utilisateurs dans l'application Flowise. Cela est essentiel pour garder une trace du rôle / espace de travail de l'utilisateur invité. Consultez la section [Inviter des utilisateurs](../using-flowise/workspaces.md#invite-user) pour la configuration des variables d'environnement.

L'utilisateur invité recevra un lien d'invitation pour se connecter :

<figure><img src="../.gitbook/assets/image (222).png" alt="" width="449"><figcaption></figcaption></figure>

En cliquant sur le bouton, l'utilisateur invité sera directement dirigé vers l'écran de connexion SSO de Flowise :

<figure><img src="../.gitbook/assets/image (210).png" alt="" width="400"><figcaption></figcaption></figure>

Ou naviguez vers l'application Flowise et connectez-vous avec SSO :

<figure><img src="../.gitbook/assets/image (211).png" alt="" width="437"><figcaption></figcaption></figure>