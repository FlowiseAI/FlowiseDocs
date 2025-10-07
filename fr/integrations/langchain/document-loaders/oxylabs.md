---
description: Get data from any website with Oxylabs.
---

# Chargeurs de documents oxylabs

OxyLabs est un service de grattage Web qui récupère les données Web publiques à grande échelle, avec des outils conçus pour naviguer dans les restrictions régionales.

<gigne> <img src = "../../../. GitBook / Assets / OxyLabs_Document_Loadher.png" alt = "" width = "260"> <Figcaption> <p> oxylabs docuemnt chargeur nœud </ p> </gigcaption> </ Figure>


### Caractéristiques
- Récupérer les données de Google, d'Amazon et de tout autre site Web
- Régler la géolocalisation
- Utiliser le rendu du navigateur
- Analyser les données
- Spécifiez les types d'agents utilisateur
- Traiter le contenu avec des séparateurs de texte

### Paramètres requis
- ** Connectez les informations d'identification **: les informations d'identification de l'API OxyLabs
- ** Query **: Rechercher la requête ou l'URL
- ** Source **: l'une des sources disponibles:
  - Universal - grattez n'importe quel site Web
  - Recherche Google - StrAchez les résultats de la recherche Google
  - Product Amazon - StrArez les informations sur les produits Amazon
  - Recherche d'Amazon - StrAter les résultats de recherche Amazon

### Paramètres facultatifs
- ** Geolocation **: Définit l'emplacement GEO du proxy pour récupérer les données. Voir[documentation](https://files.gitbook.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FiwDdoZGfMbUe5cRL2417%2Fuploads%2FxoQb19qSyodB2D4no0DZ%2FList%20of%20supported%20geo_location%20values_sapi.json?alt=media&token=d2e2df7b-10ba-4399-a547-0c4a99e62293)pour plus de détails.
- ** Render **: Active le rendu JavaScript lorsqu'il est défini sur true.
- ** Parse **: Renvoie les données analysées lorsqu'elles sont définies sur true, tant qu'un analyseur dédié existe pour le type de page de l'URL soumis.
- ** Type d'agent utilisateur **: type de périphérique et navigateur.

### Sorties
- ** Document **: tableau d'objets de document contenant des métadonnées et un conceptent
- ** Texte **: chaîne concaténée du conceptent de documents


## Structure de document
Chaque document contient:
- ** PageContent **: Contenu de la page extrait
