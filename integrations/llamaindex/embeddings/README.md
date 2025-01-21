---
description: Nodos de Embeddings de LlamaIndex
---

# Embeddings

***

Un embedding es un vector (lista) de números de punto flotante. La distancia entre dos vectores mide su relación. Distancias pequeñas sugieren una alta relación y distancias grandes sugieren una baja relación.

Los embeddings pueden usarse para crear una representación numérica de datos textuales. Esta representación numérica es útil porque puede usarse para encontrar documentos similares.

Se utilizan comúnmente para:

* Búsqueda (donde los resultados se clasifican por relevancia a una consulta)
* Agrupación (donde las cadenas de texto se agrupan por similitud)
* Recomendaciones (donde se recomiendan elementos con cadenas de texto relacionadas)
* Detección de anomalías (donde se identifican valores atípicos con poca relación)
* Medición de diversidad (donde se analizan las distribuciones de similitud)
* Clasificación (donde las cadenas de texto se clasifican por su etiqueta más similar)

### Nodos de Embeddings:

* [Azure OpenAI Embeddings](azure-openai-embeddings.md)
* [OpenAI Embedding](openai-embedding.md)
