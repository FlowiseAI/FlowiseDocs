# Élastique

## Condition préalable

1. Vous pouvez utiliser le[official Docker image](https://www.elastic.co/guide/en/elasticsearch/reference/current/docker.html)pour commencer, ou vous pouvez utiliser[Elastic Cloud](https://www.elastic.co/cloud/), Service cloud officiel d'Elastic. Dans ce guide, nous utiliserons la version cloud.
2. [Register](https://cloud.elastic.co/registration)un compte ou[login](https://cloud.elastic.co/login)avec le compte existant sur Elastic Cloud.

<gigne> <img src = "../../../. GitBook / Assets / Elastic1.png" alt = ""> <Figcaption> </gigcaption> </gigne>

3. Cliquez sur ** Créer un déploiement **. Ensuite, nommez votre déploiement et choisissez le fournisseur.

<gigne> <img src = "../../../. GitBook / Assets / Elastic2.png" alt = "" width = "563"> <Figcaption> </ Figcaption> </ Figure>

4. Une fois le déploiement terminé, vous devriez être en mesure de voir les guides de configuration comme indiqué ci-dessous. Cliquez sur l'option ** Configuration de la recherche vectorielle **.

<gigne> <img src = "../../../. GitBook / Assets / Elastic4.png" alt = ""> <Figcaption> </gigcaption> </gigne>

5. Vous devriez maintenant voir la page ** de démarrage ** pour ** Vector Search **.

<gigne> <img src = "../../../. GitBook / Assets / Elastic5.png" alt = ""> <Figcaption> </gigcaption> </ Figure>

6. Sur la barre gauche, cliquez sur ** Indices **. Ensuite, ** Créez un nouvel index **.

<gigne> <img src = "../../../. GitBook / Assets / Elastic6.png" alt = ""> <Figcaption> </ Figcaption> </gigne>

7. Sélectionnez ** API ** Méthode d'ingestion

<gigne> <img src = "../../../. GitBook / Assets / Elastic7.png" alt = ""> <Figcaption> </gigcaption> </gigne>

8. Nommez votre nom d'index de recherche, puis ** Créer un index **

<gigne> <img src = "../../../. GitBook / Assets / Elastic8.png" alt = ""> <Figcaption> </gigcaption> </gigust>

9. Une fois l'index créé, générez une nouvelle clé API, prenez note à la fois de la clé API générée et de l'URL

<gigne> <img src = "../../../. gitbook / actifs / élastique9

## Configuration de flux

1. Ajoutez un nouveau nœud ** elasticsearch ** sur toile et remplissez le nom ** index **

<gigne> <img src = "../../../. GitBook / Assets / Elastic10.png" alt = "" width = "275"> <Figcaption> </gigcaption> </gigust>

2. Ajouter de nouveaux informations d'identification via ** API Elasticsearch **

<gigne> <img src = "../../../. GitBook / Assets / Elastic11.png" alt = "" width = "429"> <Figcaption> </ Figcaption> </ Figure>

3. Prenez la clé URL et API d'Elasticsearch, remplissez les champs

<gigne> <img src = "../../../. GitBook / Assets / Elastic12.png" alt = "" width = "563"> <Figcaption> </gigcaption> </ Figure>

4. Une fois les informations d'identification créées avec succès, vous pouvez démarrer les données

<gigne> <img src = "../../../. Gitbook / Assets / Untitled (1) (1) (1) .png" alt = ""> <Figcaption> </gigcaption> </gigust>

<gigne> <img src = "../../../. GitBook / Assets / Elastic13.png" alt = ""> <Figcaption> </gigcaption> </gigust>

5. Une fois que les données ont été renversées avec succès, vous pouvez la vérifier à partir du tableau de bord élastique:

<gigne> <img src = "../../../. GitBook / Assets / Image (7) (1) (1) (1) (1) (1) (2) (1) .png" alt = ""> <figCaption> </gigcaption> </gigu

6. Le tour est joué! Vous pouvez maintenant commencer à poser une question dans le chat

<gigne> <img src = "../../../. GitBook / Assets / Image (6) (1) (1) (1) (1) (1) (1) (2) (1) .png" alt = ""> <figCaption> </ FigCaption> </pigucial>

## Ressources

* [LangChain JS Elastic](https://js.langchain.com/docs/integrations/vectorstores/elasticsearch)
* [Vector Search (kNN) Implementation Guide - API Edition](https://www.elastic.co/search-labs/blog/articles/vector-search-implementation-guide-api-edition)
