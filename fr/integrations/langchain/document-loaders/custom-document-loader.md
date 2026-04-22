---
description: Custom function for loading documents.
---

# Chargeur de documents personnalisé

<gigne> <img src = "../../../. GitBook / Assets / Image_Custom-Wocher (1) .png" alt = "" width = "269"> <figcaption> <p> Node de chargeur de document personnalisé </p> </ figCaption> </pigucial>

Le chargeur de documents personnalisé offre la possibilité de créer des fonctionnalités de chargement de documents personnalisées à l'aide de JavaScript. Ce module permet le traitement de documents flexible et personnalisé via des fonctions définies par l'utilisateur.

Ce module fournit un chargeur de documents flexible qui peut:
- Exécuter des fonctions JavaScript personnalisées pour le chargement des documents
- Gérer dynamiquement les variables d'entrée
- Prise en charge des sorties de document et de texte
- Exécuter dans un environnement en sable
- Contexte de flux d'accès et variables
- Traiter les métadonnées personnalisées

## Entrées

### Paramètres requis
- ** Fonction JavaScript **: code personnalisé qui revient soit:
  - Tableau d'objets de document (pour la sortie du document)
  - String (pour la sortie du texte)

### Paramètres facultatifs
- ** Variables d'entrée **: Variables JSON contenant des variables accessibles dans la fonction avec $ Prefix

## Sorties

- ** Document **: tableau d'objets de document contenant des métadonnées et un conceptent
- ** Texte **: chaîne concaténée du conceptent de documents

## Caractéristiques
- Environnement d'exécution de boîte à sable
- Support d'injection variable
- Accès au contexte de flux
- Support de dépendance personnalisé
- Gestion des erreurs
- Protection contre le délai
- Validation d'entrée

## Structure de document
Lors du retour des documents, chaque objet doit avoir:
```javascript
{
  pageContent: 'Document Content',
  metadata: {
    title: 'Document Title',
    // ... other metadata
  }
}
```

## Exemple d'utilisation

### Documer la sortie
```javascript
return [
  {
    pageContent: 'Document Content',
    metadata: {
      title: 'Document Title',
      source: 'Custom Source'
    }
  }
]
```

### Sortie de texte
```javascript
return "Processed text content"
```

## Contexte disponible
- ** $ entrée **: valeur d'entrée transmise à la fonction
- ** $ vars **: Accès aux variables de flux
- ** $ Flow **: Flow Context Object Contenant:
  - chatflowid
  - de session
  - chatitide
  - saisir

## Notes
- Fonctions exécutées dans un bac à sable sécurisé
- Tempsion d'exécution de 10 secondes
- Dépendances intégrées disponibles
- Dépendances externes configurables
- Les variables d'entrée doivent être valides JSON
- Gestion des erreurs pour les rendements non valides
- Prend en charge les opérations asynchrones
