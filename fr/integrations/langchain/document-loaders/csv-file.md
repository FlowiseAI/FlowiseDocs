---
description: Load data from CSV files.
---

# Fichiers CSV

<gigne> <img src = "../../../. GitBook / Assets / Image_CSV (1) .png" alt = "" width = "271"> <Figcaption> <p> CSV Node de fichier </p> </ figCaption> </ Figure>

CSV (valeurs séparées par les virgules) est un format de fichier simple utilisé pour stocker des données tabulaires, telles qu'une feuille de calcul ou une base de données. Ce module fournit des fonctionnalités pour charger et traiter les fichiers CSV dans votre flux de travail.

Ce module fournit un chargeur de document CSV polyvalent qui peut:
- Chargez des fichiers CSV uniques ou multiples
- Prise en charge des fichiers et fichiers codés en base de base64 à partir du stockage
- Extraire des colonnes spécifiques ou du contenu entier
- Traiter efficacement de grands ensembles de données
- Gérer la gestion des métadonnées personnalisées

## Entrées

- ** Fichier CSV **: le (s) fichier (s) CSV (S) à traiter (extension .csv requise)
- ** Splitter de texte ** (facultatif): un séparateur de texte pour traiter le contenu extrait
- ** Extraction à colonne unique ** (facultatif): nom d'une colonne spécifique à extraire
- ** Métadonnées supplémentaires ** (Facultatif): objet JSON avec des métadonnées supplémentaires à ajouter aux documents
- ** omettre les clés de métadonnées ** (facultatif): liste de clés de métadonnées séparées par des virgules pour omettre à partir des métadonnées par défaut

## Sorties

- ** Document **: tableau d'objets de document contenant des métadonnées et un conceptent
- ** Texte **: chaîne concaténée du conceptent de tous les documents

## Caractéristiques
- Support de traitement de fichiers multiples
- Capacité d'extraction à colonne unique
- Gestion efficace des grands ensembles de données
- Manipulation des métadonnées personnalisables
- Prise en charge de l'intégration du stockage
- Capacités de manutention de base64 et de blob
