---
description: Aprende cómo hacer upsert de datos a Vector Stores con Flowise
---

# Upserting Data

***

Hay dos formas fundamentales de hacer upsert de tus datos en un [Vector Store](../integrations/langchain/vector-stores/) usando Flowise, ya sea mediante [llamadas API](../using-flowise/api.md#id-2.-vector-upsert-api) o usando un conjunto de nodos dedicados que tenemos listos para este propósito.

En esta guía, aunque es **altamente recomendado** que prepares tus datos usando los [Document Stores](../using-flowise/document-stores.md) antes de hacer upsert a un Vector Store, recorreremos todo el proceso usando los nodos específicos requeridos para este fin, describiendo los pasos, ventajas de este enfoque y estrategias de optimización para un manejo eficiente de datos.

## Entendiendo el proceso de upsert

Lo primero que necesitamos entender es que el proceso de upsert de datos a un [Vector Store](../integrations/langchain/vector-stores/) es una pieza fundamental para la formación de un sistema de [Retrieval Augmented Generation (RAG)](multiple-documents-qna.md). Sin embargo, una vez que este proceso finaliza, el RAG puede ejecutarse independientemente.

En otras palabras, en Flowise puedes hacer upsert de datos sin una configuración RAG completa, y puedes ejecutar tu RAG sin los nodos específicos usados en el proceso de upsert, lo que significa que aunque un vector store bien poblado es crucial para que RAG funcione, los procesos reales de recuperación y generación no requieren upsert continuo.

<figure><img src="../.gitbook/assets/ud_01.png" alt=""><figcaption><p>Upsert vs. RAG</p></figcaption></figure>

## Configuración

Digamos que tenemos un conjunto de datos extenso en formato PDF que necesitamos hacer upsert a nuestro [Upstash Vector Store](../integrations/langchain/vector-stores/upstash-vector.md) para poder instruir a un LLM que recupere información específica de ese documento.

Para hacer esto, y para ilustrar este tutorial, necesitaríamos crear un **flujo de upsert** con 5 nodos diferentes:

<figure><img src="../.gitbook/assets/UD_02.png" alt=""><figcaption><p>Flujo de Upsert</p></figcaption></figure>

## 1. Document Loader

El primer paso es **cargar nuestros datos PDF en la instancia de Flowise** usando un nodo [Document Loader](../integrations/langchain/document-loaders/). Los Document Loaders son nodos especializados que manejan la ingesta de varios formatos de documentos, incluyendo **PDFs**, **TXT**, **CSV**, páginas de Notion y más.

Es importante mencionar que cada Document Loader viene con dos **parámetros adicionales** importantes que nos permiten agregar y omitir metadatos de nuestro conjunto de datos a voluntad.

<figure><img src="../.gitbook/assets/UD_03.png" alt="" width="375"><figcaption><p>Parámetros Adicionales</p></figcaption></figure>

{% hint style="info" %}
**Consejo**: Los parámetros de agregar/omitir metadatos, aunque son opcionales, son muy útiles para identificar nuestro conjunto de datos una vez que se ha hecho upsert en un Vector Store o para eliminar metadatos innecesarios del mismo.
{% endhint %}

## 2. Text Splitter

Una vez que hemos cargado nuestro PDF o conjunto de datos, necesitamos **dividirlo en piezas más pequeñas, documentos o chunks**. Este es un paso de preprocesamiento crucial por 2 razones principales:

* **Velocidad y relevancia de recuperación:** Almacenar y consultar documentos grandes como entidades únicas en una base de datos vectorial puede llevar a tiempos de recuperación más lentos y resultados potencialmente menos relevantes. Dividir el documento en chunks más pequeños permite una recuperación más dirigida. Al consultar contra unidades de información más pequeñas y enfocadas, podemos lograr tiempos de respuesta más rápidos y mejorar la precisión de los resultados recuperados.
* **Costo-efectivo:** Ya que solo recuperamos chunks relevantes en lugar del documento completo, el número de tokens procesados por el LLM se reduce significativamente. Este enfoque de recuperación dirigida se traduce directamente en menores costos de uso para nuestro LLM, ya que la facturación típicamente se basa en el consumo de tokens. Al minimizar la cantidad de información irrelevante enviada al LLM, también optimizamos el costo.

### Nodos

En Flowise, este proceso de división se logra usando los nodos [Text Splitter](../integrations/langchain/text-splitters/). Estos nodos proporcionan una variedad de estrategias de segmentación de texto, incluyendo:

* **Character Text Splitting:** Dividiendo el texto en chunks de un número fijo de caracteres. Este método es directo pero puede dividir palabras o frases entre chunks, potencialmente interrumpiendo el contexto.
* **Token Text Splitting:** Segmentando el texto basado en límites de palabras o esquemas de tokenización específicos del modelo de embedding elegido. Este enfoque a menudo lleva a chunks más coherentes semánticamente, ya que preserva los límites de las palabras y considera la estructura lingüística subyacente del texto.
* **Recursive Character Text Splitting:** Esta estrategia busca dividir el texto en chunks que mantengan la coherencia semántica mientras permanecen dentro de un límite de tamaño especificado. Es particularmente adecuada para documentos jerárquicos con secciones o encabezados anidados. En lugar de dividir ciegamente en el límite de caracteres, analiza recursivamente el texto para encontrar puntos de ruptura lógicos, como finales de oraciones o saltos de sección. Este enfoque asegura que cada chunk represente una unidad significativa de información, incluso si excede ligeramente el tamaño objetivo.
* **Markdown Text Splitter:** Diseñado específicamente para documentos con formato markdown, este divisor segmenta lógicamente el texto basado en encabezados markdown y elementos estructurales, creando chunks que corresponden a secciones lógicas dentro del documento.
* **Code Text Splitter:** Adaptado para dividir archivos de código, esta estrategia considera la estructura del código, definiciones de funciones y otros elementos específicos del lenguaje de programación para crear chunks significativos que son adecuados para tareas como búsqueda de código y documentación.
* **HTML-to-Markdown Text Splitter:** Este divisor especializado primero convierte el contenido HTML a Markdown y luego aplica el Markdown Text Splitter, permitiendo la segmentación estructurada de páginas web y otros documentos HTML.

Los nodos Text Splitter proporcionan control granular sobre la segmentación de texto, permitiendo la personalización de parámetros como:

* **Chunk Size:** El tamaño máximo deseado de cada chunk, usualmente definido en caracteres o tokens.
* **Chunk Overlap:** El número de caracteres o tokens a superponer entre chunks consecutivos, útil para mantener el flujo contextual entre chunks.

{% hint style="info" %}
**Consejo:** Ten en cuenta que los valores de Chunk Size y Chunk Overlap no son aditivos. Seleccionar `chunk_size=1200` y `chunk_overlap=400` no resulta en un tamaño total de chunk de 1600. El valor de overlap determina el número de tokens del chunk anterior incluidos en el chunk actual para mantener el contexto. No aumenta el tamaño general del chunk.
{% endhint %}

### Entendiendo Chunk Overlap

En el contexto de recuperación basada en vectores y consultas LLM, el chunk overlap juega un **papel importante en mantener la continuidad contextual** y **mejorar la precisión de respuesta**, especialmente cuando se trata de profundidad de recuperación limitada o **top K**, que es el parámetro que determina el número máximo de chunks más similares que se recuperan del [Vector Store](../integrations/langchain/vector-stores/) en respuesta a una consulta.

Durante el procesamiento de consultas, el LLM ejecuta una búsqueda de similitud contra el Vector Store para recuperar los chunks semánticamente más relevantes para la consulta dada. Si la profundidad de recuperación, representada por el parámetro top K, está establecida en un valor pequeño, 4 por defecto, el LLM inicialmente usa información solo de estos 4 chunks para generar su respuesta.

Este escenario nos presenta un problema, ya que confiar únicamente en un número limitado de chunks sin superposición puede llevar a respuestas incompletas o inexactas, particularmente cuando se trata de consultas que requieren información que abarca múltiples chunks.

El chunk overlap ayuda con este problema asegurando que una porción del contexto textual se comparta entre chunks consecutivos, **aumentando la probabilidad de que toda la información relevante para una consulta dada esté contenida dentro de los chunks recuperados**.

En otras palabras, esta superposición sirve como un puente entre chunks, permitiendo al LLM acceder a una ventana contextual más amplia incluso cuando está limitado a un pequeño conjunto de chunks recuperados (top K). Si una consulta se relaciona con un concepto o pieza de información que se extiende más allá de un solo chunk, las regiones superpuestas aumentan la probabilidad de capturar todo el contexto necesario.

Por lo tanto, al introducir chunk overlap durante la fase de división de texto, mejoramos la capacidad del LLM para:

1. **Preservar la continuidad contextual:** Los chunks superpuestos proporcionan una transición más suave de información entre segmentos consecutivos, permitiendo al modelo mantener una comprensión más coherente del texto.
2. **Mejorar la precisión de recuperación:** Al aumentar la probabilidad de capturar toda la información relevante dentro de los chunks top K recuperados objetivo, el overlap contribuye a respuestas más precisas y contextualmente apropiadas.

### Precisión vs. Costo

Entonces, para optimizar aún más el equilibrio entre precisión de recuperación y costo, se pueden usar dos estrategias principales:

1. **Aumentar/Disminuir Chunk Overlap:** Ajustar el porcentaje de overlap durante la división de texto permite un control fino sobre la cantidad de contexto compartido entre chunks. Porcentajes de overlap más altos generalmente llevan a una mejor preservación del contexto pero también pueden aumentar los costos ya que necesitarías usar más chunks para abarcar todo el documento. Por el contrario, porcentajes de overlap más bajos pueden reducir costos pero arriesgan perder información contextual clave entre chunks, potencialmente llevando a respuestas menos precisas o incompletas del LLM.
2. **Aumentar/Disminuir Top K:** Elevar el valor top K por defecto (4) expande el número de chunks considerados para la generación de respuestas. Si bien esto puede mejorar la precisión, también aumenta el costo.

{% hint style="info" %}
**Consejo:** La elección de valores óptimos de **overlap** y **top K** depende de factores como la complejidad del documento, características del modelo de embedding y el balance deseado entre precisión y costo. La experimentación con estos valores es importante para encontrar la configuración ideal para una necesidad específica.
{% endhint %}

## 3. Embedding

Ahora hemos cargado nuestro conjunto de datos y configurado cómo se van a dividir nuestros datos antes de hacer upsert a nuestro [Vector Store](../integrations/langchain/vector-stores/). En este punto, [los nodos de embedding](../integrations/langchain/embeddings/) entran en juego, **convirtiendo todos esos chunks en un "lenguaje" que un LLM puede entender fácilmente**.

En este contexto actual, embedding es el proceso de convertir texto en una representación numérica que captura su significado. Esta representación numérica, también llamada vector de embedding, es un array multidimensional de números, donde cada dimensión representa un aspecto específico del significado del texto.

Estos vectores permiten a los LLMs comparar y buscar piezas similares de texto dentro del vector store midiendo la distancia o similitud entre ellos en este espacio multidimensional.

### Entendiendo las dimensiones de Embeddings/Vector Store

El número de dimensiones en un índice de Vector Store está determinado por el modelo de embedding usado cuando hacemos upsert de nuestros datos, y viceversa. Cada dimensión representa una característica o concepto específico dentro de los datos. Por ejemplo, una **dimensión** podría **representar un tema particular, sentimiento u otro aspecto del texto**.

Cuantas más dimensiones usemos para embeber nuestros datos, mayor será el potencial para capturar significado matizado de nuestro texto. Sin embargo, este aumento viene al costo de mayores requisitos computacionales por consulta.

En general, un mayor número de dimensiones necesita más recursos para almacenar, procesar y comparar los vectores de embedding resultantes. Por lo tanto, modelos de embeddings como el Google `embedding-001`, que usa 768 dimensiones, son, en teoría, más baratos que otros como el OpenAI `text-embedding-3-large`, con 3072 dimensiones.

Es importante notar que la **relación entre dimensiones y captura de significado no es estrictamente lineal**; hay un punto de rendimientos decrecientes donde agregar más dimensiones proporciona beneficios insignificantes para el costo innecesario añadido.

{% hint style="info" %}
**Consejo:** Para asegurar la compatibilidad entre un modelo de embedding y un índice de Vector Store, la alineación dimensional es esencial. Tanto **el modelo como el índice deben utilizar el mismo número de dimensiones para la representación vectorial**. La incompatibilidad de dimensionalidad resultará en errores de upsert, ya que el Vector Store está diseñado para manejar vectores de un tamaño específico determinado por el modelo de embedding elegido.
{% endhint %}

## 4. Vector Store

El nodo [Vector Store](../integrations/langchain/vector-stores/) es el **nodo final de nuestro flujo de upsert**. Actúa como el puente entre nuestra instancia de Flowise y nuestra base de datos vectorial, permitiéndonos enviar los embeddings generados, junto con cualquier metadato asociado, a nuestro índice de Vector Store objetivo para almacenamiento persistente y recuperación posterior.

Es en este nodo donde podemos establecer parámetros como "**top K**", que, como dijimos anteriormente, es el parámetro que determina el número máximo de chunks más similares que se recuperan del Vector Store en respuesta a una consulta.

<figure><img src="../.gitbook/assets/UD_04.png" alt="" width="375"><figcaption></figcaption></figure>

{% hint style="info" %}
**Consejo:** Un valor top K más bajo producirá menos resultados pero potencialmente más relevantes, mientras que un valor más alto devolverá un rango más amplio de resultados, potencialmente capturando más información.
{% endhint %}

## 5. Record Manager

El nodo [Record Manager](../integrations/langchain/record-managers.md) es una adición opcional pero increíblemente útil a nuestro flujo de upsert. Nos permite mantener registros de todos los chunks que se han hecho upsert a nuestro Vector Store, permitiéndonos agregar o eliminar chunks eficientemente según sea necesario.

Para una guía más detallada, te referimos a [esta guía](../integrations/langchain/record-managers.md).

<figure><img src="../.gitbook/assets/UD_05.png" alt="" width="375"><figcaption></figcaption></figure>

## 6. Visión General Completa

Finalmente, examinemos cada etapa, desde la carga inicial del documento hasta la representación vectorial final, destacando los componentes clave y sus roles en el proceso de upsert.

<figure><img src="../.gitbook/assets/UD_06.png" alt=""><figcaption></figcaption></figure>

1. **Ingesta de Documentos**:
   * Comenzamos alimentando nuestros datos crudos en Flowise usando el nodo **Document Loader** apropiado para tu formato de datos.
2. **División Estratégica**
   * Luego, el nodo **Text Splitter** divide nuestro documento en chunks más pequeños y manejables. Esto es crucial para la recuperación eficiente y el control de costos.
   * Tenemos flexibilidad en cómo ocurre esta división seleccionando el nodo text splitter apropiado y, importantemente, ajustando el tamaño de chunk y chunk overlap para balancear la preservación del contexto con la eficiencia.
3. **Embeddings Significativos**
   * Ahora, justo antes de que nuestros datos sean registrados en el Vector Store, el nodo **Embedding** entra en acción. Transforma cada chunk de texto y su significado en una representación numérica que nuestro LLM puede entender.
4. **Índice de Vector Store**
   * Finalmente, el nodo **Vector Store** actúa como el puente entre Flowise y nuestra base de datos. Envía nuestros embeddings, junto con cualquier metadato asociado, al índice de Vector Store designado.
   * Aquí, en este nodo, podemos controlar el comportamiento de recuperación estableciendo el parámetro **top K**, que influye en cuántos chunks se consideran al responder una consulta.
5. **Datos Listos**
   * Una vez hecho el upsert, nuestros datos ahora están representados como vectores dentro del Vector Store, listos para búsqueda de similitud y recuperación.
6. **Mantenimiento de Registros (Opcional)**
   * Para un control y gestión de datos mejorados, el nodo **Record Manager** mantiene un registro de todos los chunks con upsert. Esto facilita actualizaciones o eliminaciones fáciles a medida que tus datos o necesidades evolucionan.

En esencia, el proceso de upsert transforma nuestros datos crudos en un formato listo para LLM, optimizado para recuperación rápida y costo-efectiva.
