# Résumé de la conversation Mémoire de tampon

Utilisez la table de base de données Flowise`chat_message`comme mécanisme de stockage pour stocker / récupérer les conversations.

Cette mémoire garde un tampon d'interactions récentes et compile les anciennes en un résumé, en utilisant les deux dans son stockage. Au lieu de rincer les vieilles interactions basées uniquement sur leur nombre, il considère désormais la longueur totale des jetons pour décider quand les éliminer.

<gigne> <img src = "../../../. GitBook / Assets / Image (4) (1) (2) (1) .png" alt = "" width = "297"> <Figcaption> </ Figcaption> </ Figure>

## Saisir

| Paramètre | Description | Par défaut |
| --------------- | ----------------------------------------------------------------------------- | ------------- |
| Modèle de chat | LLM utilisé pour effectuer un résumé |               |
| Limite de jeton maximum | Résumez les conversations une fois que la limite de jeton est atteinte | 2000 |
| ID de session | Un identifiant pour récupérer / stocker les messages. S'il n'est pas spécifié, un ID aléatoire sera utilisé. |               |
| Clé de mémoire | Une clé utilisée pour formater les messages dans le modèle d'invite | CHAT \ _History |
