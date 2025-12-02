---
description: Load data from URL using FireCrawl.
---

# Pompier

<gigne> <img src = "../../../. GitBook / Assets / Up-004.png" alt = "" width = "347"> <figcaption> <p> Node Firecraw

# Chargeur de documents Firecraw

Firecrawl est un puissant service Web rampant et grattant qui offre des capacités avancées pour extraire le contenu des sites Web. Ce module permet le chargement et le traitement du contenu Web via l'API Firecrawl.

Ce module fournit un robot Web sophistiqué qui peut:
- Gratter les pages Web uniques
- Crawl entiers des sites Web
- Extraire des données structurées
- Gérer le contenu rendu javascript
- Traiter le contenu avec des séparateurs de texte
- Personnaliser l'extraction des métadonnées
- Prise en charge de plusieurs modes de fonctionnement

## Entrées

### Paramètres requis
- ** URL **: la page Web ou l'URL du site Web à traiter
- ** Connectez les informations d'identification **: FireCrawl API Création d'identification
- ** Mode **: Choisissez entre:
  - Groupement: extraction à une seule page
  - Crawl: site Web de plusieurs pages rampant
  - Extrait: Extraction de données structurées

### Paramètres facultatifs
- ** Splitter du texte **: un séparateur de texte pour traiter le contenu extrait
- ** Options de rupture **:
  - Inclure des balises: balises HTML à inclure
  - Exclure les balises: balises HTML pour exclure
  - Mobile: utilisez un agent utilisateur mobile
  - Skip TLS Vérification: contourner les contrôles SSL
  - Timeout: Demandez le délai d'expiration
- ** Metadata supplémentaires **: objet JSON avec métadonnées supplémentaires
- ** omettre les clés de métadonnées **: Liste des clés de métadonnées séparées par des virgules pour omettre

## Sorties

- ** Document **: tableau d'objets de document contenant des métadonnées et un conceptent
- ** Texte **: chaîne concaténée du conceptent de documents

## Caractéristiques
- Plusieurs modes de fonctionnement
- Options de grattage avancées
- Extraction de données structurées
- Rendu javascript
- Émulation de l'appareil mobile
- Paramètres de délai d'attente personnalisés
- Gestion des erreurs

## Modes de fonctionnement

### Mode de grat
- Traitement à une seule page
- Extraction de contenu principale
- Sélection de format
- Filtrage de balises personnalisé

### Mode de crawl
- Rampage de plusieurs pages
- Manipulation du sous-domaine
- Traitement du site
- Extraction de liaison

### Mode d'extrait
- Extraction de données structurées
- Analyse de schéma
- Extraction à base de LLM
- Invites d'extraction personnalisées

## Structure de document
Chaque document contient:
- ** PageContent **: Contenu extrait au format Markdown
- ** Metadata **:
  - Titre: Titre de la page
  - Description: Meta Description
  - Langue: contenu langue
  - SourceUrl: URL d'origine
  - Métadonnées personnalisées supplémentaires

## Notes
- Nécessite une clé API Firecrawl valide
- Prend en charge plusieurs formats de contenu
- Les manches limites de taux
- Surveillance de l'état du travail
- Gestion des erreurs et tentatives
- Options de demande personnalisables
- Traitement économe en mémoire
