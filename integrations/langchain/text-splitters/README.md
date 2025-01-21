---
description: Nodos Text Splitter de LangChain
---

# Text Splitters

***

**Cuando quieres trabajar con textos largos, es necesario dividir ese texto en fragmentos.**\
Aunque esto suene simple, hay mucha complejidad potencial aquí. Idealmente, quieres mantener juntas las partes de texto semánticamente relacionadas. Lo que significa "semánticamente relacionado" puede depender del tipo de texto. Esta sección muestra varias formas de hacerlo.

**A alto nivel, los text splitters funcionan de la siguiente manera:**

1. Dividen el texto en fragmentos pequeños y semánticamente significativos (a menudo oraciones).
2. Comienzan a combinar estos pequeños fragmentos en un fragmento más grande hasta alcanzar cierto tamaño (medido por alguna función).
3. Una vez que alcanzas ese tamaño, ese fragmento se convierte en su propia pieza de texto y luego se comienza a crear un nuevo fragmento de texto con cierto solapamiento (para mantener el contexto entre fragmentos).

**Esto significa que hay dos ejes diferentes en los que puedes personalizar tu text splitter:**

1. Cómo se divide el texto
2. Cómo se mide el tamaño del fragmento

### Nodos Text Splitter:

* [Character Text Splitter](character-text-splitter.md)
* [Code Text Splitter](code-text-splitter.md)
* [Html-To-Markdown Text Splitter](html-to-markdown-text-splitter.md)
* [Markdown Text Splitter](markdown-text-splitter.md)
* [Recursive Character Text Splitter](recursive-character-text-splitter.md)
* [Token Text Splitter](token-text-splitter.md)
