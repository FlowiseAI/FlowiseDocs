---
description: Load data from a Figma file.
---

# Chargeur de documents Figma

<gigne> <img src = "../../../. Gitbook / Assets / Image (8) (1) (1) (1) (1) (1) (1) (1) .png" alt = "" width = "264"> </ Figcaption> <p> nœud </p> </ / Figcaption> </ Figure>

Figma est une application Web collaborative pour la conception d'interface. Ce module fournit des fonctionnalités pour charger et traiter le contenu à partir des fichiers Figma, y ​​compris du texte, des composants et des métadonnées.

Ce module fournit un chargeur de document Figma sophistiqué qui peut:
- Chargez le contenu à partir de fichiers Figma spécifiques
- Extraire du texte à partir de nœuds sélectionnés
- Traiter le contenu récursivement
- Gérer l'authentification avec l'API Figma
- Traiter le contenu avec des séparateurs de texte
- Personnaliser l'extraction des métadonnées

## Entrées

### Paramètres requis
- ** Clé de fichier **: l'identifiant unique du fichier Figma (à partir de l'URL du fichier)
- ** ID de nœud **: Liste des identifiants de nœud séparés par des virgules à extraire
- ** Connectez les informations d'identification **: les informations d'identification de l'API Figma (jeton d'accès)

### Paramètres facultatifs
- ** récursif **: s'il faut traiter les nœuds récursivement
- ** Splitter du texte **: un séparateur de texte pour traiter le contenu extrait
- ** Metadata supplémentaires **: objet JSON avec métadonnées supplémentaires
- ** omettre les clés de métadonnées **: Liste des clés de métadonnées séparées par des virgules pour omettre

## Sorties

- ** Document **: tableau d'objets de document contenant des métadonnées et un conceptent
- ** Texte **: chaîne concaténée du conceptent de documents

## Caractéristiques
- Extraction de contenu basée sur l'API
- Sélection de contenu au niveau du nœud
- Traitement récursif
- Support de division de texte
- Personnalisation des métadonnées
- Gestion des erreurs
- Gestion de l'authentification

## Format de clé de fichier
La touche de fichier se trouve dans l'URL du fichier Figma:
```
https://www.figma.com/file/:key/:title
```
Exemple: dans`https://www.figma.com/file/12345/Website`, la clé de fichier est`12345`

## ID de nœud
Pour obtenir des ID de nœud:
1. Installez le plugin d'inspecteur de nœud sur Figma
2. Sélectionnez les éléments souhaités
3. Copiez les ID de nœud de l'inspecteur
4. Utilisez le format séparé des virgules: "0, 1, 2"

## Notes
- Nécessite un jeton d'accès Figma valide
- Les ID de nœud doivent être valides pour le fichier
- Prend en charge l'extraction de contenu récursif
- Peut traiter plusieurs nœuds à la fois
- Gère les limites et les erreurs du taux de l'API
- Préserve la hiérarchie des nœuds dans les métadonnées
- Prend en charge l'ajout de métadonnées personnalisées
