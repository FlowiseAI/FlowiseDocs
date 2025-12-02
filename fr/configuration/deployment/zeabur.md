---
description: Apprenez à déployer Flowise sur Zeabur
---

# Zeabur

***

{% hint style="warning" %}
Veuillez noter que le modèle suivant créé par Zeabur est obsolète (du 2024-01-24).
{% endhint %}

1. Cliquez sur le [modèle](https://zeabur.com/templates/2JYZTR) préconstruit ci-dessous ou sur le bouton ci-dessous.

[![Déployer sur Zeabur](https://zeabur.com/button.svg)](https://zeabur.com/templates/2JYZTR)

2. Cliquez sur Déployer

<figure><img src="../../.gitbook/assets/zeabur/1.png" alt="modèle zeabur"><figcaption></figcaption></figure>

3. Sélectionnez votre région préférée et continuez

<figure><img src="../../.gitbook/assets/zeabur/2.png" alt="sélectionner la région"><figcaption></figcaption></figure>

4. Vous serez redirigé vers le tableau de bord de Zeabur et vous verrez le processus de déploiement

<figure><img src="../../.gitbook/assets/zeabur/3.png" alt="processus de déploiement"><figcaption></figcaption></figure>

5. Pour ajouter une autorisation, allez dans l'onglet Variables et ajoutez :

* FLOWISE\_USERNAME
* FLOWISE\_PASSWORD

<figure><img src="../../.gitbook/assets/zeabur/4.png" alt="autorisation"><figcaption></figcaption></figure>

6. Il existe une liste de variables d'environnement que vous pouvez configurer. Consultez [environment-variables.md](../environment-variables.md "mention")

C'est tout ! Vous avez maintenant déployé Flowise sur Zeabur [🎉](https://emojipedia.org/party-popper/)[🎉](https://emojipedia.org/party-popper/)

## Volume Persistant

Zeabur créera automatiquement un volume persistant pour vous, donc vous n'avez pas à vous en soucier.