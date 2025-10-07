# Espaces de travail

{% hint style = "info"%}
Les évaluations ne sont disponibles que pour le cloud et le plan d'entreprise
{% EndHint%}

Lors de votre connexion initiale, un espace de travail par défaut sera généré automatiquement pour vous. Les espaces de travail servent à partitionner les ressources entre diverses équipes ou unités commerciales. À l'intérieur de chaque espace de travail, le contrôle d'accès basé sur les rôles (RBAC) est utilisé pour gérer les autorisations et l'accès, garantissant que les utilisateurs n'ont accès qu'aux ressources et paramètres requis pour leur rôle.

<Figure> <img src = "../. GitBook / Assets / Untitled-2024-10-19-0050.png" alt = ""> <Figcaption> </gigcaption> </gigne>

## Configuration du compte d'administration

<Dettots>

<summary> Pour l'entreprise auto-hébergée, les variables Env suivantes doivent être définies </summary>

```
JWT_AUTH_TOKEN_SECRET
JWT_REFRESH_TOKEN_SECRET
JWT_ISSUER
JWT_AUDIENCE
JWT_TOKEN_EXPIRY_IN_MINUTES
JWT_REFRESH_TOKEN_EXPIRY_IN_MINUTES
PASSWORD_RESET_TOKEN_EXPIRY_IN_MINS
PASSWORD_SALT_HASH_ROUNDS
TOKEN_HASH_SECRET
```

</fords>

Par défaut, la nouvelle installation de Flowise nécessitera une configuration d'administration, similaire à la façon dont vous devez configurer un utilisateur racine pour votre base de données initialement.

<gigne> <img src = "../. Gitbook / Assets / image (2) (1) (1) (1) (1) (1) (2) .png" alt = "" width = "478"> <figCaption> </gigcaption> </gigu

Après la configuration, l'utilisateur sera amené au tableau de bord Flowise. Depuis la barre gauche, vous verrez la section de gestion des utilisateurs et de l'espace de travail. Un espace de travail par défaut a été automatiquement créé.

<gigne> <img src = "../. Gitbook / Assets / image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (2) (1) (1).

## Création d'espace de travail

Pour créer un nouvel espace de travail, cliquez sur Ajouter un nouveau:

<gigne> <img src = "../. Gitbook / Assets / image (3) (1) (1) (2) .png" alt = ""> <figcaption> </gigcaption> </gigust>

Vous vous verrez ajouté en tant qu'administrateur de l'organisation dans l'espace de travail que vous avez créé.

<gigne> <img src = "../. GitBook / Assets / Image (4) (1) (2) .png" alt = ""> <Figcaption> </gigcaption> </gigust>

Pour inviter de nouveaux utilisateurs dans l'espace de travail, vous devez d'abord créer un rôle.

## Créer un rôle

Accédez à des rôles dans la barre gauche et cliquez sur Ajouter un rôle:

<gigne> <img src = "../. Gitbook / Assets / image (5) (1) (1) (1) (1) (1) (1) (1) (1) (1) .png" alt = ""> <figcaption> </gigcaption> </gigu

L'utilisateur peut spécifier le contrôle granulaire des autorisations pour chaque ressource. Les seules exceptions sont les ressources de ** User & Workspace Management ** (rôles, utilisateurs, espaces de travail, activité de connexion). Ceux-ci ne sont disponibles que pour l'administrateur de compte pour l'instant.

Ici, nous créons un rôle d'éditeur qui a accès à tout. Et un autre rôle avec les autorisations de vue uniquement.

<gigne> <img src = "../. GitBook / Assets / Image (6) (1) (1) (1) (1) (1) (1) (1) .png" alt = ""> <figCaption> </gigcaption> </ figure>

## Inviter l'utilisateur

<Dettots>

<summary> Pour l'entreprise auto-hébergée, les variables Env suivantes doivent être définies </summary>

```
INVITE_TOKEN_EXPIRY_IN_HOURS
SMTP_HOST
SMTP_PORT
SMTP_USER
SMTP_PASSWORD
```

</fords>

Accédez aux utilisateurs de la barre gauche, vous vous verrez comme l'administrateur du compte. Ceci est indiqué par l'icône de la personne avec une étoile:

<gigne> <img src = "../. Gitbook / Assets / image (7) (1) (1) (1) (1) (1) (1) (1) .png" alt = ""> <figcaption> </gigcaption> </gigu

Cliquez sur Inviter l'utilisateur et entrez les e-mails à inviter, l'espace de travail à être attribué et le rôle également.

<gigne> <img src = "../. Gitbook / Assets / Image (8) (1) (1) (1) (1) (1) (1) .png" alt = ""> <figcaption> </gigcaption> </stigne>

Cliquez sur Inviter. L'e-mail invité recevra une invitation:

<gigne> <img src = "../. GitBook / Assets / Image (9) (1) (1) (1) (1) (1) .png" alt = ""> <figCaption> </ / Figcaption> </ Figure>

En cliquant sur le lien d'invitation, l'utilisateur invité sera amené à une page d'inscription.

<gigne> <img src = "../. GitBook / Assets / image (10) (1) (1) (1) (1) (1) .png" alt = "" width = "463"> <figCaption> </gigcaption> </gigust>

Après avoir été inscrit et connecté en tant qu'utilisateur invité, vous serez dans l'espace de travail attribué, et il n'y aura pas de section de gestion de l'utilisateur et de l'espace de travail:

<gigne> <img src = "../. GitBook / Assets / Image (11) (1) (1) (1) (1) (1) .png" alt = ""> <figcaption> </gigcaption> </gistre>

