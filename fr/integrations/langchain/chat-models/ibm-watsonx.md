# IBM Watsonx

## Prérequis

1. Inscrivez-vous sur [IBM Watsonx](https://www.ibm.com/watsonx)
2. Créez un nouveau projet :

<figure><img src="../../../.gitbook/assets/image (238).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (239).png" alt=""><figcaption></figcaption></figure>

3. Une fois le projet créé, retournez au tableau de bord principal et cliquez sur **Explorer les modèles de base** :

<figure><img src="../../../.gitbook/assets/image (240).png" alt=""><figcaption></figcaption></figure>

4. Choisissez le modèle que vous souhaitez utiliser et ouvrez-le dans Prompt Lab :

<figure><img src="../../../.gitbook/assets/image (241).png" alt=""><figcaption></figcaption></figure>

5. Dans le coin supérieur droit, cliquez sur Afficher le code :

<figure><img src="../../../.gitbook/assets/image (242).png" alt=""><figcaption></figcaption></figure>

6. Notez le paramètre `model_id` et `version`. Dans ce cas, c'est `ibm/granite-3-8b-instruct,` et la version est `2023-05-29`.
7. Cliquez sur la barre de navigation à gauche, puis cliquez sur Accès développeur

<figure><img src="../../../.gitbook/assets/image (243).png" alt="" width="308"><figcaption></figcaption></figure>

8. Notez l'URL `watsonx.ai`, l'ID du projet et créez une nouvelle clé API depuis le tableau de bord IBM Cloud.
9. À ce stade, vous devriez avoir les informations suivantes :
   * URL Watsonx.ai
   * ID du projet
   * Clé API
   * Version du modèle
   * ID du modèle

## Configuration

1. **Modèles de chat** > faites glisser le nœud **ChatIBMWatsonx**

<figure><img src="../../../.gitbook/assets/image (244).png" alt="" width="306"><figcaption></figcaption></figure>

2. Remplissez le modèle avec l'ID du modèle précédemment. Créez de nouvelles informations d'identification et remplissez tous les détails.

<figure><img src="../../../.gitbook/assets/image (245).png" alt="" width="419"><figcaption></figcaption></figure>

2. Voilà [🎉](https://emojipedia.org/party-popper/), vous pouvez maintenant utiliser le **nœud ChatIBMWatsonx** dans Flowise !

<figure><img src="../../../.gitbook/assets/image (246).png" alt=""><figcaption></figcaption></figure>