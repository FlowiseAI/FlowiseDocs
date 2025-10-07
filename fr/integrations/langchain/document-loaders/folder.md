# Dossier avec chargeur de fichiers

<gigne> <img src = "../../../. Gitbook / Assets / Image (9) (1) (1) (1) (1) (1) (1) (1) .png" alt = "" width = "262"> <pigtion> <p> Folder avec fichiers nœud </p> </gigcution> </gigne>

Le chargeur de dossier fournit des fonctionnalités pour charger et traiter plusieurs fichiers à partir d'un répertoire. Ce module prend en charge une large gamme de formats de fichiers et peut traiter récursivement des sous-répertoires.

Ce module fournit un chargeur de dossier sophistiqué qui peut:
- Charger plusieurs types de fichiers simultanément
- Traiter les répertoires récursivement
- Gérer divers formats de documents
- Prise en charge du traitement spécifique au PDF
- Traiter les fichiers de données structurées
- Personnaliser l'extraction des métadonnées
- Prise en charge du fractionnement du texte

## Entrées

### Paramètres requis
- ** Chemin de dossier **: Chemin vers le répertoire contenant des fichiers
- ** récursif **: s'il faut traiter les sous-répertoires

### Paramètres facultatifs
- ** Splitter du texte **: un séparateur de texte pour traiter le contenu extrait
- ** Utilisation du PDF **: Choisissez entre:
  - Un document par page
  - Un document par fichier
- ** Extraction du pointeur JSONL **: Nom du pointeur pour les fichiers JSONL
- ** Metadata supplémentaires **: objet JSON avec métadonnées supplémentaires
- ** omettre les clés de métadonnées **: Liste des clés de métadonnées séparées par des virgules pour omettre

## Sorties

- ** Document **: tableau d'objets de document contenant des métadonnées et un conceptent
- ** Texte **: chaîne concaténée du conceptent de documents

## Types de fichiers pris en charge

### Documents
- PDF (.pdf)
- Word (.doc, .docx)
- Excel (.xls, .xlsx, .xlsm, .xlsb)
- PowerPoint (.ppt, .pptx)
- Texte (.txt)
- Markdown (.md, .markdown)
- Html (.html)
- XML (.xml)

### Fichiers de données
- JSON (.json)
- JSONL (.jsonl)
- CSV (.csv)

### Langues de programmation
- Python (.py, .python)
- Javascript (.js)
- TypeScript (.ts)
- Java (.java)
- C / c ++ (.c, .cpp, .h)
- C # (.cs)
- Ruby (.rb, .Ruby)
- Aller (.go)
- Php (.php)
- Swift (.swift)
- Rust (.RS)
- Scala (.scala, .sc)
- Kotlin (.kt)
- Solidité (.sol)

### Technologies Web
- CSS (.css)
- SCSS (.SCSS)
- Moins (.
- SQL (.SQL)
- Tampons de protocole (.proto)

## Caractéristiques
- Support multi-format
- Traitement répertoire récursif
- Options de traitement PDF
- Gestion des données structurées
- Support de division de texte
- Personnalisation des métadonnées
- Gestion des erreurs

## Notes
- Détecte automatiquement les types de fichiers
- Gère les grands répertoires
- Préserve les métadonnées du fichier
- Traitement économe en mémoire
- Prend en charge les extensions de fichiers personnalisées
- Gestion des erreurs pour les fichiers non valides
- Formats de sortie flexibles