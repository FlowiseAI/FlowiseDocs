---
description: Load data from a GitHub repository.
---

# Chargeur de documents github

<gigne> <img src = "../../../. GitBook / Assets / Image (79) .png" alt = "" width = "260"> <figcaption> <p> nœud github </p> </gigcaption> </ figure>

GitHub est une plate-forme pour le contrôle et la collaboration de versions. Ce module fournit des fonctionnalités pour charger et traiter le contenu à partir des référentiels GitHub, soutenant les référentiels publics et privés.

Ce module fournit un chargeur de document GitHub sophistiqué qui peut:
- Chargez le contenu des référentiels GitHub
- Prise en charge de l'accès au référentiel privé
- Traiter les référentiels récursivement
- Gérer les instances de github personnalisées
- Contrôler la concurrence et les tentatives
- Personnaliser le filtrage des fichiers
- Traiter le contenu avec des séparateurs de texte

## Entrées

### Paramètres requis
- ** REPO LIEN **: L'URL du référentiel GitHub (par exemple, https://github.com/flowiseai/flowise)
- ** Branch **: La branche pour charger le contenu de (par défaut: Main)

### Paramètres facultatifs
- ** Connectez les informations d'identification **: les informations d'identification de l'API GitHub (requises pour les références privées)
- ** récursif **: s'il faut traiter les sous-répertoires
- ** MAX CONCURENCE **: Nombre maximum de charges de fichiers simultanées
- ** URL de base GitHub **: URL de base GitHub personnalisée pour les instances d'entreprise
- ** API d'instance GitHub **: URL de l'API GitHub personnalisée pour les instances d'entreprise
- ** Ignorer les chemins **: tableau des modèles globaux pour les chemins à ignorer
- ** MAX RETRES **: Nombre maximum de tentatives de réessayer
- ** Splitter du texte **: un séparateur de texte pour traiter le contenu extrait
- ** Metadata supplémentaires **: objet JSON avec métadonnées supplémentaires
- ** omettre les clés de métadonnées **: Liste des clés de métadonnées séparées par des virgules pour omettre

## Sorties

- ** Document **: tableau d'objets de document contenant des métadonnées et un conceptent
- ** Texte **: chaîne concaténée du conceptent de documents

## Caractéristiques
- Support public / privé Repo
- Prise en charge de l'instance d'entreprise
- Traitement répertoire récursif
- Contrôle de la concurrence
- Réessayer le mécanisme
- Filtrage de chemin
- Support de division de texte
- Personnalisation des métadonnées

## Méthodes d'authentification

### Référentiels publics
- Aucune authentification requise
- Les limites de taux s'appliquent
- Limité au contenu public

### Référentiels privés
- Nécessite un jeton d'accès GitHub
- Limites de taux plus élevées
- Accès au contenu privé
- Assistance d'entreprise

## Structure de document
Chaque document contient:
- ** PageContent **: contenu de fichier
- ** Metadata **:
  - Source: chemin de fichier dans le référentiel
  - branche: branche du référentiel
  - commit: engager le hachage
  - Métadonnées personnalisées supplémentaires

## Notes
- Soutient les reposs publics et privés
- Instances GitHub d'entreprise prises en charge
- La limitation du taux géré automatiquement
- Backoff exponentiel pour les tentatives
- Filtrage de chemin avec les modèles globaux
- Traitement économe en mémoire
- Gestion des erreurs pour les références non valides
