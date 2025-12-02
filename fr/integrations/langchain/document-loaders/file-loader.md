# Déposer

<gigne> <img src = "../../../. Gitbook / Assets / image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (2) .png" alt = "" width = "282"> <Figcaption> </figcaption>

Le chargeur de fichiers est un chargeur de document polyvalent qui prend en charge plusieurs formats de fichiers, notamment TXT, JSON, CSV, DOCX, PDF, Excel, PowerPoint, etc. Ce module fournit une interface unifiée pour le chargement et le traitement de divers types de fichiers.

Ce module fournit un chargeur de fichiers sophistiqué qui peut:

* Chargez plusieurs formats de fichiers
* Prise en charge des fichiers et fichiers codés en base de base64 à partir du stockage
* Gérer les options de traitement spécifiques au PDF
* Traiter JSON et JSONL avec l'extraction du pointeur
* Prise en charge du fractionnement du texte
* Personnaliser l'extraction des métadonnées
* Gérer l'intégration du stockage des fichiers

## Entrées

### Paramètres requis

* ** Fichier **: le (s) fichier (s) à traiter (prend en charge plusieurs formats)

### Paramètres facultatifs

* ** Splitter du texte **: un séparateur de texte pour traiter le contenu extrait
* ** Utilisation du PDF **: Choisissez entre:
  * Un document par page
  * Un document par fichier
* ** Utilisez le héritage Build **: Utilisez la construction héritée pour les problèmes de compatibilité PDF
* ** Extraction du pointeur JSONL **: Nom du pointeur pour les fichiers JSONL
* ** Metadata supplémentaires **: objet JSON avec métadonnées supplémentaires
* ** omettre les clés de métadonnées **: Liste des clés de métadonnées séparées par des virgules pour omettre

## Sorties

* ** Document **: tableau d'objets de document contenant des métadonnées et un conceptent
* ** Texte **: chaîne concaténée du conceptent de documents

## Types de fichiers pris en charge

* Fichiers texte (.txt)
* Fichiers JSON (.json)
* Fichiers JSONL (.jsonl)
* Fichiers CSV (.csv)
* Fichiers PDF (.pdf)
* Documents de mots (.docx)
* Files Excel (.xlsx, .xls)
* Fichiers PowerPoint (.pptx, .ppt)
* Et plus ...

## Caractéristiques

* Support multi-format
* Intégration de stockage
* Options de traitement PDF
* Extraction du pointeur JSON
* Support de division de texte
* Personnalisation des métadonnées
* Gestion des erreurs
* Détection de type mime

## Options de traitement de fichiers

### Traitement PDF

* Fractionnement par page
* Mode de document unique
* Support de construction hérité
* Compatibilité OCR

### Traitement JSON / JSONL

* Extraction basée sur le pointeur
* Gestion des données structurées
* Traitement du tableau
* Support d'objet imbriqué

## Notes

* Détecte automatiquement le type de fichier
* Gère plusieurs fichiers simultanément
* Prend en charge l'intégration du stockage de fichiers
* Préserve les métadonnées du fichier
* Gère efficacement les fichiers volumineux
* Gestion des erreurs pour les fichiers non valides
* Traitement économe en mémoire
