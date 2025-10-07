# Chargeur de documents Jira

<gigne> <img src = "../../../. GitBook / Assets / Image (284) .png" alt = "" width = "234"> <Figcaption> </gigcaption> </ figure>

Jira est un outil de suivi et de gestion de projet populaire. Ce module fournit des fonctionnalités pour charger et traiter les problèmes des projets JIRA, prenant en charge diverses options de filtrage et personnalisation des métadonnées.

Ce module fournit un chargeur de document Jira sophistiqué qui peut:
- Chargez les problèmes des projets Jira
- Filtre les problèmes par date de création
- Contrôler la taille du lot pour les demandes
- Traiter le contenu avec des séparateurs de texte
- Personnaliser l'extraction des métadonnées
- Gérer l'authentification de l'API

## Entrées

### Paramètres requis
- ** Connectez les informations d'identification **: JIRA API Identifiés (nom d'utilisateur et jeton d'accès)
- ** Hôte **: URL de l'instance Jira (par exemple, https://jira.example.com)
- ** Clé du projet **: La clé du projet JIRA pour charger des problèmes de

### Paramètres facultatifs
- ** Limite par demande **: Nombre de problèmes à récupérer par demande API (par défaut: 100)
- ** Créé après **: Filtrez les problèmes créés après une date spécifique (par exemple, 2024-01-01)
- ** Splitter du texte **: un séparateur de texte pour traiter le contenu extrait
- ** Metadata supplémentaires **: objet JSON avec métadonnées supplémentaires
- ** omettre les clés de métadonnées **: Liste des clés de métadonnées séparées par des virgules pour omettre

## Sorties

- ** Document **: tableau d'objets de document contenant des métadonnées et un conceptent
- ** Texte **: chaîne concaténée du conceptent de documents

## Caractéristiques
- Authentification des jetons API
- Chargement du problème basé sur les projets
- Filtrage de date de création
- Contrôle de la taille du lot
- Support de division de texte
- Personnalisation des métadonnées
- Formats de sortie flexibles

## Authentification
Le chargeur nécessite:
- Nom d'utilisateur Jira
- Token d'accès API
- URL de l'hôte de votre instance Jira

## Structure de document
Chaque document contient:
- ** PageContent **: émettez le contenu et la description
- ** Metadata **:
  - Métadonnées spécifiques au problème (personnalisables)
  - Informations sur le projet
  - Dates de création
  - Statut d'émission
  - Métadonnées personnalisées supplémentaires

## Manipulation des métadonnées
Deux façons de personnaliser les métadonnées:
1. ** Metadata supplémentaires **: Ajouter de nouveaux champs de métadonnées
   - Spécifiez comme objet JSON
   - Fusionné avec les métadonnées existantes

2. ** omettre les clés de métadonnées **: supprimer les métadonnées indésirables
   - Liste des clés séparées par des virgules
   - Utiliser * pour supprimer toutes les métadonnées par défaut
   - Clés imbriquées prises en charge (par exemple, Key1, Key2, Key3.NestedKey1)

## Notes
- Gère la limitation du taux de l'API
- Traitement efficace des lots
- Chargement économe en mémoire
- Gestion des erreurs pour les demandes d'API
- Prend en charge Jira Cloud et auto-hébergé
- Accès aux données du problème en temps réel
