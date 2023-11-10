# Text Splitters
**When you want to deal with long pieces of text, it is necessary to split up that text into chunks.**<br>
As simple as this sounds, there is a lot of potential complexity here. Ideally, you want to keep the semantically related pieces of text together. What "semantically related" means could depend on the type of text. This notebook showcases several ways to do that.

**At a high level, text splitters work as following:**<br>
1. Split the text up into small, semantically meaningful chunks (often sentences).
1. Start combining these small chunks into a larger chunk until you reach a certain size (as measured by some function).
1. Once you reach that size, make that chunk its own piece of text and then start creating a new chunk of text with some overlap (to keep context between chunks).

**That means there are two different axes along which you can customize your text splitter:**<br>
1. How the text is split
1. How the chunk size is measured
