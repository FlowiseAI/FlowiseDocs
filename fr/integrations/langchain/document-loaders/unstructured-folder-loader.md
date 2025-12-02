---
description: >-
  Use Unstructured.io to load data from a folder. Note: Currently doesn't
  support .png and .heic until unstructured is updated.
---

# Chargeur de dossiers non structurés

<gigne> <img src = "../../../. GitBook / Assets / Image (101) .png" alt = "" width = "320"> <FigCaption> <p> Node de chargeur de dossier non structuré </p> </gigcaption> </piguré> non structuré

Le chargeur de dossier non structuré utilise[Unstructured.io](https://unstructured.io)Pour charger et traiter plusieurs documents à partir d'un dossier. Il fournit des capacités d'analyse de document avancées avec des options de configuration étendues pour l'OCR, le groupe et l'extraction des métadonnées.

{% hint style = "avertissement"%}
Actuellement, ne prend pas en charge les fichiers .png et .heic jusqu'à la mise à jour non structurée.
{% EndHint%}

## Caractéristiques
- Traitement par lots de plusieurs documents
- Plusieurs stratégies de traitement
- Support OCR avec plus de 15 langues
- Stratégies de section flexibles
- Inférence de la structure du tableau
- Options de traitement XML
- Gestion de la pause de la page
- Coordonnée
- Personnalisation des métadonnées

## Configuration

### Configuration de l'API
- URL de l'API par défaut:`http://localhost:8000/general/v0/general`
- Peut être configuré via une variable d'environnement:`UNSTRUCTURED_API_URL`
- Authentification des clés API facultative

## Paramètres

### Paramètres requis
- ** Path de dossier **: Chemin vers le dossier contenant des documents à traiter

### Paramètres facultatifs

#### Configuration de base
- ** URL API non structurée **: point de terminaison de l'API (par défaut: http: // localhost: 8000 / général / v0 / général)
- ** Stratégie **: stratégie de traitement (par défaut: auto)
  - HI_RES: traitement haute résolution
  - rapide: traitement rapide
  - OCR_ONLY: Traitement axé sur l'OCR
  - Auto: sélection automatique
- ** Encodage **: Encodage du document (par défaut: UTF-8)

#### Options OCR
- ** Langues OCR **: Support linguistique multiple, y compris:
  - Anglais (Eng)
  - Espagnol (spa)
  - Mandarin Chinois (CMN)
  - Hindi (hin)
  - Arabe (ARA)
  - Portugais (POR)
  - Bengali (Ben)
  - Russe (RUS)
  - Japonais (JPN)
  - Et plus ...

#### Options de traitement
- ** Les types de tables de saut de déduction **: Types de fichiers pour sauter l'extraction de la table (par défaut: ["PDF", "JPG", "PNG"])
- ** Nom du modèle HI-RES **: Sélection du modèle pour la stratégie HI_RES (par défaut: Detectron2_onnx)
  - Chipper: modèle VDU interne de non structuré
  - Detectron2_onnx: la détection rapide d'objets de Facebook AI
  - Yolox: détecteur en temps réel à un étage
  - yolox_quantized: version optimisée yolox
- ** Coordonnées **: Extraire les coordonnées des éléments (par défaut: false)
- ** Inclure les pauses de page **: Inclure des éléments de pause de page
- ** Tags de conservation XML **: préserver les balises XML
- ** Sections de plusieurs pages **: gérer les sections de plusieurs pages

#### Options de section texte
- ** Stratégie de section **: Méthode de bunking texte (par défaut: by_title)
  - Aucun: pas de bond
  - by_title: Chunk by Document titres
- ** Mélanger sous N Chars **: Taille minimale de morceaux
- ** NOUVEAU APRÈS N Chars **: Taille de morceau maximum doux
- ** Caractères max **: Taille du morceau maximum dur (par défaut: 500)

#### Options de métadonnées
- ** Clé ID source **: clé pour l'identification de la source du document (par défaut: source)
- ** Metadata supplémentaires **: métadonnées personnalisées en tant que JSON
- ** omettre les métadonnées clés **: clés pour exclure des métadonnées

## Types de fichiers pris en charge
- Documents: .doc, .docx, .odt, .ppt, .pptx, .pdf
- Feuilles de calcul: .xls, .xlsx
- Texte: .txt, .Text, .md, .rtf
- Web: .html, .htm
- Courriel: .eml, .msg
- Images: .jpg, .jpeg (Remarque: .png et .heic actuellement non pris en charge)

## Structure de sortie

### Format de document
Chaque document traité comprend:
- ** PageContent **: Contenu texte extrait
- ** Metadata **:
  - Source: Document Source Identifier
  - Métadonnées supplémentaires du traitement
  - Métadonnées personnalisées (si spécifiées)

## Exemples d'utilisation

### Configuration de base
```json
{
  "folderPath": "/path/to/documents",
  "strategy": "auto",
  "encoding": "utf-8"
}
```

### Traitement avancé
```json
{
  "folderPath": "/path/to/documents",
  "strategy": "hi_res",
  "hiResModelName": "detectron2_onnx",
  "ocrLanguages": ["eng", "spa", "fra"],
  "chunkingStrategy": "by_title",
  "maxCharacters": 500,
  "coordinates": true,
  "metadata": {
    "source": "company_docs",
    "department": "legal"
  }
}
```

## Meilleures pratiques
1. Choisissez une stratégie appropriée basée sur la qualité des documents et les besoins de traitement
2. Configurer les langages OCR en fonction du contenu du document
3. Ajustez les paramètres de segment pour la segmentation optimale de texte
4. Utilisez un modèle haute résolution approprié pour votre cas d'utilisation
5. Considérez l'utilisation de la mémoire lors du traitement de grands dossiers
6. Surveiller l'utilisation et les temps de réponse de l'API
7. Gérer les erreurs d'API potentielles dans votre flux de travail

## Notes
- Traiter plusieurs documents en lot
- Prend en charge divers formats de fichiers
- Traitement économe en mémoire
- Gestion automatique des métadonnées
- Formats de sortie flexibles
- Gestion des erreurs pour les réponses API
- Options de traitement configurables

{% hint style = "info"%}
Cette section est un travail en cours. Nous apprécions toute aide que vous pouvez fournir pour terminer cette section. Veuillez vérifier notre[Contribution Guide](broken-reference)Pour commencer.
{% EndHint%}
