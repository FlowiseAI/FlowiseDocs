---
description: Load data from Airtable table.
---

# Chargeur de documents Airtable

<gigne> <img src = "../../../. GitBook / Assets / Image_AirTable (1) .png" alt = "" width = "271"> <Figcaption> <p> nœud AirTable </p> </gigcaption> </ figure>

AirTable est un service de collaboration cloud qui combine les fonctionnalités d'une feuille de calcul avec une base de données. Ce module fournit des fonctionnalités complètes pour charger et traiter les données des tables Airtable.

Ce module fournit un chargeur de document Airtable sophistiqué qui peut:
- Chargez des données à partir de bases, de tables et de vues à air spécifiques
- Filtre et sélectionner des champs spécifiques
- Gérer la pagination et les grands ensembles de données
- Prise en charge du filtrage personnalisé avec des formules
- Traiter les données avec des séparateurs de texte
- Personnaliser l'extraction des métadonnées

## Entrées

### Paramètres requis
- ** ID de base **: l'identifiant de base Airtable (par exemple, app10ROBDGOX0YNSC)
- ** ID de table **: l'identifiant de table spécifique (par exemple, tbljdmvbrgizbyico)
- ** Connectez les informations d'identification **: AirTable API Credentials

### Paramètres facultatifs
- ** Voir ID **: Identificateur de vue spécifique (par exemple, viw9urp77id0ce4ee)
- ** Splitter du texte **: un séparateur de texte pour traiter le contenu extrait
- ** Inclure uniquement les champs **: Liste de noms ou identifiants séparés par des virgules à inclure
- ** Renvoie tout **: s'il faut renvoyer tous les résultats (par défaut: true)
- ** Limit **: Nombre de résultats à retourner lorsque le retour tout est faux (par défaut: 100)
- ** Filtre par formule **: Formule AirTable pour filtrer les enregistrements
- ** Metadata supplémentaires **: objet JSON avec métadonnées supplémentaires
- ** omettre les clés de métadonnées **: Liste des clés de métadonnées séparées par des virgules pour omettre

## Sorties

- ** Document **: tableau d'objets de document contenant des métadonnées et un conceptent
- ** Texte **: chaîne concaténée du conceptent de documents

## Caractéristiques
- Récupération des données basée sur l'API
- Sélection et filtrage des champs
- Support de pagination
- Filtrage basé sur la formule
- Manipulation des métadonnées personnalisables
- Capacités de division de texte
- Gestion des erreurs pour les entrées non valides

## Notes
- Nécessite des informations d'identification API Airtable valide
- L'ID de base et l'ID de table sont obligatoires
- Les noms de champ contenant des virgules doivent utiliser des ID de champ à la place
- Les formules de filtre doivent suivre la syntaxe de formule Airtable
- La limitation des taux et les quotas API s'appliquent
- Prend en charge la récupération de données complète et partielle

## Exemple de structure d'URL
Pour une URL de table comme:
```
https://airtable.com/app11RobdGoX0YNsC/tblJdmvbrgizbYICO/viw9UrP77Id0CE4ee
```
- ID de base: app10ROBDGOX0YNSC
- ID de table: tbljdmvbrgizbyico
- Afficher l'ID: viw9urp77id0ce4ee

