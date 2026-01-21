---
description: Load data from DOCX files.
---

# Fichier docx

<gigne> <img src = "../../../. GitBook / Assets / Image (7) (1) (1) (1) (1) (1) (1) (1) (2) .png" alt = "" width = "269"> <figcaption> <p> docx file node </p> </igcaption> </gigne>

Microsoft Word Document (DOCX) est un format de document largement utilisé pour la création et l'édition de documents texte. Ce module fournit des fonctionnalités pour charger et traiter les fichiers docx dans votre workflow.

Ce module fournit un chargeur de document DOCX complet qui peut:

* Chargez des fichiers Docx uniques ou multiples
* Prise en charge des fichiers et fichiers codés en base de base64 à partir du stockage
* Extraire le contenu du texte avec des métadonnées
* Intégrer avec des séparateurs de texte pour le traitement du contenu
* Gérer la gestion des métadonnées personnalisées

## Entrées

* ** Fichier Docx **: le (s) fichier (s) DOCX à traiter (extension .docx requise)
* ** Splitter de texte ** (facultatif): un séparateur de texte pour traiter le contenu extrait
* ** Métadonnées supplémentaires ** (Facultatif): objet JSON avec des métadonnées supplémentaires à ajouter aux documents
* ** omettre les clés de métadonnées ** (facultatif): liste de clés de métadonnées séparées par des virgules pour omettre à partir des métadonnées par défaut

## Sorties

* ** Document **: tableau d'objets de document contenant des métadonnées et un conceptent
* ** Texte **: chaîne concaténée du conceptent de tous les documents

## Caractéristiques

* Support de traitement de fichiers multiples
* Options de division de texte flexible
* Manipulation des métadonnées personnalisables
* Prise en charge de l'intégration du stockage
* Capacités de manutention de base64 et de blob
