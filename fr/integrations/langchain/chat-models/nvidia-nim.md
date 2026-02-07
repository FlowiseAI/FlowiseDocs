# NVIDIA NIM

## Local

### Remarque importante sur l'exécution de NIM avec Flowise

Si une instance NIM existante est déjà en cours d'exécution (par exemple, via ChatRTX de NVIDIA), démarrer une autre instance via Flowise **sans vérifier un point de terminaison existant** peut provoquer des conflits. Ce problème se produit lorsque plusieurs commandes `podman run` sont exécutées sur le même NIM, entraînant des échecs.

Pour obtenir de l'aide, consultez :

- **[Forums des développeurs NVIDIA](https://forums.developer.nvidia.com/)** – Pour les problèmes techniques et les questions.
- **[Discord des développeurs NVIDIA](https://discord.gg/nvidiadeveloper)** – Pour l'engagement communautaire et les [annonces](https://discord.com/channels/1019361803752456192/1340013505834647572).

### Prérequis

1. Configurez [NVIDIA NIM localement avec WSL2](https://docs.nvidia.com/nim/wsl2/1.0.0/getting-started.html).

### Flowise

1. **Modèles de chat** > Faites glisser le nœud **Chat NVIDIA NIM** > Cliquez sur **Configurer NIM localement**.

<figure><img src="../../../.gitbook/assets/nvidia-nim-local-1.png" alt=""><figcaption></figcaption></figure>

2. Si NIM est déjà installé, cliquez sur **Suivant**. Sinon, cliquez sur **Télécharger** pour démarrer l'installateur.

<figure><img src="../../../.gitbook/assets/nvidia-nim-local-2.png" alt=""><figcaption></figcaption></figure>

3. Sélectionnez une image de modèle à télécharger.

<figure><img src="../../../.gitbook/assets/nvidia-nim-local-3.png" alt=""><figcaption></figcaption></figure>

4. Une fois sélectionnée, cliquez sur **Suivant** pour continuer le téléchargement.

<figure><img src="../../../.gitbook/assets/nvidia-nim-local-4.png" alt=""><figcaption></figcaption></figure>

5. **Téléchargement de l'image** – La durée dépend de la vitesse de connexion Internet.

<figure><img src="../../../.gitbook/assets/nvidia-nim-local-5.png" alt=""><figcaption></figcaption></figure>

6. En savoir plus sur [Relaxer les contraintes de mémoire](https://docs.nvidia.com/nim/large-language-models/1.7.0/configuration.html#environment-variables).  
   Le **port hôte** est le port que le conteneur doit mapper à la machine locale.

<figure><img src="../../../.gitbook/assets/nvidia-nim-local-6.png" alt=""><figcaption></figcaption></figure>

7. **Démarrage du conteneur...**

<figure><img src="../../../.gitbook/assets/nvidia-nim-local-7.png" alt=""><figcaption></figcaption></figure>

_Remarque : Si vous avez déjà un conteneur en cours d'exécution avec le modèle sélectionné, Flowise vous demandera si vous souhaitez réutiliser le conteneur en cours d'exécution. Vous pouvez choisir de réutiliser le conteneur en cours d'exécution ou d'en démarrer un nouveau avec un port différent._

<figure><img src="../../../.gitbook/assets/nvidia-nim-container-exists.png" alt=""><figcaption></figcaption></figure>

8. **Enregistrer le flux de discussion**

9. [🎉](https://emojipedia.org/party-popper/) **Voilà !** Votre nœud **Chat NVIDIA NIM** est maintenant prêt à être utilisé dans Flowise !

<figure><img src="../../../.gitbook/assets/nvidia-nim-local-8.png" alt=""><figcaption></figcaption></figure>

## Cloud

### Prérequis

1. Connectez-vous ou inscrivez-vous sur [NVIDIA](https://build.nvidia.com/).
2. Dans la barre de navigation en haut, cliquez sur NIM :

<figure><img src="../../../.gitbook/assets/image (247).png" alt=""><figcaption></figcaption></figure>

3. Recherchez le modèle que vous souhaitez utiliser. Pour le télécharger localement, nous allons utiliser Docker :

<figure><img src="../../../.gitbook/assets/image (248).png" alt=""><figcaption></figcaption></figure>

4. Suivez les instructions de configuration de Docker. Vous devez d'abord obtenir une clé API pour tirer l'image Docker :

<figure><img src="../../../.gitbook/assets/image (249).png" alt="" width="563"><figcaption></figcaption></figure>

### Flowise

1. **Modèles de chat** > faites glisser le nœud **Chat NVIDIA NIM**

<figure><img src="../../../.gitbook/assets/image (250).png" alt=""><figcaption></figcaption></figure>

2. Si vous utilisez un point de terminaison hébergé par NVIDIA, vous devez avoir votre clé API. **Connecter les identifiants** > cliquez sur **Créer nouveau.** Cependant, si vous utilisez une configuration locale, cela est optionnel.

<div align="left"><figure><img src="../../../.gitbook/assets/image (251).png" alt=""><figcaption></figcaption></figure> <figure><img src="../../../.gitbook/assets/Screenshot 2024-12-23 180712.png" alt=""><figcaption></figcaption></figure></div>

3. Entrez le nom du modèle et voilà [🎉](https://emojipedia.org/party-popper/), votre **nœud Chat NVIDIA NIM** est maintenant prêt à être utilisé dans Flowise !

<figure><img src="../../../.gitbook/assets/image (252).png" alt=""><figcaption></figcaption></figure>

### Ressources

- [NVIDIA LLM Guide de démarrage](https://docs.nvidia.com/nim/large-language-models/latest/getting-started.html)
- [NVIDIA NIM](https://build.nvidia.com/microsoft/phi-3-mini-4k?snippet_tab=Docker)