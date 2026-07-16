---
description: LangChain文本分割器节点
---

# 文本分割器

***

**当您想要处理长文本时，有必要将该文本分割成块。**\
这听起来很简单，但这里存在很多潜在的复杂性。理想情况下，您希望将语义相关的文本片段保留在一起。 “语义相关”的含义可能取决于文本的类型。本笔记本展示了实现此目的的几种方法。

**在较高层面上，文本分割器的工作原理如下：**

1. 将文本分成小的、具有语义意义的块（通常是句子）。
2. 开始将这些小块组合成一个更大的块，直到达到一定的大小（通过某些函数测量）。
3. 达到该大小后，将该块设为自己的文本片段，然后开始创建具有一些重叠的新文本块（以保留块之间的上下文）。

**这意味着您可以沿着两个不同的轴自定义文本分割器：**

1. 文本如何分割
2. chunk 大小如何测量

### 文本分割器节点：

* [字符文本分割器](character-text-splitter.md)
* [代码文本分割器](code-text-splitter.md)
* [Html-To-Markdown 文本分割器](html-to-markdown-text-splitter.md)
* [Markdown 文本分割器](markdown-text-splitter.md)
* [递归字符文本分割器](recursive-character-text-splitter.md)
* [令牌文本分割器](token-text-splitter.md)
