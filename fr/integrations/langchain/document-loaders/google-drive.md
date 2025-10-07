# Google Drive

<gigne> <img src = "../../../. GitBook / Assets / Image (282) .png" alt = "" width = "317"> <Figcaption> </ Figcaption> </ Figure>

Google Drive est un service de stockage et de synchronisation de fichiers cloud. Ce module fournit des fonctionnalités pour charger et traiter les fichiers de Google Drive, prenant en charge divers formats de fichiers et Google Workspace Documents.

Ce module fournit un chargeur de documents Google Drive sophistiqué qui peut:

* Chargez plusieurs types de fichiers
* Traiter les documents Google Workspace
* Gérer le chargement basé sur les dossiers
* Soutenir les lecteurs partagés
* Traiter les fichiers récursivement
* Personnaliser le filtrage des types de fichiers
* Gérer l'authentification OAuth2

### Paramètres requis

* ** Connectez les informations d'identification **: Google Drive OAuth2 Informations d'identification. Se référer à[#Google Drive](../tools/google-drive.md)
* ** Sélectionnez des fichiers ** ou ** ID de dossier **: Choisissez des fichiers spécifiques ou fournissez un ID de dossier

### Paramètres facultatifs

* ** Types de fichiers **: Types de fichiers à charger:
  * Google Docs
  * Feuilles Google
  * Glissements Google
  * Fichiers pdf
  * Fichiers texte
  * Documents de mots
  * PowerPoint
  * Fichiers Excel
* ** Inclure les sous-dossiers **: les fichiers de processus dans les sous-dossiers
* ** Inclure les lecteurs partagés **: Accès aux fichiers à partir de lecteurs partagés
* ** Fichiers max **: nombre maximum de fichiers à charger (par défaut: 50)
* ** Splitter du texte **: un séparateur de texte pour traiter le contenu extrait
* ** Metadata supplémentaires **: objet JSON avec métadonnées supplémentaires
* ** omettre les clés de métadonnées **: Liste des clés de métadonnées séparées par des virgules pour omettre

## Sorties

* ** Document **: tableau d'objets de document contenant des métadonnées et un conceptent
* ** Texte **: chaîne concaténée du conceptent de documents

## Types de fichiers pris en charge

### Google Workspace

* Google Docs (application / vnd.google-apps.document)
* Google Sheets (application / vnd.google-apps.spreadsheet)
* Google Slides (application / vnd.google-apps.presentation)

### Microsoft Office

* Word (.docx)
* Excel (.xlsx)
* PowerPoint (.pptx)

### Autres formats

* PDF (.pdf)
* Fichiers texte (.txt)

## Caractéristiques

* Authentification OAuth2
* Prise en charge du type de fichier multiple
* Traitement des dossiers
* Accès au lecteur partagé
* Filtrage de type de fichier
* Support de division de texte
* Personnalisation des métadonnées
* Gestion des erreurs

## Méthodes de chargement

### Mode de sélection de fichiers

* Sélection directe des fichiers
* Prise en charge des fichiers multiples
* Filtrage de type de fichier
* Conservation des métadonnées

### Mode de dossier

* Traitement des dossiers récursifs
* Support de sous-dossier
* Filtrage de type de fichier
* Traitement par lots

## Structure de document

Chaque document contient:

* ** PageContent **: Extrait du contenu du fichier
* ** Metadata **:
  * Nom de fichier: nom de fichier d'origine
  * FileType: Type MIME
  * FileID: ID de fichier Google Drive
  * Source: chemin de fichier / URL
  * Métadonnées personnalisées supplémentaires

## Notes

* Nécessite une authentification OAuth2
* Les manches limites de taux
* Prend en charge les grands fichiers
* Gestion temporaire des fichiers
* Traitement économe en mémoire
* Gestion des erreurs pour les fichiers non valides
* Rafraîchissement automatique des jetons
