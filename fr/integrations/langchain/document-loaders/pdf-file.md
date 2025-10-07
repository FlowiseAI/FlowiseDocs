# Chargeur de documents PDF

PDF (Format de document portable) est un format de fichier développé par Adobe pour présenter des documents de manière cohérente sur les plateformes logicielles. Ce module fournit des fonctionnalités pour charger et traiter les fichiers PDF à l'aide de pdf.js.

Ce module fournit un chargeur de document PDF sophistiqué qui peut:
- Chargez des fichiers PDF uniques ou multiples
- Diviser les documents par page ou fichier
- Prise en charge des fichiers codés Base64
- Gérer l'intégration du stockage des fichiers
- Traiter le contenu avec des séparateurs de texte
- Prise en charge des versions PDF héritées
- Personnaliser l'extraction des métadonnées

## Entrées

### Paramètres requis
- ** Fichier PDF **: le (s) fichier (s) PDF à traiter (extension .pdf)
- ** Utilisation **: Choisissez entre:
  - Un document par page
  - Un document par fichier

### Paramètres facultatifs
- ** Splitter du texte **: un séparateur de texte pour traiter le contenu extrait
- ** Utilisez le héritage Build **: s'il faut utiliser le héritage pdf.js build
- ** Metadata supplémentaires **: objet JSON avec métadonnées supplémentaires
- ** omettre les clés de métadonnées **: Liste des clés de métadonnées séparées par des virgules pour omettre

## Sorties

- ** Document **: tableau d'objets de document contenant des métadonnées et un conceptent
- ** Texte **: chaîne concaténée du conceptent de documents

## Caractéristiques
- Prise en charge des fichiers multiples
- Division au niveau de la page
- Prise en charge de la version héritée
- Extraction de texte
- Manipulation des métadonnées
- Gestion des erreurs
- Traitement économe en mémoire

## Modes de traitement

### En mode page
- Chaque page devient un document
- Conserve les numéros de page
- Métadonnées de page individuelles
- Accès de contenu granulaire

### Par mode de fichier
- PDF entier en un seul document
- Contenu combiné
- Ensemble de métadonnées uniques
- Mémoire efficace

## Structure de document
Chaque document contient:
- ** PageContent **: Contenu texte extrait
- ** Metadata **:
  - Source: chemin d'origine du fichier
  - PDF: métadonnées spécifiques au PDF
  - Page: numéro de page (en mode par page)
  - Métadonnées personnalisées supplémentaires

## Manutention de fichiers

### Fichiers locaux
- Chargement de fichier direct
- Base64 Contenu codé
- Prise en charge des fichiers multiples

### Intégration de stockage
- Prise en charge du système de stockage de fichiers
- Stockage basé sur l'organisation
- Stockage basé sur Chatflow

## Notes
- Utilise pdf.js pour l'extraction
- Prise en charge de la version héritée
- Traitement économe en mémoire
- Gestion des erreurs pour les fichiers non valides
- Prise en charge des grands PDF
- Formats de sortie flexibles
- Personnalisation des métadonnées
- Manipulation du codage de texte
