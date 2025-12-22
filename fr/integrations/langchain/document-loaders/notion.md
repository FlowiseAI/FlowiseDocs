# Notion

La notion est une plate-forme de collaboration qui combine la prise de notes, la gestion des connaissances et la gestion de projet. Ce module fournit trois chargeurs différents pour traiter le contenu de la notion: la base de données, la page et les chargeurs de dossiers.

## Chargeur de base de données de notion

<gigne> <img src = "../../../. Gitbook / Assets / image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (2) .png" Alt = "" width = "260"> <Figcaption> <p> notion database notion notion database notion notion DATAbase notion notion notion de notion notion de notion de notion notion de notion de notion de notion de notion de notion de notion de notion de notion de notion de notion de notion de notion Nœud </p> </gigcaption> </ figure>

Le chargeur de base de données extrait le contenu des bases de données de notion, traitant chaque ligne comme un document distinct.

### Caractéristiques

* Chargez les lignes de la base de données comme documents
* Extraire les propriétés sous forme de métadonnées
* Soutenir les en-têtes de propriété
* Gérer le chargement simultané
* Traiter le contenu avec des séparateurs de texte
* Personnaliser l'extraction des métadonnées

### Paramètres requis

* ** Connectez les informations d'identification **: notion API
* ** ID de base de données **: L'identifiant unique de la base de données de notion

## Chargeur de page de notion

<gigne> <img src = "../../../. GitBook / Assets / Image (4) (1) (1) (1) (1) (1) (1) (1) (1) (2) .png" alt = "" width = "262"> <figcaption> <p> Page de notion Node </p> </1/ / Figcaption>

Le chargeur de page extrait le contenu des pages de notion, y compris toutes les pages enfants en tant que documents distincts.

### Caractéristiques

* Chargez le contenu de la page sous forme de documents
* Traiter les pages enfants récursivement
* Extraire les propriétés de la page
* Gérer la hiérarchie des pages
* Prise en charge du fractionnement du texte
* Personnaliser l'extraction des métadonnées

### Paramètres requis

* ** Connectez les informations d'identification **: notion API
* ** ID de page **: l'identifiant hexadécimal à 32 caractères de l'URL de la page

## Chargeur de dossiers de notion

<gigne> <img src = "../../../. gitbook / actifs / image (3) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (2) .png" alt = "" width = "259"> <Figcaption> <p> NODE NODE </p> </pigction>

Le chargeur de dossier traite le contenu de notion exporté et dézippé à partir d'un dossier local.

### Caractéristiques

* Traiter le contenu de la notion exportée
* Gérer plusieurs pages
* Prise en charge du système de fichiers local
* Extraire le contenu de la page
* Maintenir la structure des documents
* Prise en charge du fractionnement du texte
* Personnaliser l'extraction des métadonnées

### Paramètres requis

* ** Dossier de notion **: Chemin vers le dossier de notion exporté et dézippé

## Caractéristiques communes

Tous les chargeurs de notion Support:

### Paramètres facultatifs

* ** Splitter du texte **: un séparateur de texte pour traiter le contenu extrait
* ** Metadata supplémentaires **: objet JSON avec métadonnées supplémentaires
* ** omettre les clés de métadonnées **: Liste des clés de métadonnées séparées par des virgules pour omettre

### Sorties

* ** Document **: tableau d'objets de document contenant des métadonnées et un conceptent
* ** Texte **: chaîne concaténée du conceptent de documents

## Authentification

### Authentification de l'API (base de données et chargeurs de page)

* Nécessite un jeton d'intégration de notion
* Limitation du taux d'API géré automatiquement
* Prise en charge de l'accès au niveau de l'espace de travail
* Gestion d'identification sécurisée

### Accès local (chargeur de dossiers)

* Aucune authentification requise
* Accès au système de fichiers direct
* Traiter le contenu hors ligne
* Gérer les données exportées

## Structure de document

Chaque document contient:

* ** PageContent **: Contenu texte extrait
* ** Metadata **:
  * Source: source d'origine (URL ou chemin de fichier)
  * Titre: Page ou Titre de la base de données
  * Propriétés: propriétés de notion
  * Métadonnées personnalisées supplémentaires

## Notes

* Les chargeurs d'API nécessitent une configuration d'intégration de notion
* Le chargeur de dossier a besoin de contenu exporté
* La limitation du taux géré automatiquement
* Traitement économe en mémoire
* Gestion des erreurs pour les entrées non valides
* Prise en charge des grands ensembles de données
* Formats de sortie flexibles
* Personnalisation des métadonnées
