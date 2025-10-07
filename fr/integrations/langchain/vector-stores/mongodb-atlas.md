---
description: >-
  Upsert embedded data and perform similarity or mmr search upon query using
  MongoDB Atlas, a managed cloud mongodb database.
---

# MongoDB Atlas

<gigne> <img src = "../../../. GitBook / Assets / Image (161) .png" alt = "" width = "308"> <Figcaption> <p> MongoDB Atlas nœud </p> </ figCaption> </ Figure>

### Configuration de la cluster[​](https://js.langchain.com/docs/integrations/vectorstores/mongodb_atlas/#initial-cluster-configuration)<a href = "# Initial-Cluster-Configuration" id = "Initial-Cluster-Configuration"> </a>

Pour configurer un cluster MongoDB Atlas, allez au[MongoDB Atlas ](https://www.mongodb.com/)Site Web et inscrivez-vous si vous n'avez pas de compte. Lorsque vous y êtes invité, créez et nommez votre cluster, qui apparaîtra dans la section de la base de données. Ensuite, sélectionnez "** Browse Collections **" pour créer une nouvelle collection ou en utiliser une à partir des exemples de données fournies.

{% hint style = "avertissement"%}
Assurez-vous que le cluster que vous créez est la version 7.0 ou supérieure.
{% EndHint%}

### Création d'index

Après avoir configuré votre cluster, l'étape suivante consiste à créer un index pour le champ de collection que vous avez l'intention de rechercher.

1. Accédez à l'onglet ** Recherche ** ATLAS ** et cliquez sur ** Créer un index de recherche **.
2. SELECT ** ATLAS VECTOR Recherche - JSON Editor **, choisissez la base de données et la collection appropriées, puis collez ce qui suit dans la zone de texte:

```json
{
  "fields": [
    {
      "numDimensions": 1536,
      "path": "embedding",
      "similarity": "euclidean",
      "type": "vector"
    }
  ]
}
```

Assurez-vous que le`numDimensions`La propriété correspond à la dimensionnalité des intérêts que vous utilisez. Par exemple, les incorporations de cohére ont généralement 1024 dimensions, tandis que les incorporations OpenAI ont 1536 par défaut.

** Remarque: ** Le magasin vectoriel attend certaines valeurs par défaut, telles que:

* Un nom d'index de`default`
* Un nom de champ de collection de`embedding`
* Un nom de champ de texte brut de`text`

Assurez-vous d'initialiser le magasin vectoriel avec des noms de champ qui correspondent à votre schéma d'index et de collecte, comme indiqué dans l'exemple ci-dessus.

Une fois cela fait, procédez pour construire l'index.

{% hint style = "info"%}
Cette section est un travail en cours. Nous apprécions toute aide que vous pouvez fournir pour terminer cette section. Veuillez vérifier notre[Contribution Guide](broken-reference)Pour commencer.
{% EndHint%}

### Configuration de flux

Faites glisser et déposez le MongoDB Atlas Vector Store et ajoutez un nouvel diplôme. Utilisez la chaîne de connexion fournie à partir du tableau de bord MongoDB ATLAS:

<gigne> <img src = "../../../. GitBook / Assets / Image (1) (1) (1) (1) (1) (1) (1) (1) (2) .png" alt = ""> <figCaption> </ Figcaption> </pigucial>

Remplissez le reste des champs:

<gigne> <img src = "../../../. Gitbook / Assets / image (1) (1) (1) (1) (1) (1) (1) (1) (2) (1) .png" alt = "" width = "252"> <figcaption>

Vous pouvez également configurer plus de détails à partir de paramètres supplémentaires:

<gigne> <img src = "../../../. GitBook / Assets / Image (164) .png" alt = "" width = "518"> <Figcaption> </ Figcaption> </ Figure>
