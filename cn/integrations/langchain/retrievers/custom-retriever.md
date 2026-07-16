---
description: 自定义检索器允许用户将上下文的格式指定为 LLM
---

# 自定义检索器

<figure><img src="../../../.gitbook/assets/image (3) (1) (1) (2) (1).png" alt="" width="298"><figcaption></figcaption></figure>

默认情况下，当从向量存储中检索上下文时，它们采用以下格式：

```json
[ 
    {
        "pageContent": "This is an example",
        "metadata": {
            "source": "example.pdf"
        }
    },
    {
        "pageContent": "This is example 2",
        "metadata": {
            "source": "example2.txt"
        }
    }
]
```

数组的 **pageContent** 将作为字符串连接在一起，并反馈到 LLM 完成。

但是，在某些情况下，您可能希望包含元数据中的信息，以便为 LLM 提供更多信息，例如源、链接等。这就是 **自定义检索器** 的用武之地。我们可以指定返回到 LLM 的格式。

例如，使用以下格式：

```javascript
{{context}}
Source: {{metadata.source}}
```

将产生如下组合字符串：

```
This is an example
Source: example.pdf

This is example 2
Source: example2.txt
```

这将被发送回 LLM。由于 LLM 现在有了答案的来源，我们可以使用提示词来指示 LLM 返回答案，然后引用。
