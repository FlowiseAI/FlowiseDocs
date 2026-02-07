---
description: Load data from a Confluence Document
---

# Confluence

## Confluence

<gigne> <img src = "../../../. Gitbook / Assets / Image (3) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (2) .png" alt = "" width = "263"> <Figcution> <p>

## Chargeur de documents Confluence

Confluence est le wiki et la plate-forme de collaboration d'Atlassian. Ce module fournit des fonctionnalités pour charger et traiter le contenu à partir des espaces et des pages de confluence.

Ce module fournit un chargeur de document à confluence sophistiqué qui peut:

* Chargez le contenu à partir d'espaces de confluence spécifiques
* Prise en charge des déploiements Cloud et Server / Data Center
* Gérer l'authentification avec plusieurs méthodes
* Limiter le nombre de pages récupérées
* Traiter le contenu avec des séparateurs de texte
* Personnaliser l'extraction des métadonnées

### Entrées

#### Paramètres requis

* ** URL de base **: L'URL de l'instance de confluence (par exemple, https://example.atlassian.net/wiki)
* ** Espace Key **: L'identifiant unique de l'espace de confluence
* ** Connectez les informations d'identification **: Choisissez entre:
  * Conditions d'identification de l'API Cloud Confluence (nom d'utilisateur + jeton d'accès)
  * Informations sur l'api du serveur Confluence / DC (Token d'accès personnel)

#### Paramètres facultatifs

* ** Splitter du texte **: un séparateur de texte pour traiter le contenu extrait
* ** Limite **: Nombre maximum de pages à récupérer (0 pour illimité)
* ** Metadata supplémentaires **: objet JSON avec métadonnées supplémentaires
* ** omettre les clés de métadonnées **: Liste des clés de métadonnées séparées par des virgules pour omettre

### Sorties

* ** Document **: tableau d'objets de document contenant des métadonnées et un conceptent
* ** Texte **: chaîne concaténée du conceptent de documents

### Caractéristiques

* Prise en charge multi-déploiement (cloud / serveur / dc)
* Options d'authentification flexibles
* Contrôles de limite de page
* Capacités de traitement du contenu
* Personnalisation des métadonnées
* Gestion des erreurs
* Support de division de texte

### Méthodes d'authentification

#### Nuage de confluence

* Nom d'utilisateur et jeton d'accès
* Token d'accès généré à partir des paramètres du compte atlassian
* Prend en charge l'authentification des jetons API

#### Serveur de confluence / centre de données

* Utilise un jeton d'accès personnel
* Jeton généré à partir de l'instance de confluence
* Prend en charge l'accès direct au serveur

### Notes

* La clé d'espace peut être trouvée dans les réglages d'espace Confluence
* Différentes méthodes d'authentification pour le cloud vs serveur
* La limitation des taux peut s'appliquer en fonction de l'instance
* Le contenu comprend le texte et les métadonnées de la page
* Prend en charge la récupération de contenu à la fois complète et partielle
* Gestion des erreurs pour les informations d'identification ou URL non valides

### Trouver une clé d'espace

Pour trouver votre clé d'espace de confluence:

1. Accédez à l'espace en confluence
2. Aller dans les paramètres de l'espace
3. Recherchez "Key Space" dans l'aperçu
4. Exemple de format: \ ~ Exemple362906de5d343d49dcdbae5Dexample
