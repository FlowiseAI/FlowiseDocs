---
description: Load and process data from web search results.
---

# Serpapi pour la recherche Web

<gigne> <img src = "../../../. GitBook / Assets / Image (81) .png" alt = "" width = "319"> <Figcaption> <p> serpapi pour le nœud de recherche Web </p> </gigcaption> </pigucion>

Le serpapi pour le chargeur de recherche Web vous permet de récupérer et de traiter les résultats de recherche Web à l'aide du service SERPAPI. Ce chargeur transforme les résultats de recherche en documents structurés qui peuvent être facilement intégrés dans votre flux de travail, ce qui le rend idéal pour les applications nécessitant des données de recherche Web en temps réel.

## Caractéristiques
- Résultats de recherche Web en temps réel
- Capacités de division de texte
- Manipulation des métadonnées personnalisables
- Formats de sortie multiples
- Authentification des clés de l'API
- Traitement de documents efficace

## Entrées

### Paramètres requis
- ** Connectez les informations d'identification **: les informations d'identification de la clé de l'API serpapi
- ** requête **: la requête de recherche à exécuter

### Paramètres facultatifs
- ** Splitter du texte **: un séparateur de texte pour traiter le contenu extrait
- ** Métadonnées supplémentaires **: objet JSON avec des métadonnées supplémentaires à ajouter aux documents
- ** omettre les clés de métadonnées **: Liste des clés de métadonnées séparées par des virgules à exclure
  - Format:`key1, key2, key3.nestedKey1`
  - Utiliser * pour supprimer toutes les métadonnées par défaut, sauf les métadonnées personnalisées

## Sorties

- ** Document **: tableau d'objets de document contenant:
  - métadonnées: métadonnées du résultat de la recherche
  - contenu de la recherche: contenu de résultat de recherche
- ** Texte **: chaîne concaténée du contenu de tous les résultats de recherche

## Structure de document
Chaque document contient:
- ** PageContent **: le contenu principal du résultat de la recherche
- ** Metadata **:
  - Métadonnées de résultat de recherche par défaut
  - Métadonnées personnalisées (si spécifiées)
  - Métadonnées filtrées (basées sur les clés omises)

## Manipulation des métadonnées
Deux façons de personnaliser les métadonnées:
1. ** métadonnées supplémentaires **
   - Ajouter de nouveaux champs de métadonnées via JSON
   - Fusionné avec les métadonnées existantes
   - Utile pour ajouter un suivi ou une catégorisation personnalisée

2. ** omettre les clés de métadonnées **
   - Retirer les champs de métadonnées indésirables
   - Liste de clés séparée par des virgules pour exclure
   - Prise en charge de la suppression des clés imbriqués
   - Utiliser * pour supprimer toutes les métadonnées par défaut

## Conseils d'utilisation
- Fournir des requêtes de recherche spécifiques pour de meilleurs résultats
- Utilisez des séparateurs de texte pour les grands résultats de recherche
- Personnalisez les métadonnées pour répondre à vos besoins
- Considérez les limites de taux lors de la fabrication de requêtes multiples
- Gérer les résultats de recherche de manière appropriée en fonction de la taille

## Notes
- Nécessite la clé de l'API SERPAPI
- Respecte les limites de taux de l'API
- Résultats de recherche en temps réel
- Traitement économe en mémoire
- Gestion des erreurs pour les demandes d'API
- Prend en charge les formats de sortie de document et de texte

## Exemple d'utilisation
```typescript
// Example search query
query: "artificial intelligence latest developments"

// Example additional metadata
metadata: {
  "source": "serpapi",
  "category": "tech",
  "timestamp": "2024-03-21"
}

// Example metadata keys to omit
omitMetadataKeys: "snippet, position, link"
```
