# Réponse de la conversation Mémoire

Utilisez la table de base de données Flowise`chat_message`comme mécanisme de stockage pour stocker / récupérer les conversations.

Ce type de mémoire crée un bref résumé de la conversation au fil du temps. Ceci est utile pour raccourcir les informations provenant de longues discussions. Il met à jour et enregistre un résumé actuel au fil de la conversation. Cela est particulièrement utile pour les chats plus longs, où la sauvegarde de chaque message passé prendrait trop de place.

<gigne> <img src = "../../../. GitBook / Assets / Image (3) (1) (1) (1) (2) .png" alt = "" width = "296"> <Figcaption> </ Figcaption> </ Figure>

## Saisir

| Paramètre | Description | Par défaut |
| ---------- | ----------------------------------------------------------------------------- | ------------- |
| Modèle de chat | LLM utilisé pour effectuer un résumé |               |
| ID de session | Un identifiant pour récupérer / stocker les messages. S'il n'est pas spécifié, un ID aléatoire sera utilisé. |               |
| Clé de mémoire | Une clé utilisée pour formater les messages dans le modèle d'invite | CHAT \ _History |
