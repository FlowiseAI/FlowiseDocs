---
description: Apprenez à gérer les demandes d'API dans Flowise
---

# Limite de Taux

***

Lorsque vous partagez votre chatflow publiquement sans autorisation API via l'API ou le chat intégré, n'importe qui peut accéder au flux. Pour éviter le spam, vous pouvez définir la limite de taux sur votre chatflow.

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="462"><figcaption></figcaption></figure>

* **Limite de Messages par Durée** : Combien de messages peuvent être reçus dans une durée spécifique. Ex : 20
* **Durée en Secondes** : La durée spécifiée. Ex : 60
* **Message de Limite** : Quel message retourner lorsque la limite est dépassée. Ex : Quota Dépassé

En utilisant l'exemple ci-dessus, cela signifie que seulement 20 messages peuvent être reçus en 60 secondes. La limitation de taux est suivie par adresse IP. Si vous avez déployé Flowise sur un service cloud, vous devrez définir la variable d'environnement `NUMBER_OF_PROXIES`.

## Configuration de la Limite de Taux

Lorsque vous hébergez Flowise sur un cloud tel qu'AWS, GCP, Azure, etc., il est probable que vous soyez derrière un proxy/équilibreur de charge. Par conséquent, la limite de taux pourrait ne pas fonctionner. Plus d'infos peuvent être trouvées [ici](https://github.com/express-rate-limit/express-rate-limit/wiki/Troubleshooting-Proxy-Issues).

Pour résoudre le problème :

1. **Définir la Variable d'Environnement :** Créez une variable d'environnement nommée `NUMBER_OF_PROXIES` et définissez sa valeur à `0` dans votre environnement d'hébergement.
2. **Redémarrez votre instance Flowise hébergée :** Cela permet à Flowise d'appliquer les modifications des variables d'environnement.
3. **Vérifiez l'Adresse IP :** Pour vérifier l'adresse IP, accédez à l'URL suivante : `{{hosted_url}}/api/v1/ip`. Vous pouvez le faire en entrant l'URL dans votre navigateur ou en effectuant une requête API.
4. **Comparer l'Adresse IP** Après avoir effectué la requête, comparez l'adresse IP retournée avec votre adresse IP actuelle. Vous pouvez trouver votre adresse IP actuelle en visitant l'un de ces sites :
   * [http://ip.nfriedly.com/](http://ip.nfriedly.com/)
   * [https://api.ipify.org/](https://api.ipify.org/)
5. **Adresse IP Incorrecte :** Si l'adresse IP retournée ne correspond pas à votre adresse IP actuelle, augmentez `NUMBER_OF_PROXIES` de 1 et redémarrez votre instance Flowise. Répétez ce processus jusqu'à ce que l'adresse IP corresponde à la vôtre.