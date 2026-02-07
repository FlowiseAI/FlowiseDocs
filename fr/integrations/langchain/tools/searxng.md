---
description: Wrapper around SearXNG - a free internet metasearch engine.
---

# Searxng

<gigne> <img src = "../../../. GitBook / Assets / Up-011.png" alt = "" width = "283"> <Figcaption> <p> SearXng Node </p> </gigcaption> </gigust>

### Configuration de SearXng

Suivre[official documentation](https://docs.searxng.org/admin/installation.html)pour configurer SearXng localement. Dans ce cas, nous utiliserons Docker Compose pour le configurer.

Se diriger vers[searxng-docker](https://github.com/searxng/searxng-docker)Référentiel et suivez les instructions de configuration.

Assurez-vous que vous avez`server.limiter`se mettre à`false`et`json`est inclus dans`search.formats`. Ces paramètres peuvent être définis dans`searxng/settings.yml` :

```yaml
server:
  limiter: false
general:
  debug: true
search:
  formats:
    - html
    - json
```

`docker-compose up -d`Pour démarrer le conteneur. Ouvrez le navigateur Web et accédez à ** http: // localhost: 8080 / search **, vous pourrez voir la page SearXng.

### Utilisation de Flowise

Faites glisser et déposez le nœud SearXng sur le toile. Remplissez l'URL de base comme ** http: // localhost: 8080. ** Vous pouvez également spécifier d'autres paramètres de recherche si nécessaire. LLM déterminera automatiquement ce qu'il faut utiliser pour la question de la requête de recherche.

<gigne> <img src = "../../../. GitBook / Assets / Image (171) .png" alt = ""> <Figcaption> </gigcaption> </gigne>
