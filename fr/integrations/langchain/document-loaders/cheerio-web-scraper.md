# Reccardage de cheerio

Cheerio est une implémentation rapide, flexible et maigre de Core JQuery conçu spécifiquement pour le serveur. Ce module offre de puissantes capacités de grattage Web utilisant Cheerio pour extraire le contenu des pages Web.

Ce module fournit un grattoir Web sophistiqué qui peut:

* Chargez le contenu à partir de pages Web uniques ou multiples
* Crawl liens relatifs des sites Web
* Extraire du contenu à l'aide de sélecteurs CSS
* Gérer des sitemaps XML
* Traiter le contenu Web avec des séparateurs de texte

## Entrées

* ** URL **: L'URL de la page Web pour gratter
* ** Splitter de texte ** (facultatif): un séparateur de texte pour traiter le contenu extrait
* ** Obtenez la méthode des liens relatifs ** (facultatif): Choisissez entre:
  * Crawl Web: Crawl Liens relatifs de l'URL HTML
  * Gratter le plan du site XML: Racler les liens relatifs de l'URL du site XML
* ** Obtenez des liens relatifs Limite ** (Facultatif): Limite pour le nombre de liens relatifs à traiter (par défaut: 10, 0 pour tous les liens)
* ** Sélecteur (CSS) ** (Facultatif): sélecteur CSS pour cibler un contenu spécifique
* ** Métadonnées supplémentaires ** (Facultatif): objet JSON avec des métadonnées supplémentaires à ajouter aux documents
* ** omettre les clés de métadonnées ** (facultative): liste de clés de métadonnées séparées par des virgules pour omettre

## Sorties

* ** Document **: tableau d'objets de document contenant des métadonnées et un conceptent
* ** Texte **: chaîne concaténée du conceptent de documents

## Caractéristiques

* Extraction de contenu basée sur le sélecteur CSS
* Capacités d'exploration sur le Web
* Traitement du plan du site XML
* Limites de liaison configurables
* Gestion des erreurs pour les URL et les PDF non valides
* Personnalisation des métadonnées
* Débogage du support de journalisation

## Notes

* Les fichiers PDF ne sont pas pris en charge et seront ignorés
* Les URL non valides lanceront une erreur
* La définition de la limite des liens à 0 récupérera tous les liens disponibles (peut prendre plus de temps)
* Le mode de débogage fournit une journalisation détaillée du processus de grattage

## Gratter une URL

1. _ (Facultatif) _ Connectez-vous[**Text Splitter**](../text-splitters/).
2. Entrée URL souhaitée à gratter.

## Crawl et gratter plusieurs URL

1. Sélectionner`Web Crawl`ou`Scrape XML Sitemap`Dans ** Get Relative Links Method **.
2. Saisir`0`Dans ** Obtenez des liens relatifs Limite ** Pour récupérer tous les liens disponibles à partir de l'URL fournie.

<gigne> <img src = "../../../. GitBook / Assets / Image (87) .png" alt = "" width = "563"> <Figcaption> </ Figcaption> </ Figure>

### Gérer les liens (facultatif)

1. Entrée URL souhaitée à ramper.
2. Cliquez sur ** Répondre aux liens ** Pour récupérer les liens en fonction des entrées de la méthode ** Get Relative Links ** et ** Obtenir des liens relatifs Limite ** Dans ** Paramètres supplémentaires **.
3. Dans ** Liens rampés ** Section, supprimez les liens indésirables en cliquant sur ** Icône de bac à ordures rouges **.
4. Enfin, cliquez sur ** Enregistrer **.

<gigne> <img src = "../../../. GitBook / Assets / Image (88) .png" alt = "" width = "563"> <Figcaption> </ Figcaption> </ Figure>

## Sortir

Charge le contenu de l'URL en tant que document

## Ressources

* [LangChain JS Cheerio](https://js.langchain.com/docs/integrations/document_loaders/web_loaders/web_cheerio)
* [Cheerio](https://cheerio.js.org/)
