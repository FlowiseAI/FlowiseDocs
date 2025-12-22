# CHIFFON

Les modèles de grandes langues (LLMS) ont débloqué le potentiel de création de chatbots Q \ & A avancés capables de fournir des réponses précises en fonction du contenu spécifique. Ces systèmes reposent sur une méthode appelée génération (RAG) (RAG) de la récupération, ce qui améliore leurs réponses en les ancrant dans le matériel source pertinent.

Dans ce tutoriel, vous apprendrez à créer une application Q \ & A de base qui peut extraire et répondre aux questions à partir de sources de documents données.

Le processus peut être séparé en 2 sous-processus:

* Indexage
* Récupération

## Indexage

[Document Stores](../using-flowise/document-stores.md)est conçu pour aider à l'ensemble des pipelines d'indexation - récupérer des données à partir de différentes sources, une stratégie de section, une augmentation de la base de données vectorielle, une synchronisation avec des données mises à jour.

Nous prenons en charge un large éventail de chargeurs de documents, allant de fichiers comme PDF, Word, Google Drive, vers des grabyers Web comme Playwright, Firecrawl, Apify et autres. Vous pouvez également créer un chargeur de documents personnalisé.

<gigne> <img src = "../. GitBook / Assets / Image (293) .png" alt = "" width = "563"> <Figcaption> </ Figcaption> </gigust>

## Récupération

En fonction de l'entrée de l'utilisateur, les morceaux de document pertinents sont récupérés à partir de la base de données vectorielle. LLM utilise ensuite le contexte récupéré pour générer une réponse.

1. Faites glisser et déposez un[Agent](../using-flowise/agentflowv2.md#id-3.-agent-node)Node et configurez le modèle à utiliser.

<gigne> <img src = "../. GitBook / Assets / Image (290) .png" alt = "" width = "391"> <Figcaption> </gigcaption> </ Figure>

2. Ajoutez une nouvelle connaissance (magasin de documents) et définissez ce qu'est le contenu. Cela aide le LLM à comprendre quand et comment récupérer les informations pertinentes. Vous pouvez également utiliser le bouton de génération automatique pour aider à ce processus.

{% Hint Style = "Success"%}
Seul le magasin de documents renversé peut être utilisé
{% EndHint%}

<gigne> <img src = "../. GitBook / Assets / Image (288) .png" alt = "" width = "482"> <Figcaption> </gigcaption> </gigust>

3. (Facultatif) Si les données ont déjà été stockées dans une base de données vectorielle sans passer par le pipeline d'indexation du magasin de documents, vous pouvez également vous connecter directement à la base de données vectorielle et au modèle d'intégration.

<gigne> <img src = "../. GitBook / Assets / Image (289) .png" alt = "" width = "388"> <Figcaption> </gigcaption> </gigne>

4. Ajoutez une invite système ou utilisez le bouton ** générer ** pour aider. Nous vous recommandons de l'utiliser, car cela aide à élaborer une invite plus efficace et optimisée.

<gigne> <img src = "../. GitBook / Assets / Image (294) .png" alt = "" width = "482"> <Figcaption> </gigcaption> </ Figure>

<gigne> <img src = "../. GitBook / Assets / Image (292) .png" alt = "" width = "563"> <Figcaption> </gigcaption> </ Figure>

5. Votre agent de chiffon est maintenant prêt à l'emploi!

## Ressources

{% embed url = "https://youtu.be/khc0cloiv0a?si=mezjydm8bt2imkjy"%}
