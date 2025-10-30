---
description: Load data from Apify Website Content Crawler.
---

# Crawler de contenu du site Web Apify

<gigne> <img src = "../../../. GitBook / Assets / Image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (2). Nœud </p> </gigcaption> </ figure>

[Apify](https://apify.com/)Le Crawler de contenu du site Web est un puissant outil de grattage Web qui peut extraire le contenu des sites Web à l'aide de divers moteurs rampants. Ce module fournit une intégration avec le robot de contenu du site Web d'Apify pour charger et traiter le contenu Web.

Ce module fournit un robot Web sophistiqué qui peut:

* Crawl plusieurs sites Web à partir d'URL de démarrage spécifiée
* Utilisez différents moteurs rampants (Chrome, Firefox, Cheerio, JSDom)
* Contrôler la profondeur de rampe et les limites de page
* Gérer le contenu rendu javascript
* Processus du contenu extrait avec des séparateurs de texte
* Personnaliser l'extraction des métadonnées

## Entrées

### Paramètres requis

* ** Démarrer les URL **: Liste des URL séparée par des virgules où le rampage commencera
* ** Connectez API APIFIE **: APIFIE API Identifiés
* ** Type de chenille **: choix du moteur rampant:
  * Browser Web sans tête (Chrome + dramaturge)
  * Brefteur Web furtif (Firefox + Playwright)
  * Client HTTP brut (Cheerio)
  * Client HTTP brut avec exécution JavaScript (JSDom)

### Paramètres facultatifs

* ** Splitter du texte **: un séparateur de texte pour traiter le contenu extrait
* ** Profondeur maximale de rampe **: profondeur maximale des liens de page à suivre (par défaut: 1)
* ** PAGES DE CRAWLS MAX **: Nombre maximum de pages à ramper (par défaut: 3)
* ** Entrée supplémentaire **: objet JSON avec configuration supplémentaire de chenilles
* ** Metadata supplémentaires **: objet JSON avec métadonnées supplémentaires
* ** omettre les clés de métadonnées **: Liste des clés de métadonnées séparées par des virgules pour omettre

## Sorties

* ** Document **: tableau d'objets de document contenant des métadonnées et un conceptent
* ** Texte **: chaîne concaténée du conceptent de documents

## Caractéristiques

* Prise en charge du moteur rampant multiple
* Paramètres rampant configurables
* Support de rendu javascript
* Contrôles de limite de profondeur et de page
* Personnalisation des métadonnées
* Capacités de division de texte
* Gestion des erreurs

## Types de chenilles

### Chrome sans tête (dramaturge)

* Meilleur pour les applications Web modernes
* Prise en charge JavaScript complète
* Utilisation des ressources plus élevée

### Firefox furtif (dramaturge)

* Bon pour les sites avec détection de bot
* Prise en charge JavaScript complète
* Fonctionnement plus furtif

### Rafale

* Rapide et léger
* Pas de support JavaScript
* Utilisation des ressources inférieures

### Jsdom (expérimental)

* Prise en charge de l'exécution JavaScript
* Alternative légère aux navigateurs
* Caractéristiques expérimentales

## Notes

* Nécessite un jeton API APIFY valide
* Différents types de chenilles ont des capacités différentes
* L'utilisation des ressources varie selon le type de chenille
* Le support JavaScript dépend du type de chenille
* La limitation des taux peut s'appliquer sur la base du plan Apify
* Configuration supplémentaire disponible via l'entrée JSON

## Crawl entièrement le site Web

1. _ (Facultatif) _ Connectez-vous[**Text Splitter**](../text-splitters/).
2. Connectez l'API APIFY (créez un nouvel titulaire avec votre[Apify API token](https://my.apify.com/account#/integrations)).
3. Entrez une ou plusieurs URL (séparées par des virgules) où le robotage commencera, par exemple`https://docs.flowiseai.com/`.
4. Sélectionnez le type de robot. Se référer à[Website Content Crawler documentation for more information](https://apify.com/apify/website-content-crawler/input-schema#crawlerType).
5. _ (Facultatif) _ Spécifiez des paramètres supplémentaires tels que la profondeur de rampe maximale et le nombre maximal de pages à ramper.

## Sortir

Charge le contenu du site Web en tant que document.

## Ressources

* [Apify-Flowise integration](https://docs.apify.com/platform/integrations/flowise)
* [Website Content Crawler](https://apify.com/apify/website-content-crawler)
