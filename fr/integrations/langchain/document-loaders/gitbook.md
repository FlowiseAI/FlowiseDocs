---
description: Load data from GitBook.
---

# Gitbook

<gigne> <img src = "../../../. GitBook / Assets / Image (74) .png" alt = "" width = "270"> <figcaption> <p> gitbook nœud </p> </gigcaption> </ figure>

# Chargeur de documents gitbook

GitBook est une plate-forme de documentation moderne qui aide les équipes à partager les connaissances. Ce module fournit des fonctionnalités pour charger et traiter le contenu à partir des sites de documentation GitBook.

Ce module fournit un chargeur de documents Gitbook sophistiqué qui peut:
- Chargez le contenu à partir de pages de gitbook spécifiques
- Crawl des sites de documentation entièrement gitbook
- Extrait de contenu structuré
- Traiter le contenu avec des séparateurs de texte
- Personnaliser l'extraction des métadonnées
- Gérer le chargement récursif de la page

## Entrées

### Paramètres requis
- ** Chemin Web **: L'URL vers la page Gitbook ou le chemin racine
  - Page unique: par exemple, https://docs.gitbook.com/product-tour/navigation
  - Chemin racine: par exemple, https://docs.gitbook.com/

### Paramètres facultatifs
- ** devrait charger tous les chemins **: s'il faut charger de manière récursivement toutes les pages du chemin racine
- ** Splitter du texte **: un séparateur de texte pour traiter le contenu extrait
- ** Metadata supplémentaires **: objet JSON avec métadonnées supplémentaires
- ** omettre les clés de métadonnées **: Liste des clés de métadonnées séparées par des virgules pour omettre

## Sorties

- ** Document **: tableau d'objets de document contenant des métadonnées et un conceptent
- ** Texte **: chaîne concaténée du conceptent de documents

## Caractéristiques
- Chargement d'une page
- Site récursif rampant
- Extraction de contenu
- Support de division de texte
- Personnalisation des métadonnées
- Gestion des erreurs
- Gestion des chemins

## Modes de chargement

### Mode de page unique
- Charge le contenu d'une page spécifique
- Extrait le contenu de la page et les métadonnées
- Conserve la structure de la page
- Plus rapide pour un accès à une seule page

### Tous les chemins
- Charge récursivement toutes les pages de la racine
- Maintient la hiérarchie du site
- Extrait tous les contenus disponibles
- Conserve la structure de navigation

## Structure de document
Chaque document contient:
- ** PageContent **: Extrait du contenu de la page
- ** Metadata **:
  - Titre: Titre de la page
  - URL: URL de la page d'origine
  - Métadonnées personnalisées supplémentaires

## Notes
- Prend en charge le chargement unique de la page et du site complet
- Gère le contenu dynamique de Gitbook
- Préserve la structure du document
- Prend en charge l'ajout de métadonnées personnalisées
- Gestion des erreurs pour les URL non valides
- Traitement économe en mémoire
- Formats de sortie flexibles