Si vous êtes invité dans plusieurs espaces de travail, vous pouvez passer à différents espaces de travail à partir du bouton déroulant supérieur droit. Ici, nous sommes affectés à Workspace 2 avec ** Voir uniquement ** Permission. Vous pouvez remarquer que le bouton Ajouter un nouveau pour ChatFlow n'est plus visible. Cela garantit que l'utilisateur ne peut que visualiser, et non créer, mettre à jour ni supprimer. Les mêmes règles RBAC s'appliquent également à l'API.

<gigne> <img src = "../. Gitbook / Assets / image (12) (1) (1) (1) (1) (1) .png" alt = ""> <figcaption> </ / figCaption> </gigust>

Maintenant, de retour à l'administrateur de compte, vous pourrez voir les utilisateurs invités, leur statut, leurs rôles et l'espace de travail actif:

<gigne> <img src = "../. Gitbook / Assets / image (14) (1) (1) (1) (1) (1) .png" alt = ""> <figCaption> </gigcaption> </gigne>

L'administrateur du compte peut également modifier les paramètres des autres utilisateurs:

<gigne> <img src = "../. GitBook / Assets / Image (15) (1) (1) (1) (1) (1) .png" alt = ""> <figCaption> </ / Figcaption> </ Figure>

## Activité de connexion

L'administrateur pourra voir chaque connexion et déconnexion de tous les utilisateurs:

<gigne> <img src = "../. GitBook / Assets / image (13) (1) (1) (2) .png" alt = ""> <figcaption> </gigcaption> </gigust>

## Création d'un élément dans l'espace de travail

Tous les éléments créés dans un espace de travail sont isolés d'un autre espace de travail. Les espaces de travail sont un moyen de regrouper logiquement les utilisateurs et les ressources au sein d'une organisation, garantissant des limites de confiance distinctes pour la gestion des ressources et le contrôle d'accès. Il est recommandé de créer des espaces de travail distincts pour chaque équipe.

Ici, nous créons un chatflow nommé ** Chatflow1 ** dans ** workspace1 **:

<gigne> <img src = "../. GitBook / Assets / Image (16) (1) (1) (2) .png" alt = ""> <Figcaption> </gigcaption> </gigust>

Lorsque nous passons à ** workspace2 **, ** Chatflow1 ** ne sera pas visible. Cela s'applique à toutes les ressources telles que les flux d'agent, les outils, les assistants, etc.

<gigne> <img src = "../. GitBook / Assets / Image (17) (1) (1) (1) .png" alt = ""> <Figcaption> </ Figcaption> </gigust>

Le diagramme ci-dessous illustre la relation entre les organisations, les espaces de travail et les différentes ressources associées et contenues dans un espace de travail.

<Figure> <img src = "../. GitBook / Assets / Untitled-2024-10-19-0050.png" alt = ""> <Figcaption> </gigcaption> </gigne>

## Partage d'identification

Vous pouvez partager des informations d'identification à d'autres espaces de travail. Cela permet aux utilisateurs de réutiliser le même ensemble d'identification dans différents espaces de travail.

Après avoir créé un diplôme, l'administrateur de compte ou l'utilisateur avec l'autorisation de partage des informations d'identification du RBAC pourra cliquer sur Partager:

<gigne> <img src = "../. GitBook / Assets / Image (18) (1) (1) (1) .png" alt = ""> <Figcaption> </gigcaption> </gigust>

L'utilisateur peut sélectionner les espaces de travail pour partager les informations d'identification avec:

<gigne> <img src = "../. GitBook / Assets / Image (19) (1) (1) .png" alt = ""> <Figcaption> </gigcaption> </ figure>

Maintenant, passez à l'espace de travail où les informations d'identification ont été partagées, vous verrez les informations d'identification partagées. L'utilisateur n'est pas en mesure de modifier les informations d'identification partagées.

<gigne> <img src = "../. GitBook / Assets / Image (20) (1) (1) .png" alt = ""> <Figcaption> </gigcaption> </gigust>

## Suppression d'un espace de travail

Actuellement, seul l'administrateur du compte peut supprimer les espaces de travail. Par défaut, vous n'êtes pas en mesure de supprimer un espace de travail s'il y a encore des utilisateurs dans cet espace de travail.

<gigne> <img src = "../. GitBook / Assets / Image (21) (1) (1) .png" alt = ""> <figcaption> </gigcaption> </gigust>

Vous devrez d'abord dissoudre tous les utilisateurs invités. Cela permettait de la flexibilité au cas où vous souhaitez simplement supprimer certains utilisateurs d'un espace de travail. Notez que le propriétaire de l'organisation qui a créé l'espace de travail n'est pas en mesure d'être non lié à un espace de travail.

<gigne> <img src = "../. GitBook / Assets / Image (22) (1) .png" alt = ""> <figcaption> </gigcaption> </gigust>

Après avoir désabillé les utilisateurs invités, et le seul utilisateur laissé dans l'espace de travail est le propriétaire de l'organisation, le bouton de suppression est maintenant cliquable:

<gigne> <img src = "../. GitBook / Assets / Image (23) (1) .png" alt = ""> <figcaption> </gigcaption> </gigust>

La suppression d'un espace de travail est une action irréversible et sera en cascade de supprimer tous les éléments de cet espace de travail. Vous verrez une boîte d'avertissement:

<gigne> <img src = "../. GitBook / Assets / Image (24) .png" alt = ""> <Figcaption> </gigcaption> </gigust>

Après avoir supprimé un espace de travail, l'utilisateur se repliera à l'espace de travail par défaut. L'espace de travail par défaut qui a été automatiquement créé au début n'est pas en mesure d'être supprimé.
