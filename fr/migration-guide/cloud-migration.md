# Migration Cloud

Ce guide consiste à aider les utilisateurs à migrer du cloud V1 vers V2.

Dans Cloud V1, l'URL des applications ressemble à <Mark Style = "Color: Blue;"> ** https: // \ <your-instance-name> .app.flowiseai.com ** </mark>

Dans Cloud V2, l'URL des applications est <Mark Style = "Color: Blue;"> ** https: //cloud.flowiseai.com** </mark>

Pourquoi Cloud V2? Nous avons réécrit un nuage à partir de zéro, qui a une amélioration de la vitesse 5X, la capacité d'avoir plusieurs espaces de travail, des membres de l'organisation et, surtout, il est très évolutif avec[production-ready architecture](../configuration/running-in-production.md).

1. Connectez-vous à Cloud V1 via[https://flowiseai.com/auth/login](https://flowiseai.com/auth/login)
2. Dans votre tableau de bord, dans le coin supérieur droit:

<gigne> <img src = "../. GitBook / Assets / Image (8) (2) .png" alt = ""> <Figcaption> </gigcaption> </gigust>

3. ** Sélectionnez la version, puis mettez à jour vers la dernière version. **

<gigne> <img src = "../. GitBook / Assets / Migration-Guide / Cloud-Migration / 3.png" alt = "" width = "563"> <Figcaption> </ / Figcaption> </gigust>

4. Sélectionnez Exporter, sélectionnez les données que vous souhaitez exporter:

<gigne> <img src = "../. GitBook / Assets / Image (20) (2) .png" alt = "" width = "563"> <Figcaption> </ Figcaption> </gigne>

5. Enregistrez le fichier JSON exporté.
6. Accédez à Cloud V2[https://cloud.flowiseai.com](https://cloud.flowiseai.com/)
7. Le compte Cloud V2 ne se synchronise pas avec votre compte existant dans Cloud V1, vous devrez vous inscrire à nouveau ou vous connecter avec Google / GitHub.

<gigne> <img src = "../. GitBook / Assets / Image (37) .png" alt = "" width = "563"> <Figcaption> </ Figcaption> </gigust>

8. Une fois connecté, dans le coin du tableau de bord en haut à droite, cliquez sur Importer et téléchargez le fichier JSON exporté.

<gigne> <img src = "../. GitBook / Assets / Image (42) .png" alt = ""> <Figcaption> </gigcaption> </gigust>

9. Le nouvel utilisateur par défaut est sur le ** plan gratuit ** avec une limitation de 2 flux et assistants (pour chacun). Si vos données exportées en ont plus que cela, l'importation du fichier JSON exporté lancera une erreur. C'est pourquoi nous donnons <Mark Style = "Color: Orange;"> ** Premier mois gratuit ** </mark> sur ** Plan de démarrage ** qui a des flux et assistants illimités!

<gigne> <img src = "../. GitBook / Assets / Image (55) .png" alt = ""> <Figcaption> </gigcaption> </gigust>

10. Cliquez sur le bouton ** démarrage ** et ajoutez votre mode de paiement préféré:

<gigne> <img src = "../. GitBook / Assets / Image (67) .png" alt = "" width = "563"> <Figcaption> </gigcaption> </gigust>

<gigne> <img src = "../. GitBook / Assets / Image (80) .png" alt = ""> <Figcaption> </gigcaption> </gigust>

11. Après le mode de paiement supplémentaire, reviendrez pour couler, cliquez sur Démarrer sur le plan sélectionné et confirmez la modification:

<gigne> <img src = "../. GitBook / Assets / Image (95) .png" alt = "" width = "563"> <Figcaption> </gigcaption> </gigust>

12. Si tout se passe bien, vous devriez être sur le plan de démarrage avec des flux et assistants illimités! Hourra: Tada: essayez à nouveau d'importer le fichier JSON s'il échouait précédemment en raison de la limitation du plan libre.

{% Hint Style = "Success"%}
Tous les ID des données exportées restent les mêmes, vous n'avez donc pas à vous soucier de la mise à jour de l'ID pour l'API, il vous suffit de mettre à jour l'URL comme[https://cloud.flowiseai.com/api/v1/prediction/69fb1055-ghj324-ghj-0a4ytrerf](https://cloud.flowiseai.com/api/v1/prediction/69fb1055-ghj324-ghj-0a4ytrerf)
{% EndHint%}

{% hint style = "avertissement"%}
Les informations d'identification ne sont pas exportées. Vous devrez créer de nouvelles informations d'identification et les utiliser dans les flux et les assistants.
{% EndHint%}

13. Après avoir vérifié que tout fonctionne comme prévu, vous pouvez désormais annuler l'abonnement Cloud V1.
14. Dans le panneau du côté gauche, cliquez sur Paramètres du compte, faites défiler vers le bas et vous verrez ** Annuler l'abonnement précédent **:

<gigne> <img src = "../. GitBook / Assets / Image (135) .png" alt = ""> <figcaption> </gigcaption> </gigust>

15. Entrez votre e-mail précédent qui a été utilisé pour inscrire le Cloud V1 et appuyez sur ** Envoyer des instructions **.
16. Vous recevrez ensuite un e-mail pour annuler votre abonnement précédent:

<gigne> <img src = "../. GitBook / Assets / Image (136) .png" alt = "" width = "563"> <Figcaption> </gigcaption> </gigust>

17. Cliquez sur le bouton ** Gérer l'abonnement ** vous amènera à un portail où vous pouvez annuler l'abonnement Cloud V1. Votre application Cloud V1 sera ensuite fermée sur le prochain cycle de facturation.

<gigne> <img src = "../. GitBook / Assets / Image (137) .png" alt = ""> <Figcaption> </gigcaption> </gigust>

Nous nous excusons sincèrement pour tout inconvénient que nous avons causé au cours du processus de migration. Si quelque chose, nous aimerions aider, n'hésitez pas à nous joindre à support@flowiseai.com.
