# Extract Metadata Retriever

This retriever is designed to automatically extract keywords from query. The extracted JSON output is used as metadata filter for vector store.

For example, when we ask a question: "What is the profit for Apple", LLM will give an output of `{source: "apple"}`, and this will be passed to vectore store's metadata filter.

<figure><img src="../../../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>
