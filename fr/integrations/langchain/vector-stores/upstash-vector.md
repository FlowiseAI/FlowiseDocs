# Aubier

## Préquis

1. Inscrivez-vous ou connectez-vous à[Upstash Console](https://console.upstash.com)
2. Accédez à la page vectorielle et cliquez sur ** Créer un index **
<gigne> <img src = "../../../. GitBook / Assets / Upstash / list-index.jpeg" alt = ""> <figcaption> </ figCaption> </gigust>
3. Effectuez les configurations nécessaires et créez l'index.

   1. ** Nom de l'index **, nom de l'index à créer. (par exemple "Flowise Upstash-Demo")
   2. ** Dimensions **, taille des vecteurs à insérer dans l'index. (par exemple 1536)
   3. ** Modèle d'intégration **, le modèle à utiliser dans[Upstash Embeddings](https://upstash.com/docs/vector/features/embeddingmodels). C'est facultatif. Si vous l'activez, vous n'avez pas besoin de fournir un modèle d'intégration.

<gigne> <img src = "../../../. GitBook / Assets / Upstash / Create-index.jpeg" alt = ""> <figcaption> </gigcaption> </gigust>

## Installation

1.  Obtenez vos informations d'identification d'index

<gigne> <img src = "../../../. GitBook / Assets / Upstash / Env-Variables.jpeg" Alt = ""> <Figcaption> </gigcaption> </ Figure>

1. Créez de nouveaux informations d'identification Upstash Vector et remplissez
   1. URL de repos vectoriel upstash de Upstash_Vector_Rest_url sur la console
   2. Jeton de repos vectoriel upstash de Upstash_Vector_Rest_Token sur la console

<gigne> <img src = "../../../. GitBook / Assets / Upstash / Credentials.jpeg" alt = "" width = "563"> <Figcaption> </ Figcaption> </ Figure>

1.  Ajouter un nouveau nœud Vector ** Vector ** à toile

<gigne> <img src = "../../../. GitBook / Assets / Upstash / Upstash-node.jpeg" alt = "" width = "279"> <Figcaption> </ Figcaption> </gistre>

1. Ajouter des nœuds supplémentaires à la toile et démarrer le processus ussert
   - ** Document ** peut être connecté à n'importe quel nœud sous[**Document Loader**](../document-loaders/)catégorie
   - ** Embeddings ** peut être connecté à n'importe quel nœud sous[**Embeddings** ](../embeddings/)catégorie

<gigne> <img src = "../../../. GitBook / Assets / Upstash / Flowise-DeSign.jpeg" Alt = ""> <Figcaption> </gigcaption> </gigust>

1. Vérifier[Upstash dashboard](https://console.upstash.com)Pour voir si les données ont été mises à jour avec succès:

<gigne> <img src = "../../../. gitbook / actifs / upstash / databrowser.jpeg" alt = ""> <figcaption> </gigcaption> </ figure>
