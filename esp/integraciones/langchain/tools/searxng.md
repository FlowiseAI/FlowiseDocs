---
description: Wrapper alrededor de SearXNG - un motor de metabúsqueda de internet gratuito.
---

# SearXNG

<figure><img src="../../../.gitbook/assets/up-011.png" alt="" width="283"><figcaption><p>Nodo SearXNG</p></figcaption></figure>

### Configurar SearXNG

Sigue la [documentación oficial](https://docs.searxng.org/admin/installation.html) para configurar SearXNG localmente. En este caso, usaremos Docker Compose para configurarlo.

Navega al repositorio [searxng-docker](https://github.com/searxng/searxng-docker) y sigue las instrucciones de configuración.

Asegúrate de que `server.limiter` esté configurado como `false` y que `json` esté incluido en `search.formats`. Estos parámetros se pueden definir en `searxng/settings.yml`:

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

Usa `docker-compose up -d` para iniciar el contenedor. Abre el navegador web y ve a **http://localhost:8080/search**, podrás ver la página de SearXNG.

### Uso en Flowise

Arrastra y suelta el nodo SearXNG en el canvas. Completa la Base URL como **http://localhost:8080.** También puedes especificar otros parámetros de búsqueda si es necesario. El LLM determinará automáticamente qué usar para la pregunta de consulta de búsqueda.

<figure><img src="../../../.gitbook/assets/image (171).png" alt=""><figcaption></figcaption></figure>
