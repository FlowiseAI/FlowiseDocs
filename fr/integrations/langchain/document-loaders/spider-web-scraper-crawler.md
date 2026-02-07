---
description: Scrape & Crawl the web with Spider - the fastest open source web scraper & crawler.
---

# Spider web grattoir / Crawler

<gigne> <img src = "../../../. GitBook / Assets / spider.png" alt = "Spider Node" width = "365"> <figcaption> <p> spider web scraper / crawler nœud </p> </ figcaption> </pigucial>

[Spider](https://spider.cloud/?ref=flowise)est le grattoir et le robot d'open source le plus rapide qui renvoie les données pratiquées par LLM. Pour commencer à utiliser ce nœud, vous avez besoin d'une clé API à partir de[Spider.cloud](https://spider.cloud/?ref=flowise).

## Commencer

1. Aller au[Spider.cloud](https://spider.cloud/?ref=flowise)Site Web et inscrivez-vous à un compte gratuit.
2. Alors allez au[API Keys](https://spider.cloud/api-keys)et créer une nouvelle clé API.
3. Copiez la touche API et collez-le dans le champ "Contalin" dans le nœud Spider.

## Caractéristiques
- Deux modes d'opération: gratter et ramper
- Capacités de division de texte
- Manipulation des métadonnées personnalisables
- Configuration des paramètres flexibles
- Formats de sortie multiples
- Contenu formulé Markdown
- Manipulation des limites de taux

## Entrées

### Paramètres requis
- ** Mode **: Choisissez entre:
  - ** Scrape **: Extraire les données d'une seule page
  - ** Crawl **: Extraire les données de plusieurs pages dans le même domaine
- ** URL de la page Web **: L'URL cible pour gratter ou ramper (par exemple, https://spider.cloud)
- ** Prédiction **: clé API Spider

### Paramètres facultatifs
- ** Splitter du texte **: un séparateur de texte pour traiter le contenu extrait
- ** Limite **: Nombre maximum de pages à ramper (par défaut: 25, uniquement applicable en mode crawl)
- ** Métadonnées supplémentaires **: objet JSON avec des métadonnées supplémentaires à ajouter aux documents
- ** Paramètres supplémentaires **: objet JSON avec[Spider API parameters](https://spider.cloud/docs/api)
  - Exemple:`{ "anti_bot": true }`
  - Note:`return_format`est toujours réglé sur "Markdown"
- ** omettre les clés de métadonnées **: Liste des clés de métadonnées séparées par des virgules à exclure
  - Format:`key1, key2, key3.nestedKey1`
  - Utiliser * pour supprimer toutes les métadonnées par défaut

## Sorties

- ** Document **: tableau d'objets de document contenant:
  - métadonnées: métadonnées de page et champs personnalisés
  - Concontent: Contenu extrait au format Markdown
- ** Texte **: chaîne concaténée de tout contenu extrait

## Structure de document
Chaque document contient:
- ** PageContent **: Le contenu principal de la page Web au format Markdown
- ** Metadata **:
  - Source: L'URL de la page
  - Métadonnées personnalisées supplémentaires (si spécifiées)
  - Métadonnées filtrées (basées sur les clés omises)

## Exemples d'utilisation

### Grattage de base
```json
{
  "mode": "scrape",
  "url": "https://example.com",
  "limit": 1
}
```

### Rampant avancé
```json
{
  "mode": "crawl",
  "url": "https://example.com",
  "limit": 25,
  "additional_metadata": {
    "category": "blog",
    "source_type": "web"
  },
  "params": {
    "anti_bot": true,
    "wait_for": ".content-loaded"
  }
}
```

## Exemple

<gigne> <img src = "../../../. GitBook / Assets / Spider_Example_Usage.png" Alt = "Exemple sur Spider Node" Width = "365"> <Figcaption> <p> Exemple sur Spider Node </p> </gigcaption> </ Figure>

## Notes
- Le robotage respecte la limite spécifiée pour les opérations de crawl
- Tout le contenu est renvoyé au format Markdown
- La gestion des erreurs est intégrée à la fois pour les opérations de grattage et de rampe
- Les configurations JSON non valides sont traitées gracieusement
- Traitement économe en mémoire des grands sites Web
- Prend en charge l'extraction à une seule page et à plusieurs pages
- Manipulation automatique des métadonnées et filtrage
