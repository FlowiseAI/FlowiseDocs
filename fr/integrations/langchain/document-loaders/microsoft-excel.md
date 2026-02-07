# Chargeur de documents Microsoft Excel

<gigne> <img src = "../../../. GitBook / Assets / Image (285) .png" alt = "" width = "238"> <Figcaption> </ Figcaption> </ Figure>

Microsoft Excel est un programme de feuille de calcul qui comprend des outils de calcul, des tables de pivot et un langage de programmation macro. Ce module fournit des fonctionnalités pour charger et traiter les fichiers Excel à l'aide de sheetjs.

Ce module fournit un chargeur de documents Excel sophistiqué qui peut:
- Chargez plusieurs formats de fichiers Excel
- Traiter plusieurs feuilles de calcul
- Convertir les lignes en documents structurés
- Gérer divers types de données
- Conserver la mise en forme des cellules
- Extraire les métadonnées par rangée
- Inférence du type de support

## Entrées

### Paramètres requis
- ** Fichier Excel **: le (s) fichier (s) pour traiter (.xls, .xlsx, .xlsm, .xlsb)

### Paramètres facultatifs
- ** Splitter du texte **: un séparateur de texte pour traiter le contenu extrait
- ** Metadata supplémentaires **: objet JSON avec métadonnées supplémentaires
- ** omettre les clés de métadonnées **: Liste des clés de métadonnées séparées par des virgules pour omettre

## Sorties

- ** Document **: tableau d'objets de document contenant des métadonnées et un conceptent
- ** Texte **: chaîne concaténée du conceptent de documents

## Caractéristiques
- Support de format multiple
- Traitement à plusieurs feuilles
- Préservation du type de données
- Extraction de métadonnées
- Type d'inférence
- Gestion des erreurs
- Traitement économe en mémoire

## Formats pris en charge
- Excel binaire (.xls)
- Excel Workbook (.xlsx)
- Excel Macro-compatible (.xlsm)
- Excel Binary Workbook (.xlsb)

## Traitement des types de données

### Types pris en charge
- Texte (chaîne)
- Nombres (numéro)
- Dates (date)
- Booléens (booléen)
- Formules (valeurs calculées)
- Cellules vides (null)

## Structure de document
Chaque document contient:
- ** PageContent **: Contenu de ligne formaté comme paires de valeurs clés
- ** Metadata **:
  - feuille de travail: nom de feuille
  - Rownum: indice de ligne
  - Valeurs de colonne d'origine
  - Métadonnées personnalisées supplémentaires

## Traitement des lignes
Chaque ligne est convertie en document avec:
- Paires de valeurs clés pour chaque cellule
- En-têtes de colonne conservées
- Type d'information
- Position

## Attributs de métadonnées
Les attributs par défaut incluent:
- feuille de travail: feuille ou nom de feuille de travail (chaîne)
- Rownum: Index de ligne (numéro)
- Attributs dynamiques basés sur les en-têtes de colonne

## Notes
- Utilise des feuilles pour l'analyse
- Conserve les types de données
- Gère plusieurs feuilles
- Enfil Types de colonnes
- Traitement économe en mémoire
- Gestion des erreurs pour les fichiers non valides
- Formats de sortie flexibles
- Inférence de type colonne
