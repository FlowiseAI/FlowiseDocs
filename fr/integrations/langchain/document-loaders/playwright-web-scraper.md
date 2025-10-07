# Scraper Web du dramaturge

Le dramaturge est une bibliothèque puissante pour l'automatisation du navigateur qui peut contrôler le chrome, Firefox et WebKit avec une seule API. Ce module fournit des capacités de grattage Web avancées à l'aide de Playwright pour extraire le contenu des pages Web, y compris du contenu dynamique qui nécessite une exécution JavaScript.

Ce module fournit un grattoir Web sophistiqué qui peut:
- Chargez le contenu à partir de pages Web uniques ou multiples
- Gérer le contenu rendu javascript
- Soutenez diverses stratégies de chargement de pages
- Attendez que des éléments spécifiques se chargent
- Crawl liens relatifs des sites Web
- Processus des sitemaps XML

## Entrées

- ** URL **: L'URL de la page Web pour gratter
- ** Splitter de texte ** (facultatif): un séparateur de texte pour traiter le contenu extrait
- ** Obtenez la méthode des liens relatifs ** (facultatif): Choisissez entre:
  - Crawl Web: Crawl Liens relatifs de l'URL HTML
  - Gratter le plan du site XML: Racler les liens relatifs de l'URL du site XML
- ** Obtenez des liens relatifs Limite ** (Facultatif): Limite pour le nombre de liens relatifs à traiter (par défaut: 10, 0 pour tous les liens)
- ** Attendez jusqu'à ** (facultatif): Stratégie de chargement de la page:
  - Charge: attendez que l'événement de chargement tire
  - Contenu DOM Chargé: attendez l'événement DomContent Télélé
  - Network Inactive: attendez qu'aucune connexion réseau pendant 500 ms
  - Commit: attendez la réponse initiale du réseau et le chargement des documents
- ** Attendez que le sélecteur charge ** (facultatif): le sélecteur CSS attend avant de gratter
- ** Métadonnées supplémentaires ** (Facultatif): objet JSON avec des métadonnées supplémentaires à ajouter aux documents
- ** omettre les clés de métadonnées ** (facultative): liste de clés de métadonnées séparées par des virgules pour omettre

## Sorties

- ** Document **: tableau d'objets de document contenant des métadonnées et un conceptent
- ** Texte **: chaîne concaténée du conceptent de documents

## Caractéristiques
- Prise en charge du moteur multi-navigateur (Chromium, Firefox, Webkit)
- Prise en charge de l'exécution JavaScript
- Stratégies de chargement de page configurables
- Capacités d'attente des éléments
- Fonctionnalité de rampe sur le Web
- Traitement du plan du site XML
- Opération de navigateur sans tête
- Configuration de bac à sable
- Gestion des erreurs pour les URL non valides
- Personnalisation des métadonnées

## Notes
- S'exécute en mode sans tête par défaut
- Utilise le mode sans sandbox pour la compatibilité
- Les URL non valides lanceront une erreur
- La définition de la limite des liens à 0 récupérera tous les liens disponibles (peut prendre plus de temps)
- Prend en charge l'attente d'éléments DOM spécifiques avant l'extraction

## Gratter une URL

1.  _ (Facultatif) _ connecter **[Text Splitter](../text-splitters/)**.
2. Entrée URL souhaitée à gratter.

## Crawl et gratter plusieurs URL
Visite **[Web Crawl](../../use-cases/web-crawl.md)** Guide pour permettre le grattage de plusieurs pages.

## Ressources

* [LangChain JS Playwright](https://js.langchain.com/docs/integrations/document_loaders/web_loaders/web_playwright)
* [Playwright](https://playwright.dev/)
