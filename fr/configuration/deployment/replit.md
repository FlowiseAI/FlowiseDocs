---
description: Apprenez à déployer Flowise sur Replit
---

# Replit

***

1. Connectez-vous à [Replit](https://replit.com/~)
2. Créez un nouveau **Repl**. Sélectionnez **Node.js** comme modèle et remplissez votre **Titre** préféré.

<figure><img src="../../.gitbook/assets/image (18) (1) (2) (1).png" alt="" width="551"><figcaption></figcaption></figure>

3. Après la création d'un nouveau Repl, dans la barre latérale gauche, cliquez sur Secret :

<figure><img src="../../.gitbook/assets/image (2) (4) (1).png" alt="" width="219"><figcaption></figcaption></figure>

4. Créez 3 Secrets pour ignorer le téléchargement de Chromium pour les bibliothèques Puppeteer et Playwright.

<table><thead><tr><th width="403">Secrets</th><th>Valeur</th></tr></thead><tbody><tr><td>PLAYWRIGHT_SKIP_BROWSER_DOWNLOAD</td><td>1</td></tr><tr><td>PUPPETEER_SKIP_DOWNLOAD</td><td>true</td></tr><tr><td>PUPPETEER_SKIP_CHROMIUM_DOWNLOAD</td><td>true</td></tr></tbody></table>

<figure><img src="../../.gitbook/assets/image (5) (3).png" alt="" width="535"><figcaption></figcaption></figure>

5. Vous pouvez maintenant passer à l'onglet Shell

<figure><img src="../../.gitbook/assets/image (13) (2) (1).png" alt="" width="539"><figcaption></figcaption></figure>

6. Tapez `npm install -g flowise` dans la fenêtre du terminal Shell. Si vous rencontrez une erreur concernant une version de node incompatible, utilisez la commande suivante `yarn global add flowise --ignore-engines`

<figure><img src="../../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="530"><figcaption></figcaption></figure>

7. Ensuite, suivez avec `npx flowise start`

<figure><img src="../../.gitbook/assets/image (17) (1) (2).png" alt="" width="533"><figcaption></figcaption></figure>

8. Vous devriez maintenant pouvoir voir Flowise sur Replit !

<figure><img src="../../.gitbook/assets/image (15) (3).png" alt="" width="545"><figcaption></figcaption></figure>

9. Vous verrez maintenant une page de connexion. Connectez-vous simplement avec le nom d'utilisateur et le mot de passe que vous avez définis.

<figure><img src="../../.gitbook/assets/image (12) (2) (1).png" alt=""><figcaption></figcaption></figure>