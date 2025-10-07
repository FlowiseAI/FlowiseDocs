# Chargeur de documents Bravesearch API

Bravesearch est un moteur de recherche axé sur la confidentialité qui fournit une API puissante pour la recherche Web. Ce module permet le chargement et le traitement des résultats de recherche de Bravesearch dans les documents.

Ce module fournit un chargeur de document de recherche sophistiqué qui peut:
- Exécuter des recherches Web à l'aide de l'API Bravesearch
- Convertir les résultats de recherche en documents structurés
- Extraire des extraits et des métadonnées à partir des résultats
- Processus Résultats avec des séparateurs de texte
- Personnaliser l'extraction des métadonnées

## Entrées

### Paramètres requis
- ** requête **: la requête de recherche à exécuter
- ** Connectez les informations d'identification **: Bravesearch API Identifiés

### Paramètres facultatifs
- ** Splitter du texte **: un séparateur de texte pour traiter le contenu extrait
- ** Metadata supplémentaires **: objet JSON avec métadonnées supplémentaires
- ** omettre les clés de métadonnées **: Liste des clés de métadonnées séparées par des virgules pour omettre

## Sorties

- ** Document **: tableau d'objets de document contenant des métadonnées et un conceptent
- ** Texte **: chaîne concaténée du conceptent de documents

## Caractéristiques
- Recherche Web axée sur la confidentialité
- Traitement des résultats structurés
- Extraction automatique des métadonnées
- Résultat de contenu
- Manipulation des métadonnées personnalisables
- Gestion des erreurs pour les réponses API

## Structure de document
Chaque résultat de recherche est converti en document avec:
- ** PageContent **: l'extrait / contenu du résultat de la recherche
- ** Metadata **:
  - Titre: le titre de la page Web
  - Lien: l'URL de la page Web
  - Métadonnées personnalisées supplémentaires comme spécifié

## Notes
- Nécessite la clé de l'API Bravesearch valide
- Les résultats incluent des extraits de page Web et des métadonnées
- Peut être combiné avec des séparateurs de texte pour le traitement du contenu
- Prend en charge l'ajout et l'omission des métadonnées personnalisées
- Gère les limites et les erreurs du taux de l'API
- Préserve les fonctionnalités de recherche axées sur la confidentialité