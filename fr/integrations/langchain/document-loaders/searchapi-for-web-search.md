---
description: Load data from real-time search results.
---

# Searchapi pour la recherche sur le Web

<gigne> <img src = "../../../. GitBook / Assets / Image (8) (1) (1) (1) (1) (1) (1) (2) .png" alt = "" width = "322"> <pgaption> <p> searchapi pour la recherche Web </p> </figcaption> </ / figure>

Le SeechAPI pour le chargeur de recherche Web donne accès aux résultats de recherche en temps réel à partir de plusieurs moteurs de recherche à l'aide du service SearchAPI. Ce chargeur vous permet de récupérer, de traiter et de structurer les résultats de recherche comme des documents qui peuvent être utilisés dans votre flux de travail.

## Caractéristiques

* Résultats de recherche en temps réel à partir de plusieurs moteurs de recherche
* Paramètres de recherche personnalisables
* Capacités de division de texte
* Manipulation flexible des métadonnées
* Formats de sortie multiples
* Authentification des clés de l'API

## Entrées

### Paramètres requis

* ** Connectez les informations d'identification **: Recherche d'identification de la clé de l'API SearchAPI
* Au moins un de:
  * ** requête **: chaîne de requête de recherche
  * ** Paramètres personnalisés **: objet JSON avec paramètres de recherche

### Paramètres facultatifs

* ** Query **: la requête de recherche à exécuter (sinon en utilisant des paramètres personnalisés)
* ** Paramètres personnalisés **: objet JSON avec paramètres de recherche supplémentaires
  * Prend en charge tous les paramètres de[SearchApi documentation](https://www.searchapi.io/docs/google)
  * Peut remplacer les paramètres par défaut
  * Permet des configurations spécifiques au moteur
* ** Splitter du texte **: un séparateur de texte pour traiter le contenu extrait
* ** Métadonnées supplémentaires **: objet JSON avec des métadonnées supplémentaires à ajouter aux documents
* ** omettre les clés de métadonnées **: Liste des clés de métadonnées séparées par des virgules à exclure
  * Format:`key1, key2, key3.nestedKey1`
  * Utiliser \ * pour supprimer toutes les métadonnées par défaut

## Sorties

* ** Document **: tableau d'objets de document contenant:
  * métadonnées: métadonnées du résultat de la recherche
  * contenu de la recherche: contenu de résultat de recherche
* ** Texte **: chaîne concaténée du contenu de tous les résultats de recherche

## Structure de document

Chaque document contient:

* ** PageContent **: le contenu principal du résultat de la recherche
* ** Metadata **:
  * Métadonnées de résultat de recherche par défaut
  * Métadonnées personnalisées (si spécifiées)
  * Métadonnées filtrées (basées sur les clés omises)

## Manipulation des métadonnées

Deux façons de personnaliser les métadonnées:

1. ** métadonnées supplémentaires **
   * Ajouter de nouveaux champs de métadonnées via JSON
   * Fusionné avec les métadonnées existantes
   * Utile pour ajouter un suivi ou une catégorisation personnalisée
2. ** omettre les clés de métadonnées **
   * Retirer les champs de métadonnées indésirables
   * Liste de clés séparée par des virgules pour exclure
   * Prise en charge de la suppression des clés imbriqués
   * Utiliser \ * pour supprimer toutes les métadonnées par défaut

## Conseils d'utilisation

* Fournir des requêtes de recherche spécifiques pour de meilleurs résultats
* Utilisez des paramètres personnalisés pour les configurations de recherche avancées
* Envisagez d'utiliser des séparateurs de texte pour les grands résultats de recherche
* Gérer les métadonnées pour conserver les informations pertinentes
* Manipuler les limites de taux grâce à l'espacement des requêtes approprié

## Notes

* Nécessite la clé de l'API Searchapi
* Respecte les limites de taux de l'API
* Prend en charge plusieurs moteurs de recherche
* Résultats de recherche en temps réel
* Traitement économe en mémoire
* Gestion des erreurs pour les demandes d'API
