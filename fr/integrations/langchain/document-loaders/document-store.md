---
description: Load data from pre-configured document stores.
---

# Magasin de documents

<gigne> <img src = "../../../. Gitbook / Assets / Image (6) (1) (1) (1) (1) (1) (1) (1) (2) .png" alt = "" width = "278"> <figcaption>

Le chargeur de magasin de documents vous permet de charger des données des magasins de documents préconfigurés dans votre base de données. Ce chargeur fournit un moyen pratique d'accéder et d'utiliser des documents précédemment traités et stockés dans vos workflows.

## Caractéristiques

* Charge des documents des magasins synchronisés
* Gestion automatique des métadonnées
* Formats de sortie multiples
* Sélection de magasins asynchrones
* Intégration de la base de données
* Récupération de documents basée sur des morceaux
* Support de métadonnées JSON

## Comment ça marche

1. ** Sélection du magasin **:
   * Répertorie tous les magasins disponibles disponibles en statut «synchronisé»
   * Fournit des informations sur le magasin, y compris le nom et la description
   * Permet la sélection des magasins synchronisés uniquement
2. ** Retriel de document **:
   * Rechet des morceaux de document de la boutique sélectionnée
   * Reconstruit les documents avec des métadonnées originales
   * Maintient la structure et les relations du document

## Paramètres

### Paramètres requis

* ** Sélectionnez Store **: Choisissez parmi les magasins de documents synchronisés disponibles
  * Affiche le nom et la description du magasin
  * Affiche uniquement les magasins en statut «synchronisé»
  * Mis à jour dynamiquement en fonction du contenu de la base de données

## Sorties

Le chargeur fournit deux formats de sortie:

### Documer la sortie

Renvoie un tableau d'objets de document, chacun contenant:

* ** PageContent **: le contenu réel du morceau de document
* ** Metadata **: Métadonnées du document original au format JSON

### Sortie de texte

Renvoie une chaîne concaténée contenant:

* Tous les morceaux de document
* Séparés par Newlines
* Caractères correctement échappés

## Intégration de la base de données

Le chargeur s'intègre à votre base de données:

* Connexion de source de données Typeorm
* Gestion des entités du magasin de documents
* Stockage et récupération basés sur des morceaux
* Conservation des métadonnées

## Structure de document

Chaque document chargé contient:

```typescript
{
  pageContent: string,    // The actual content
  metadata: {            // Parsed JSON metadata
    // Original document metadata
    // Store-specific information
    // Custom metadata fields
  }
}
```

## Exemples d'utilisation

### Sélection de magasin de base

```json
{
  "selectedStore": "store-id-123"
}
```

### Accéder au contenu du document

```typescript
// Document output format
[
  {
    "pageContent": "Document content here...",
    "metadata": {
      "source": "original-file.pdf",
      "page": 1,
      "category": "reports"
    }
  }
]

// Text output format
"Document content here...\nNext document content here...\n"
```

## Meilleures pratiques

1. Assurez-vous que les magasins sont synchronisés avant l'accès
2. Choisissez le format de sortie approprié pour votre cas d'utilisation
3. Gérer les métadonnées de manière appropriée dans votre flux de travail
4. Considérez la taille du morceau lors du traitement de grands documents
5. Surveiller les performances de la base de données avec les grands magasins

## Notes

* Seuls les magasins synchronisés sont disponibles pour la sélection
* Les métadonnées sont automatiquement analysées de JSON
* Les documents sont reconstruits à partir de morceaux
* Prend en charge les formats de sortie de document et de texte
* S'intègre à Typeorm pour l'accès à la base de données
* Gère les caractères d'échappement dans la sortie du texte
* Maintient la structure du document original

{% hint style = "info"%}
Cette section est un travail en cours. Nous apprécions toute aide que vous pouvez fournir pour terminer cette section. Veuillez vérifier notre[Contribution Guide](broken-reference/)Pour commencer.
{% EndHint%}
