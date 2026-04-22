---
description: Load data from an API.
---

# Chargeur de documents API

<gigne> <img src = "../../../. Gitbook / Assets / Image (9) (1) (1) (1) (1) (1) (1) .png" alt = "" width = "273"> </ figure> <p> API Loder Node </p> </figcaption> </gigon>

Le chargeur de document API fournit des fonctionnalités pour charger et traiter les données des API externes à l'aide de requêtes HTTP. Ce module permet une intégration transparente avec des API et services Web RESTful.

Ce module fournit un chargeur de document API polyvalent qui peut:
- Faire obtenir HTTP et publier des demandes
- Gérer les en-têtes personnalisés et les corps de demande
- Traiter les réponses de l'API dans les documents
- Soutenir les structures de données JSON
- Personnaliser l'extraction des métadonnées
- Traiter les réponses avec des séparateurs de texte

## Entrées

### Paramètres requis
- ** URL **: L'URL de point de terminaison de l'API à appeler
- ** Méthode **: Méthode HTTP à utiliser (obtenir ou publier)

### Paramètres facultatifs
- ** En-têtes **: objet JSON contenant des en-têtes HTTP
- ** Corps **: objet JSON pour le corps de la demande postale
- ** Splitter du texte **: un séparateur de texte pour traiter le contenu extrait
- ** Metadata supplémentaires **: objet JSON avec métadonnées supplémentaires
- ** omettre les clés de métadonnées **: Liste des clés de métadonnées séparées par des virgules pour omettre

## Sorties

- ** Document **: tableau d'objets de document contenant des métadonnées et un conceptent
- ** Texte **: chaîne concaténée du conceptent de documents

## Caractéristiques
- Prise en charge de la méthode HTTP (GET / POST)
- Configuration de l'en-tête personnalisée
- Demander la personnalisation du corps
- Traitement des réponses
- Gestion des erreurs
- Personnalisation des métadonnées
- Capacités de division de texte

## Exemple d'utilisation

### Faire une demande
```json
{
    "method": "GET",
    "url": "https://api.example.com/data",
    "headers": {
        "Authorization": "Bearer token123",
        "Accept": "application/json"
    }
}
```

### Demande de poste
```json
{
    "method": "POST",
    "url": "https://api.example.com/data",
    "headers": {
        "Content-Type": "application/json",
        "Authorization": "Bearer token123"
    },
    "body": {
        "query": "example",
        "limit": 10
    }
}
```

## Notes
- Prend en charge les formats de demande / réponse JSON
- Gère les réponses d'erreur HTTP
- Traite automatiquement les données de réponse dans les documents
- Peut être combiné avec des séparateurs de texte pour le traitement du contenu
- Prend en charge l'ajout et l'omission des métadonnées personnalisées
- Les réponses d'erreur sont correctement gérées et signalées
