# Texte brut

<gigne> <img src = "../../../. Gitbook / Assets / image (5) (1) (1) (1) (1) (1) (1) (1) (1) (2) .png" alt = "" width = "263"> <figcaption>

Le texte brut est la forme la plus élémentaire de données de texte, ne contenant pas de formatage ou d'autres informations intégrées. Ce module fournit des fonctionnalités pour charger et traiter directement le contenu de texte brut.

Ce module fournit un chargeur de document texte simple qui peut:

* Chargez directement le contenu du texte
* Traiter le texte avec des séparateurs
* Ajouter des métadonnées personnalisées
* Gérer les caractères d'évasion
* Prise en charge du fractionnement du document
* Personnaliser l'extraction des métadonnées
* Gérer l'encodage du texte

## Entrées

### Paramètres requis

* ** Texte **: le contenu en texte brut à traiter

### Paramètres facultatifs

* ** Splitter de texte **: un séparateur de texte pour traiter le contenu
* ** Metadata supplémentaires **: objet JSON avec métadonnées supplémentaires
* ** omettre les clés de métadonnées **: Liste des clés de métadonnées séparées par des virgules pour omettre

## Sorties

* ** Document **: tableau d'objets de document contenant des métadonnées et un conceptent
* ** Texte **: chaîne concaténée du conceptent de documents

## Caractéristiques

* Entrée de texte direct
* Support de division de texte
* Manipulation des métadonnées
* Gestion des erreurs
* Traitement économe en mémoire
* Traitement de codage de caractère
* Formats de sortie flexibles

## Traitement du texte

### Mode direct

* Création de documents uniques
* Conserve le texte original
* Manipulation de métadonnées de base
* Mémoire efficace

### Mode partagé

* Création de documents multiples
* Règles de division personnalisées
* Métadonnées individuelles
* Accès de contenu granulaire

## Structure de document

Chaque document contient:

* ** PageContent **: Contenu du texte original ou divisé
* ** Metadata **:
  * Métadonnées personnalisées à partir de l'entrée
  * Métadonnées spécifiques divisées (lors de l'utilisation du séparateur)
  * Propriétés de métadonnées supplémentaires

## Traitement du contenu

### Entrée de texte

* Entrée directe de chaîne
* Support multi-lignes
* Support Unicode
* Échapper à la manipulation des personnages

### Options de traitement

* Division de texte
* Ajout de métadonnées
* Normalisation du caractère
* Manipulation des espaces

## Notes

* Simple et efficace
* Aucune gestion de fichier requise
* Traitement économe en mémoire
* Gestion des erreurs pour les entrées non valides
* Prise en charge des grands textes
* Formats de sortie flexibles
* Personnalisation des métadonnées
* Caractère Encoding Support
