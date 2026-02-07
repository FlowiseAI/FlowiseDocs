---
description: Load data from text files.
---

# Fichier texte

<gigne> <img src = "../../../. GitBook / Assets / Image (89) .png" alt = "" width = "322"> <figcaption> <p> Node de fichier texte </p> </gigcaption> </ figure>

Le chargeur de fichiers texte vous permet de charger et de traiter le contenu à partir de divers formats de fichiers textuels. Il prend en charge plusieurs types de fichiers et fournit des options flexibles pour la division de texte et la gestion des métadonnées.

## Caractéristiques
- Prise en charge de plusieurs formats de fichiers textuels
- Capacité de chargement de fichiers multiples
- Support de division de texte
- Manipulation des métadonnées personnalisables
- Prise en charge de l'intégration du stockage
- Gestion des fichiers Base64
- Formats de sortie multiples

## Types de fichiers pris en charge
Le chargeur prend en charge une large gamme de formats de fichiers textuels:
- Fichiers texte (.txt)
- Fichiers Web (.html, .aspx, .asp, .css)
- Langages de programmation:
  - C / c ++ (.cpp, .c, .h)
  - C # (.cs)
  - Aller (.go)
  - Java (.java)
  - JavaScript / TypeScript (.js, .ts)
  - Php (.php)
  - Python (.py, .python)
  - Ruby (.rb, .Ruby)
  - Rust (.RS)
  - Scala (.sc, .scala)
  - Solidité (.sol)
  - Swift (.swift)
  - Visual Basic (.vb)
- Marquage / style:
  - CSS / Moins / SCSS (.css, .less, .scss)
  - Markdown (.md, .markdown)
  - XML (.xml)
  - Latex (.tex, .ltx)
- Autre:
  - Tampons de protocole (.proto)
  - SQL (.SQL)
  - RST (.RST)

## Entrées

### Paramètres requis
- ** Fichier TXT **: un ou plusieurs fichiers texte à traiter
  - Accepte les fichiers du téléchargement local ou du stockage
  - Prend en charge la sélection de fichiers multiples

### Paramètres facultatifs
- ** Splitter du texte **: un séparateur de texte pour traiter le contenu extrait
- ** Métadonnées supplémentaires **: objet JSON avec des métadonnées supplémentaires à ajouter aux documents
- ** omettre les clés de métadonnées **: Liste des clés de métadonnées séparées par des virgules à exclure
  - Format:`key1, key2, key3.nestedKey1`
  - Utiliser * pour supprimer toutes les métadonnées par défaut

## Sorties

- ** Document **: tableau d'objets de document contenant:
  - métadonnées: fichiers de métadonnées et champs personnalisés
  - contenu de contenu: contenu texte extrait
- ** Texte **: chaîne concaténée de tout contenu extrait

## Structure de document
Chaque document contient:
- ** PageContent **: le contenu principal du fichier texte
- ** Metadata **:
  - Métadonnées de fichier par défaut
  - Métadonnées personnalisées supplémentaires (si spécifiées)
  - Métadonnées filtrées (basées sur les clés omises)

## Exemples d'utilisation

### Traitement de fichiers unique
```json
{
  "txtFile": "example.txt",
  "metadata": {
    "source": "local",
    "category": "documentation"
  }
}
```

### Traitement de fichiers multiples
```json
{
  "txtFile": ["doc1.txt", "doc2.md", "code.py"],
  "metadata": {
    "batch": "docs-2024",
    "processor": "text-loader"
  },
  "omitMetadataKeys": "source, timestamp"
}
```

## Intégration de stockage
Le chargeur prend en charge deux modes de source de fichiers:
1. ** Téléchargement direct **: fichiers téléchargés directement via l'interface
2. ** Intégration de stockage **: fichiers accessibles via le système de stockage
   - Format:`FILE-STORAGE::filename.txt`
   - Prend en charge l'organisation et le stockage spécifique à ChatFlow

## Notes
- Gère le traitement de fichiers unique et multiple
- Prend en charge le contenu de fichier codé Base64
- Gère automatiquement différents encodages de fichiers
- Traitement économe en mémoire des fichiers volumineux
- Conserve les métadonnées de fichier en cas de besoin
- Prend en charge le fractionnement du texte pour les grands documents
- Gère les caractères d'échappement dans le texte de sortie
- S'intègre au stockage spécifique à l'organisation

{% hint style = "info"%}
Cette section est un travail en cours. Nous apprécions toute aide que vous pouvez fournir pour terminer cette section. Veuillez vérifier notre[Contribution Guide](broken-reference)Pour commencer.
{% EndHint%}
