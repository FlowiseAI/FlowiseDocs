---
description: Custom Retriever allows user to specify the format of the context to LLM
---

# Custom Retriever

<figure><img src="../../../.gitbook/assets/image (3) (1).png" alt="" width="298"><figcaption></figcaption></figure>

By default, when context is being retrieved from vector store, they are in the following format:

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

**pageContent** of the array will be joined together as a string, and fed back to LLM for completion.

However, in some cases, you might want to include information from metadata to give more information to LLM, such as source, link, etc. This is where **Custom Retriever** comes in. We can specify the format to return to LLM.

For instance, using the following format:

```javascript
{{context}}
Source: {{metadata.source}}
```

Will results in the combined string as below:

```
This is an example
Source: example.pdf

This is example 2
Source: example2.txt
```

This will be sent back to LLM. Since LLM now has the sources of the answers, we can use prompts to instruct LLM to return answers followed by citations.
