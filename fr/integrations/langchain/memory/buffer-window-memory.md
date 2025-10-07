# Mémoire de fenêtre de tampon

Utilisez la table de base de données Flowise`chat_message`comme mécanisme de stockage pour stocker / récupérer les conversations.

La différence étant, elle ne fait que les dernières interactions k. Cette approche est bénéfique pour préserver une fenêtre coulissante des interactions les plus récentes, garantissant que le tampon reste gérable en taille.

<gigne> <img src = "../../../. GitBook / Assets / Image (1) (1) (3) (1) .png" alt = "" width = "298"> <Figcaption> </ Figcaption> </ Figure>

## Saisir

| Paramètre | Description | Par défaut |
| ---------- | ----------------------------------------------------------------------------- | ------------- |
| Taille | Dernier k messages à récupérer | 4 |
| ID de session | Un identifiant pour récupérer / stocker les messages. S'il n'est pas spécifié, un ID aléatoire sera utilisé. |               |
| Clé de mémoire | Une clé utilisée pour formater les messages dans le modèle d'invite | CHAT \ _History |
