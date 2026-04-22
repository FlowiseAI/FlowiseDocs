# Chargeur de fichiers EPUB

Epub (publication électronique) est une norme de livre électronique gratuite et ouverte par le Forum international de publication numérique (IDPF). Ce module fournit des fonctionnalités pour charger et traiter les fichiers EPUB dans votre flux de travail.

Ce module fournit un chargeur de documents EPUB sophistiqué qui peut:
- Chargez des fichiers EPUB simples ou multiples
- Prise en charge des fichiers et fichiers codés en base de base64 à partir du stockage
- Extraire du contenu par chapitre ou par fichier
- Traiter le contenu avec des séparateurs de texte
- Gérer l'extraction des métadonnées
- Gérer le traitement de fichiers temporaire

## Entrées

### Paramètres requis
- ** Fichier EPUB **: le (s) fichier (s) pour traiter (extension .pub requis)
- ** Utilisation **: Choisissez entre:
  - Un document par chapitre: Contenu divisé par les chapitres
  - Un document par fichier: traiter le fichier entier comme un seul document

### Paramètres facultatifs
- ** Splitter du texte **: un séparateur de texte pour traiter le contenu extrait
- ** Metadata supplémentaires **: objet JSON avec métadonnées supplémentaires
- ** omettre les clés de métadonnées **: Liste des clés de métadonnées séparées par des virgules pour omettre

## Sorties

- ** Document **: tableau d'objets de document contenant des métadonnées et un conceptent
- ** Texte **: chaîne concaténée du conceptent de documents

## Caractéristiques
- Traitement de fichiers multiples
- Division au niveau du chapitre
- Traitement au niveau des fichiers
- Intégration de stockage
- Personnalisation des métadonnées
- Support de division de texte
- Traitement des fichiers temporaires
- Gestion des erreurs

## Modes de traitement

### Par mode chapitre
- Crée des documents distincts pour chaque chapitre
- Maintient la structure du chapitre
- Conserve les métadonnées du chapitre
- Mieux pour l'analyse détaillée

### Par mode de fichier
- Traite le fichier entier comme un seul document
- Maintient la structure globale
- Organisation de documents plus simples
- Mieux pour l'analyse de la vue d'ensemble

## Notes
- Prend en charge les fichiers locaux et basés sur le stockage
- Gère le contenu codé de base64
- Nettoie automatiquement les fichiers temporaires
- Préserve la structure du document
- Prend en charge l'ajout de métadonnées personnalisées
- Gestion des erreurs pour les fichiers non valides
- Traitement économe en mémoire