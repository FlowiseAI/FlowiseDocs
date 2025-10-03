---
description: LangChain Text Splitter Nodes
---

# Text Splitters

***

**When you want to deal with long pieces of text, it is necessary to split up that text into chunks.**\
As simple as this sounds, there is a lot of potential complexity here. Ideally, you want to keep the semantically related pieces of text together. What "semantically related" means could depend on the type of text. This notebook showcases several ways to do that.

**At a high level, text splitters work as following:**

1. Split the text up into small, semantically meaningful chunks (often sentences).
2. Start combining these small chunks into a larger chunk until you reach a certain size (as measured by some function).
3. Once you reach that size, make that chunk its own piece of text and then start creating a new chunk of text with some overlap (to keep context between chunks).

**That means there are two different axes along which you can customize your text splitter:**

1. How the text is split
2. How the chunk size is measured

### Text Splitter Nodes:

* [Character Text Splitter](character-text-splitter.md)
* [Code Text Splitter](code-text-splitter.md)
* [Html-To-Markdown Text Splitter](html-to-markdown-text-splitter.md)
* [Markdown Text Splitter](markdown-text-splitter.md)
* [Recursive Character Text Splitter](recursive-character-text-splitter.md)
* [Token Text Splitter](token-text-splitter.md)
