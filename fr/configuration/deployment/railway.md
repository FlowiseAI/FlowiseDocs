---
description: Apprenez à déployer Flowise sur Railway
---

# Railway

***

1. Cliquez sur le [modèle](https://railway.app/template/pn4G8S?referralCode=WVNPD9) préconstruit suivant
2. Cliquez sur Déployer maintenant

<figure><img src="../../.gitbook/assets/image (1) (1) (2) (1).png" alt=""><figcaption></figcaption></figure>

3. Changez le nom du dépôt selon votre préférence et cliquez sur Déployer

<figure><img src="../../.gitbook/assets/image (2) (1) (2) (1).png" alt="" width="375"><figcaption></figcaption></figure>

4. Si cela réussit, vous devriez voir une URL déployée

<figure><img src="../../.gitbook/assets/image (2) (2).png" alt=""><figcaption></figcaption></figure>

5. Pour ajouter une autorisation, allez dans l'onglet Variables et ajoutez :

* FLOWISE\_USERNAME
* FLOWISE\_PASSWORD

<figure><img src="../../.gitbook/assets/image (15) (2) (1) (1).png" alt=""><figcaption></figcaption></figure>

6. Il existe une liste de variables d'environnement que vous pouvez configurer. Consultez [environment-variables.md](../environment-variables.md "mention")

C'est tout ! Vous avez maintenant un Flowise déployé sur Railway [🎉](https://emojipedia.org/party-popper/)[🎉](https://emojipedia.org/party-popper/)

## Volume Persistant

Le système de fichiers par défaut pour les services fonctionnant sur Railway est éphémère. Les données de Flowise ne sont pas conservées entre les déploiements et les redémarrages. Pour résoudre ce problème, nous pouvons utiliser [Railway Volume](https://docs.railway.app/reference/volumes).

Pour simplifier les étapes, nous avons un modèle Railway avec volume monté : [https://railway.app/template/nEGbjR](https://railway.app/template/nEGbjR)

Il suffit de cliquer sur Déployer et de remplir les variables d'environnement comme ci-dessous :

* DATABASE\_PATH - `/opt/railway/.flowise`
* APIKEY\_PATH - `/opt/railway/.flowise`
* LOG\_PATH - `/opt/railway/.flowise/logs`
* SECRETKEY\_PATH - `/opt/railway/.flowise`
* BLOB\_STORAGE\_PATH - `/opt/railway/.flowise/storage`

<figure><img src="../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="420"><figcaption></figcaption></figure>

Essayez maintenant de créer un flux et de l'enregistrer dans Flowise. Ensuite, essayez de redémarrer le service ou de redéployer, vous devriez toujours voir le flux que vous avez enregistré précédemment.