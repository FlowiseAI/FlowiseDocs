# Chargeur de documents Microsoft Word

<gigne> <img src = "../../../. GitBook / Assets / Image (287) .png" alt = "" width = "234"> <Figcaption> </gigcaption> </ figure>

Microsoft Word est un logiciel de traitement de texte pour créer et modifier des documents texte. Ce module fournit des fonctionnalités pour charger et traiter des documents de mots à l'aide de OfficePaSer.

Ce module fournit un chargeur de documents de mots sophistiqué qui peut:
- Documents de chargement
- Extraire le contenu du texte
- Diviser le contenu en sections
- Numéro de la page
- Traiter les métadonnées par section
- Prise en charge de plusieurs formats de section
- Gérer divers séparateurs de section

## Entrées

### Paramètres requis
- ** Fichier Word **: le (s) fichier (s) pour traiter (.doc, .docx)

### Paramètres facultatifs
- ** Splitter du texte **: un séparateur de texte pour traiter le contenu extrait
- ** Metadata supplémentaires **: objet JSON avec métadonnées supplémentaires
- ** omettre les clés de métadonnées **: Liste des clés de métadonnées séparées par des virgules pour omettre

## Sorties

- ** Document **: tableau d'objets de document contenant des métadonnées et un conceptent
- ** Texte **: chaîne concaténée du conceptent de documents

## Caractéristiques
- Extraction de texte
- Séparation de section
- Manipulation des métadonnées
- Gestion des erreurs
- Traitement économe en mémoire
- Détection de section heuristique
- Filtrage de contenu

## Méthodes de détection de section

### Reconnaissance des modèles
Le chargeur tente d'identifier les sections en utilisant des modèles communs:
- Marqueurs "page X"
- Marqueurs "Section X"
- Marqueurs "Chapitre X"
- Sections numérotées (par exemple, "1.", "2.")
- Tous les têtes de capuchons
- Séparateurs de soulignement longs
- Séparateurs de tiret long

### Mécanismes de secours
Si la reconnaissance des modèles échoue:
1. Split par plusieurs nouvelles lignes
2. Split by Double Newlines
3. Traitez le contenu comme une seule section

## Structure de document
Chaque document contient:
- ** PageContent **: Contenu texte extrait de la section
- ** Metadata **:
  - DocumentType: "Word"
  - Pagenumber: numéro de section séquentiel
  - Métadonnées personnalisées supplémentaires

## Traitement du contenu
- Les sections vides sont filtrées
- Espace blanc de tête / traînage supprimé
- Validation minimale de la longueur du contenu
- Validation du nombre de sections raisonnables

## Attributs de métadonnées
Les attributs par défaut incluent:
- DocumentType: Type de document (chaîne)
- PageCount: Nombre de pages / sections (numéro)
- Métadonnées personnalisées à partir de l'entrée

## Notes
- Utilise OfficeArser pour l'extraction
- Gère divers formats de documents
- Détection de section intelligente
- Validation du contenu
- Traitement économe en mémoire
- Gestion des erreurs pour les fichiers non valides
- Formats de sortie flexibles
- Mécanismes de secours robustes
