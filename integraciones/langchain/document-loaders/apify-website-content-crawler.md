---
description: Carga datos desde el Rastreador de Contenido Web de Apify.
---

# Rastreador de Contenido Web Apify

[Apify](https://apify.com/) es una plataforma de web scraping y extracción de datos que proporciona una tienda de aplicaciones con más de mil herramientas listas para usar llamadas Actors.

El Actor [Website Content Crawler](https://apify.com/apify/website-content-crawler) puede rastrear sitios web en profundidad, limpiar su HTML eliminando modales de cookies, pies de página o navegación, y luego transformar el HTML en Markdown. Este Markdown puede ser almacenado en una base de datos vectorial para búsqueda semántica o Generación Aumentada por Recuperación (RAG).

<figure><img src="../../../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="266"><figcaption><p>Nodo del Rastreador de Contenido Web Apify</p></figcaption></figure>

## Rastrear Sitio Web Completo

1. _(Opcional)_ Conecta [**Text Splitter**](../text-splitters/).
2. Conecta Apify API (crea una nueva credencial con tu [token de API de Apify](https://my.apify.com/account#/integrations)).
3. Ingresa una o más URLs (separadas por comas) donde el rastreador comenzará, por ejemplo `https://docs.flowiseai.com/`.
4. Selecciona el tipo de rastreador. Consulta la [documentación de Website Content Crawler para más información](https://apify.com/apify/website-content-crawler/input-schema#crawlerType).
5. _(Opcional)_ Especifica parámetros adicionales como la profundidad máxima de rastreo y el número máximo de páginas a rastrear.

## Salida

Carga el contenido del sitio web como un Documento.

## Recursos

* [Integración Apify-Flowise](https://docs.apify.com/platform/integrations/flowise)
* [Website Content Crawler](https://apify.com/apify/website-content-crawler)
