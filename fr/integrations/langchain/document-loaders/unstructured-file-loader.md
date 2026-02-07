---
description: Use Unstructured.io to load data from a file path.
---

# Chargeur de fichiers non structuré

<gigne> <img src = "../../../. GitBook / Assets / Image (90) .png" alt = "" width = "332"> <Figcaption> <p> Node de chargeur de fichiers non structuré </p> </ Figcaption> </ Figure>

Le chargeur de fichiers non structuré utilise[Unstructured.io](https://unstructured.io)pour extraire et traiter le contenu à partir de divers formats de fichiers. Il fournit des capacités d'analyse de document avancées avec des options configurables pour l'OCR, la chasse et l'extraction des métadonnées.

## Caractéristiques
- Analyse avancée de documents
- Prise en charge de l'OCR avec plusieurs options de langue
- Stratégies de section flexibles
- Inférence de la structure du tableau
- Coordonnée
- Gestion de la pause de la page
- Traitement de la balise XML
- Sélection de modèle personnalisable
- Extraction de métadonnées

## Configuration

### Configuration de l'API
- URL de l'API par défaut:`https://api.unstructuredapp.io/general/v0/general`
- Nécessite une clé API de non structurée.io
- Peut être configuré via des variables d'environnement:
  - `UNSTRUCTURED_API_URL`
  - `UNSTRUCTURED_API_KEY`

### Stratégies de traitement
- ** Stratégie **: la valeur par défaut est "Hi_res"
  - Les options incluent diverses stratégies de traitement pour différents types de documents
- ** Stratégie de chasse **:
  - Aucun (par défaut)
  - by_title (texte de morceaux basé sur des titres)

## Paramètres

### Paramètres requis
- ** Fichier **: le document à traiter
- ** Clé API **: clé API non structurée.io (si elle n'est pas définie via l'environnement)

### Paramètres facultatifs

#### Options OCR
- ** Langues OCR **: tableau de langues pour le traitement OCR
- ** Encodage **: Spécifiez le codage du document

#### Options de traitement
- ** Coordonnées **: Extraire les coordonnées des éléments (true / false)
- ** Structure de la table PDF **: Structure de table inférieure dans les PDF (vrai / faux)
- ** Tags XML **: Gardez les balises XML en sortie (true / false)
- ** Sauter les types de tables **: tableau des types de table pour sauter l'inférence
- ** modèle haute résolution **: spécifiez le nom du modèle haute résolution
- ** Inclure les pauses de page **: Inclure des informations de pause de page (vrai / false)

#### Options de section texte
- ** Sections de plusieurs pages **: gérer les sections sur les pages (vrai / false)
- ** Combinez sous N Chars **: Combinez des éléments sous le nombre de caractères spécifié
- ** Nouveau après n Chars **: Créez un nouvel élément après le nombre spécifié de caractères
- ** Caractères max **: caractères maximum par élément

## Structure de sortie

### Format de document
Chaque élément traité devient un document avec:
- ** PageContent **: Contenu texte extrait
- ** Metadata **:
  - Catégorie: Type d'élément
  - Métadonnées supplémentaires du traitement

### Types d'éléments
Le chargeur peut identifier divers types d'éléments:
- Blocs de texte
- Tables
- Listes
- Têtes
- Footters
- Breaks de page (si activé)
- Autres éléments structurels

## Exemples d'utilisation

### Configuration de base
```typescript
{
  "apiKey": "your-api-key",
  "strategy": "hi_res",
  "ocrLanguages": ["eng"]
}
```

### Traitement avancé
```typescript
{
  "apiKey": "your-api-key",
  "strategy": "hi_res",
  "coordinates": true,
  "pdfInferTableStructure": true,
  "chunkingStrategy": "by_title",
  "multiPageSections": true,
  "combineUnderNChars": 100,
  "maxCharacters": 4000
}
```

## Notes
- Les appels API sont effectués pour chaque demande de traitement de fichier
- La réponse comprend des éléments structurés avec du texte et des métadonnées
- Les éléments sont filtrés pour garantir un contenu texte valide
- Prend en charge le traitement basé sur les tampons
- Gestion des erreurs pour les réponses API
- Catégorisation automatique des métadonnées
- Traitement économe en mémoire

## Meilleures pratiques
1. Définissez les paramètres de section appropriés pour votre cas d'utilisation
2. Considérez les paramètres de langue OCR pour les documents non anglais
3. Activer l'inférence de la structure du tableau pour les documents avec des tables
4. Utiliser les coordonnées lorsque les informations spatiales sont importantes
5. Configurer les limites de caractères en fonction de vos besoins de traitement en aval
6. Surveiller l'utilisation et les temps de réponse de l'API
7. Gérer les erreurs d'API potentielles dans votre flux de travail

{% hint style = "info"%}
Cette section est un travail en cours. Nous apprécions toute aide que vous pouvez fournir pour terminer cette section. Veuillez vérifier notre[Contribution Guide](broken-reference)Pour commencer.
{% EndHint%}
