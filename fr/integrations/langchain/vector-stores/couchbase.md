---
description: >-
  Upsert embedded data and perform vector search upon query using Couchbase, a
  NoSQL cloud developer data platform for critical, AI-powered applications.
---

# Canapé

## Condition préalable

### Exigences

1. Couchbase Cluster (Version auto-gérée ou capella) ** 7.6 + ** avec[Search Service](https://docs.couchbase.com/server/current/search/search.html).
2.  Configuration de Capella: Pour en savoir plus sur la connexion à votre cluster Capella, veuillez suivre le[instructions](https://docs.couchbase.com/cloud/get-started/connect.html?_gl=1*1yhpmel*_gcl_au*MTMzNDE3NTQxLjE3MzY5MjA5MzQ.).

Plus précisément, vous devez faire ce qui suit:

    * Créer le[database credentials](https://docs.couchbase.com/cloud/clusters/manage-database-users.html?_gl=1*19zk7vq*_gcl_au*MTMzNDE3NTQxLjE3MzY5MjA5MzQ.)pour accéder au cluster.
    * [Allow access](https://docs.couchbase.com/cloud/clusters/allow-ip-address.html?_gl=1*19zk7vq*_gcl_au*MTMzNDE3NTQxLjE3MzY5MjA5MzQ.)au cluster à partir de l'IP sur lequel l'application est en cours d'exécution.

Configuration auto-gérée:

    * Suivre[Couchbase Installation Options](https://developer.couchbase.com/tutorial-couchbase-installation-options)Pour l'installation de la dernière instance de serveur de base de données CouchBase. Assurez-vous d'ajouter le service de recherche.
3. Recherchez la création d'index sur le service de texte intégral dans Couchbase.

### Index d'importation de recherche

#### [Couchbase Capella](\(https:/docs.couchbase.com/cloud/search/import-search-index.html)

Suivez ces étapes pour importer un index de recherche dans Capella:

* Copiez la définition d'index dans un nouveau fichier nommé`index.json`.
* Importez le fichier dans Capella suivant les instructions de la documentation.
* Cliquez sur Créer l'index pour finaliser la création d'index.

#### [Couchbase Server](\(https:/docs.couchbase.com/server/current/search/import-search-index.html)

Suivez ces étapes pour le serveur CouchBase:

* Naviguez vers la recherche → Ajouter un index → ​​l'importation.
* Copiez la définition d'index fournie dans l'écran d'importation.
* Cliquez sur Créer l'index pour finaliser la création d'index.

Vous pouvez également créer un index vectoriel à l'aide de l'interface utilisateur de recherche sur les deux[Couchbase Capella](https://docs.couchbase.com/cloud/vector-search/create-vector-search-index-ui.html?_gl=1*1rglcpj*_gcl_au*MTMzNDE3NTQxLjE3MzY5MjA5MzQ.)et[Couchbase Self Managed Server](https://docs.couchbase.com/server/current/vector-search/create-vector-search-index-ui.html?_gl=1*t7aeet*_gcl_au*MTMzNDE3NTQxLjE3MzY5MjA5MzQ.).

### Définition d'index

Ici, nous créons l'index`vector-index`sur les documents. Le champ vectoriel est défini sur`embedding`avec 1536 dimensions et le champ de texte réglé sur`text`. Nous indexons et stockons également tous les champs sous`metadata`dans le document en tant que mappage dynamique pour tenir compte des structures de documents variables. La métrique de similitude est définie sur`dot_product`. S'il y a un changement dans ces paramètres, veuillez adapter l'index en conséquence.

```json
{
  "name": "vector-index",
  "type": "fulltext-index",
  "params": {
    "doc_config": {
      "docid_prefix_delim": "",
      "docid_regexp": "",
      "mode": "scope.collection.type_field",
      "type_field": "type"
    },
    "mapping": {
      "default_analyzer": "standard",
      "default_datetime_parser": "dateTimeOptional",
      "default_field": "_all",
      "default_mapping": {
        "dynamic": true,
        "enabled": false
      },
      "default_type": "_default",
      "docvalues_dynamic": false,
      "index_dynamic": true,
      "store_dynamic": false,
      "type_field": "_type",
      "types": {
        "_default._default": {
          "dynamic": true,
          "enabled": true,
          "properties": {
            "embedding": {
              "enabled": true,
              "dynamic": false,
              "fields": [
                {
                  "dims": 1536,
                  "index": true,
                  "name": "embedding",
                  "similarity": "dot_product",
                  "type": "vector",
                  "vector_index_optimized_for": "recall"
                }
              ]
            },
            "metadata": {
              "dynamic": true,
              "enabled": true
            },
            "text": {
              "enabled": true,
              "dynamic": false,
              "fields": [
                {
                  "index": true,
                  "name": "text",
                  "store": true,
                  "type": "text"
                }
              ]
            }
          }
        }
      }
    },
    "store": {
      "indexType": "scorch",
      "segmentVersion": 16
    }
  },
  "sourceType": "gocbcore",
  "sourceName": "pdf-chat",
  "sourceParams": {},
  "planParams": {
    "maxPartitionsPerPIndex": 64,
    "indexPartitions": 16,
    "numReplicas": 0
  }
}

```

## Installation

1. Ajoutez un nouveau nœud ** Couchbase ** sur toile et remplissez le nom du seau, le nom de la portée, le nom de la collection et le nom d'index

<gigne> <img src = "../../../. GitBook / Assets / Couchbase_1.png" alt = ""> <Figcaption> </gigcaption> </ Figure>

2. Ajoutez de nouveaux informations d'identification et remplissez les paramètres:
   * Chaîne de connexion Couchbase
   * Nom d'utilisateur en grappes
   * Mot de passe du cluster

<gigne> <img src = "../../../. GitBook / Assets / CouchBase_2.png" alt = ""> <gignedcaption> </gigcaption> </gigust>

3. Ajouter des nœuds supplémentaires à la toile et démarrer le processus ussert
   * ** Document ** peut être connecté à n'importe quel nœud sous[**Document Loader**](../document-loaders/)catégorie
   * ** Embeddings ** peut être connecté à n'importe quel nœud sous[**Embeddings** ](../embeddings/)catégorie

<gigne> <img src = "../../../. GitBook / Assets / CouchBase_3.png" alt = ""> <Figcaption> </gigcaption> </ Figure>

<gigne> <img src = "../../../. GitBook / Assets / Couchbase_4.png" alt = ""> <gignedcaption> </gigcaption> </gigust>

5. Vérifiez de l'interface utilisateur Couchbase pour voir si les données ont été renversées avec succès!

## Ressources

* Intégrations Langchain Couchbase Vectorstore
  * [Python](https://python.langchain.com/docs/integrations/vectorstores/couchbase/)
  * [NodeJS](https://js.langchain.com/docs/integrations/vectorstores/couchbase/)
* Reportez-vous au[Couchbase Documentation](https://docs.couchbase.com/home/index.html)Pour en savoir plus sur Couchbase.
