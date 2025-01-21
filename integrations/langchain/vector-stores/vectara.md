# Vectara

## Tutorial de Inicio Rápido

{% embed url="https://www.youtube.com/watch?v=rBqpvFcD5XY" %}

## Prerequisitos

1. Registra una cuenta en [Vectara](https://vectara.com/integrations/flowise)
2. Haz clic en **Create Corpus**

<figure><img src="../../../.gitbook/assets/vectara/1.png" alt=""><figcaption></figcaption></figure>

Nombra el corpus a crear y haz clic en **Create Corpus**, luego espera a que el corpus termine de configurarse.

## Configuración

1. Haz clic en la pestaña **"Access Control"** en la vista del corpus

<figure><img src="../../../.gitbook/assets/vectara/2.png" alt=""><figcaption></figcaption></figure>

2. Haz clic en el botón **"Create API Key"**, elige un nombre para la API key y selecciona la opción **QueryService & IndexService**

<figure><img src="../../../.gitbook/assets/vectara/3.png" alt=""><figcaption></figcaption></figure>

3. Haz clic en **Create** para crear la API key
4. Obtén tu **Corpus ID, API Key, y Customer ID** haciendo clic en la flecha hacia abajo bajo "copy" para tu nueva API key:

<figure><img src="../../../.gitbook/assets/vectara/4.png" alt=""><figcaption></figcaption></figure>

5. De vuelta al canvas de Flowise, crea tu chatflow. Haz clic en **Create New** desde el menú desplegable de Credentials e ingresa tus credenciales de Vectara.

<figure><img src="../../../.gitbook/assets/vectara/5.png" alt="" width="500"><figcaption></figcaption></figure>

6. ¡Disfruta!

## Parámetros de Consulta de Vectara

Para un control más preciso sobre los parámetros de consulta de Vectara, haz clic en "**Additional Parameters**" y podrás actualizar los siguientes parámetros desde sus valores por defecto:

* Metadata Filter: Vectara soporta filtrado de metadata. Para usar el [filtrado](https://docs.vectara.com/docs/common-use-cases/filtering-by-metadata/filter-overview), asegúrate de que los campos de metadata que deseas filtrar estén definidos en tu corpus de Vectara.
* "Sentences before" y "Sentences after": estos controlan cuántas oraciones antes/después del texto coincidente son devueltas como resultados del motor de recuperación de Vectara
* Lambda: define el comportamiento de la [búsqueda híbrida](https://docs.vectara.com/docs/learn/hybrid-search) en Vectara
* Top-K: cuántos resultados devolver de Vectara para la consulta
* MMR-K: número de resultados a usar para [MMR](https://docs.vectara.com/docs/api-reference/search-apis/reranking#maximal-marginal-relevance-mmr-reranker) (relevancia marginal máxima)

<figure><img src="../../../.gitbook/assets/vectara/6.png" alt="" width="500"><figcaption></figcaption></figure>

## Recursos

* [LangChain JS Vectara Blog Post](https://blog.langchain.dev/langchain-vectara-better-together/)
* [5 Razones para Usar la Integración de Vectara con Langchain Blog Post](https://vectara.com/5-reasons-to-use-vectaras-langchain-integration/)
* [Relevancia Marginal Máxima en Vectara](https://vectara.com/blog/get-diverse-results-and-comprehensive-summaries-with-vectaras-mmr-reranker/)
* [Blog Post sobre el modelo de embedding Boomerang de Vectara](https://vectara.com/introducing-boomerang-vectaras-new-and-improved-retrieval-model/)
* [Detectando Alucinaciones con HHEM de Vectara](https://vectara.com/blog/cut-the-bull-detecting-hallucinations-in-large-language-models/)
