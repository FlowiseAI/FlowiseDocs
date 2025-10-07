---
description: Load data from JSON files.
---

# Fichier json

<gigne> <img src = "../../../. GitBook / Assets / Image (12) (1) (1) (1) (2) .png" alt = "" width = "259"> <Figcaption> <p> JSON Node de fichier </p> </ Figcaption> </figne>

JSON (Notation d'objet JavaScript) est un format d'interchange de données léger qui est facile à lire et à écrire pour les humains et facile à analyser et à générer. Ce module fournit des fonctionnalités avancées pour charger et traiter les fichiers JSON dans votre flux de travail.

Ce module fournit un chargeur de documents JSON sophistiqué qui peut:

* Chargez des fichiers JSON simples ou multiples
* Prise en charge des fichiers et fichiers codés en base de base64 à partir du stockage
* Extraire des données spécifiques à l'aide de pointeurs JSON
* Manipuler l'extraction des métadonnées dynamiques
* Traiter les structures JSON imbriquées

## Entrées

* ** Fichier JSON **: le (s) fichier (s) JSON (.
* ** Splitter de texte ** (facultatif): un séparateur de texte pour traiter le contenu extrait
* ** Extraction des pointeurs ** (Facultatif): Liste de pointeurs JSON séparée par des virgules pour extraire des données spécifiques
* ** Métadonnées supplémentaires ** (Facultatif): objet JSON pour l'extraction des métadonnées dynamiques du document
* ** omettre les clés de métadonnées ** (facultatif): liste de clés de métadonnées séparées par des virgules pour omettre à partir des métadonnées par défaut

## Sorties

* ** Document **: tableau d'objets de document contenant des métadonnées et un conceptent
* ** Texte **: chaîne concaténée du conceptent de documents

## Caractéristiques

* Support de traitement de fichiers multiples
* Extraction de données basée sur le pointeur JSON
* Cartographie des métadonnées dynamiques
* Manipulation de la structure JSON imbriquée
* Prise en charge de l'intégration du stockage
* Capacités de manutention de base64 et de blob

## Exemple d'utilisation

Pour un document JSON comme:

```json
[
    {
        "url": "https://www.google.com",
        "body": "This is body 1"
    },
    {
        "url": "https://www.yahoo.com",
        "body": "This is body 2"
    }
]
```

Vous pouvez extraire des champs spécifiques sous forme de métadonnées en utilisant:

```json
{
    "source": "/url"
}
```

Cela ajoutera la valeur d'URL sous forme de métadonnées avec la clé "source" pour chaque document.
