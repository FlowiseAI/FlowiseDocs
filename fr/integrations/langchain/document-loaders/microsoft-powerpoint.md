# Chargeur de documents Microsoft PowerPoint

<gigne> <img src = "../../../. GitBook / Assets / Image (286) .png" alt = "" width = "234"> <Figcaption> </gigcaption> </ figure>

Microsoft PowerPoint est un programme de présentation pour la création et l'affichage des diapositives. Ce module fournit des fonctionnalités pour charger et traiter les fichiers PowerPoint à l'aide de OfficePaSer.

Ce module fournit un chargeur de document PowerPoint sophistiqué qui peut:
- Chargez des présentations PowerPoint
- Extraire du texte des diapositives
- Diviser le contenu en diapositives individuelles
- Numéro de la diapositive
- Traiter les métadonnées par diapositive
- Prise en charge de plusieurs formats de diapositive
- Gérer divers séparateurs de diapositives

## Entrées

### Paramètres requis
- ** Fichier PowerPoint **: le (s) fichier (s) PowerPoint (.ppt, .pptx)

### Paramètres facultatifs
- ** Splitter du texte **: un séparateur de texte pour traiter le contenu extrait
- ** Metadata supplémentaires **: objet JSON avec métadonnées supplémentaires
- ** omettre les clés de métadonnées **: Liste des clés de métadonnées séparées par des virgules pour omettre

## Sorties

- ** Document **: tableau d'objets de document contenant des métadonnées et un conceptent
- ** Texte **: chaîne concaténée du conceptent de documents

## Caractéristiques
- Extraction de texte
- Séparation de diapositives
- Manipulation des métadonnées
- Gestion des erreurs
- Traitement économe en mémoire
- Détection de diapositives heuristiques
- Filtrage de contenu

## Méthodes de détection de diapositives

### Reconnaissance des modèles
Le chargeur tente d'identifier les diapositives en utilisant des modèles communs:
- Marqueurs "Slide X"
- Marqueurs "page X"
- Numéros de page "x / y"
- Souligneurs de soulignement
- Séparateurs de tableau de bord
- Plusieurs lignes de newlines

### Mécanismes de secours
Si la reconnaissance des modèles échoue:
1. Split by Double Newlines
2. Traitez le contenu comme une seule diapositive

## Structure de document
Chaque document contient:
- ** PageContent **: Contenu du texte extrait de la diapositive
- ** Metadata **:
  - SlideNumber: numéro de diapositif séquentiel
  - DocumentType: "PowerPoint"
  - Métadonnées personnalisées supplémentaires

## Traitement du contenu
- Les lames vides sont filtrées
- Espace blanc de tête / traînage supprimé
- Validation minimale de la longueur du contenu
- Validation du nombre de diapositives raisonnable

## Attributs de métadonnées
Les attributs par défaut incluent:
- SlideNumber: Numéro de diapositive (numéro)
- DocumentType: Type de document (chaîne)
- Métadonnées personnalisées à partir de l'entrée

## Notes
- Utilise OfficeArser pour l'extraction
- Gère divers formats de diapositive
- Détection de diapositive intelligente
- Validation du contenu
- Traitement économe en mémoire
- Gestion des erreurs pour les fichiers non valides
- Formats de sortie flexibles
- Mécanismes de secours robustes
