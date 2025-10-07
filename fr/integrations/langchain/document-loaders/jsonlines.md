# Fichier de lignes JSON

<gigne> <img src = "../../../. gitbook / actifs / image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (2) (1) .png" alt = "" width = "256"> <Figction> Nœud </p> </gigcaption> </ figure>

JSON Lines (JSONL) est un format de texte où chaque ligne est une valeur JSON valide. Ce module fournit des fonctionnalités pour charger et traiter les fichiers JSONL, avec la prise en charge de l'extraction de contenu basée sur le pointeur et de la manipulation dynamique des métadonnées.

Ce module fournit un chargeur de document JSONL sophistiqué qui peut:

* Chargez des fichiers JSONL simples ou multiples
* Extraire des valeurs spécifiques à l'aide de pointeurs JSON
* Manipuler l'extraction des métadonnées dynamiques
* Traiter le contenu avec des séparateurs de texte
* Prise en charge des fichiers codés Base64
* Gérer l'intégration du stockage des fichiers
* Personnaliser l'extraction des métadonnées

## Entrées

### Paramètres requis

* ** Fichier JSONL **: le (s) fichier JSONL (.
* ** Extraction du pointeur **: Pointeur JSON pour extraire le contenu (par exemple, "clé" pour`{"key": "value"}`)

### Paramètres facultatifs

* ** Splitter du texte **: un séparateur de texte pour traiter le contenu extrait
* ** Metadata supplémentaires **: objet JSON avec métadonnées supplémentaires
* ** omettre les clés de métadonnées **: Liste des clés de métadonnées séparées par des virgules pour omettre

## Sorties

* ** Document **: tableau d'objets de document contenant des métadonnées et un conceptent
* ** Texte **: chaîne concaténée du conceptent de documents

## Caractéristiques

* Extraction du pointeur JSON
* Gestion des métadonnées dynamiques
* Support de division de texte
* Prise en charge du fichier Base64
* Intégration de stockage de fichiers
* Gestion des erreurs
* Traitement économe en mémoire

## Extraction du pointeur JSON

### Exemple de base

Pour le contenu JSONL:

```jsonl
{"key": "value1", "source": "file1.txt"}
{"key": "value2", "source": "file2.txt"}
```

Avec le pointeur "clé", extraits: "Value1", "Value2"

### Métadonnées dynamiques

Vous pouvez extraire les valeurs sous forme de métadonnées à l'aide de pointeurs JSON:

```json
{
    "source": "/source",
    "custom": "/metadata/field"
}
```

## Structure de document

Chaque document contient:

* ** PageContent **: Extrait de contenu à l'aide du pointeur
* ** Metadata **:
  * Source: chemin d'origine du fichier
  * ligne: numéro de ligne dans le fichier
  * pointeur: pointeur JSON utilisé
  * Métadonnées dynamiques supplémentaires

## Manutention de fichiers

### Fichiers locaux

* Chargement de fichier direct
* Base64 Contenu codé
* Prise en charge des fichiers multiples

### Intégration de stockage

* Prise en charge du système de stockage de fichiers
* Stockage basé sur l'organisation
* Stockage basé sur Chatflow

## Notes

* Un document par ligne JSONL
* Les lignes JSON non valides sont ignorées
* Traitement économe en mémoire
* Gestion des erreurs pour les pointeurs non valides
* Support aux structures JSON imbriquées
* Extraction des métadonnées dynamiques
* Formats de sortie flexibles
