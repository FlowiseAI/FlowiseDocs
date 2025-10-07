---
description: Learn how to scrape, upsert, and query a website
---

# Web strecle qna

***

Supposons que vous ayez un site Web (pourrait être un magasin, un site de commerce électronique, un blog), et que vous souhaitez supprimer tous les liens relatifs de ce site Web et demander à LLM de répondre à n'importe quelle question sur votre site Web. Dans ce tutoriel, nous allons passer par la façon d'y parvenir.

Vous pouvez trouver l'exemple de flux appelé - ** Page Web QNA ** à partir des modèles de marché.

## Installation

Nous allons utiliser ** Cheerio Web Scraper ** Node pour raconter des liens à partir d'une URL donnée et du séparateur de texte ** htmltomarkdown ** pour diviser le contenu gratté en petits morceaux.

<gigne> <img src = "../. GitBook / Assets / Image (86) .png" alt = ""> <Figcaption> </gigcaption> </gigust>

Si vous ne spécifiez rien, par défaut, seule la page URL donnée sera grattée. Si vous souhaitez ramper le reste des liens relatifs, cliquez sur ** Paramètres supplémentaires ** du grattoir Web Cheerio.

## 1. Crawl plusieurs pages

1. Sélectionner`Web Crawl`ou`Scrape XML Sitemap`Dans ** Get Relative Links Method **.
2. Saisir`0`Dans ** Obtenez des liens relatifs Limite ** Pour récupérer tous les liens disponibles à partir de l'URL fournie.

<gigne> <img src = "../. GitBook / Assets / Image (87) .png" alt = "" width = "563"> <Figcaption> </ Figcaption> </gigust>

### Gérer les liens (facultatif)

1. Entrée URL souhaitée à ramper.
2. Cliquez sur ** Répondre aux liens ** Pour récupérer les liens en fonction des entrées de la méthode ** Get Relative Links ** et ** Obtenir des liens relatifs Limite ** Dans ** Paramètres supplémentaires **.
3. Dans ** Liens rampés ** Section, supprimez les liens indésirables en cliquant sur ** Icône de bac à ordures rouges **.
4. Enfin, cliquez sur ** Enregistrer **.

<gigne> <img src = "../. GitBook / Assets / Image (88) .png" alt = "" width = "563"> <Figcaption> </ Figcaption> </gigust>

## 2. Upsert

1. Dans le coin supérieur droit, vous remarquerez un bouton vert:

<gigne> <img src = "../. GitBook / Assets / Untitled (2) .png" alt = ""> <Figcaption> </ Figcaption> </gigne>

2. Une boîte de dialogue sera affichée qui permettra aux utilisateurs d'infiltrer les données sur PineCone:

<gigne> <img src = "../. Gitbook / Assets / Image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (2) (1) .png" alt = ""> <figCaption> </gigcaption> </ Figure>

** Remarque: ** Sous le capot, les actions suivantes seront exécutées:

* Stracté toutes les données HTML à l'aide du grattoir Web Cheerio
* Convertir toutes les données grattées de HTML à Markdown, puis les diviser
* Les données divisées seront bouclées et converties en incorporations vectorielles à l'aide d'Openai Incorceddings
* Les incorporations vectorielles seront renversées sur Pincecone

3. Sur[Pinecone console](https://app.pinecone.io)Vous pourrez voir les nouveaux vecteurs qui ont été ajoutés.

<Figure> <img src = "../. GitBook / Assets / Web-Scrapeconcone.png" Alt = ""> <Figcaption> </Figcaption> </Figge>

## 3. Requête

La question est relativement simple. Une fois que vous avez vérifié que les données ont été déposées à la base de données vectorielle, vous pouvez commencer à poser une question dans le chat:

<gigne> <img src = "../. Gitbook / Assets / image (4) (1) (1) (1) (1) (1) (1) (1) (1) (1) (2) .png" alt = ""> <figcaption> </gigcaption> </ figure>

Dans les paramètres supplémentaires de la chaîne QA de récupération conversationnelle, vous pouvez spécifier 2 invites:

* ** Invite de reformularité: ** Utilisé pour reformuler la question compte tenu de l'historique de conversation passée
* ** Invite de réponse: ** À l'aide de la question reformatique, récupérez le contexte de la base de données vectorielle et renvoyez une réponse finale

<Figure> <img src = "../. GitBook / Assets / Image (91) .png" alt = ""> <Figcaption> </gigcaption> </gigust>

{% hint style = "info"%}
Il est recommandé de spécifier un message d'invite de réponse détaillé. Par exemple, vous pouvez spécifier le nom de l'IA, la langue pour répondre, la réponse lorsque la réponse n'est pas trouvée (pour empêcher l'hallucination).
{% EndHint%}

Vous pouvez également activer l'option Retour Source Documents pour renvoyer une liste de morceaux de document d'où vient la réponse de l'IA.

<gigne> <img src = "../. GitBook / Assets / Untitled (1) (1) (1) (1) .png" alt = "" width = "563"> <Figcaption> </ Figcaption> </stigne>

## Stracage Web supplémentaire

Outre le grattoir Web Cheerio, il existe également d'autres nœuds qui peuvent également effectuer le grattage Web:

* ** Puppeneer: ** Puppeteer est une bibliothèque Node.js qui fournit une API de haut niveau pour contrôler le chrome ou le chrome sans tête. Vous pouvez utiliser des marionnets pour automatiser les interactions de page Web, notamment l'extraction de données de pages Web dynamiques qui nécessitent un JavaScript.
* ** Playwright: ** Playwright est une bibliothèque Node.js qui fournit une API de haut niveau pour contrôler plusieurs moteurs de navigateur, y compris Chromium, Firefox et WebKit. Vous pouvez utiliser Playwright pour automatiser les interactions de la page Web, notamment l'extraction de données de pages Web dynamiques qui nécessitent un JavaScript.
* ** apify: **[Apify](https://apify.com/)est une plate-forme cloud pour le grattage Web et l'extraction de données, qui fournit un[ecosystem](https://apify.com/store)de plus d'un millier d'applications prêtes à l'emploi appelées _ACTORS_ pour divers cas d'utilisation de grattage Web, d'exploration et d'extraction de données.

<gigne> <img src = "../. GitBook / Assets / Image (92) .png" alt = ""> <Figcaption> </gigcaption> </gigust>

{% hint style = "info"%}
La même logique peut être appliquée à tous les cas d'utilisation de documents, et non seulement limitées au grattage Web!
{% EndHint%}

Si vous avez une suggestion sur la façon d'améliorer les performances, nous serions ravis de votre[contribution](broken-reference)!
