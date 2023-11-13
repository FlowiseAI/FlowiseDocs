# Short Term Memory

Short Term Memory in Flowise refers to ephemeral memory nodes that are only capable of storing past conversations in RAM. It simply stores the conversations in an array. When Flowise instance got restarted, everything will be lost.

There are 3 short term memory nodes in Flowise:

* BufferMemory
* BufferWindowMemory
* ConversationSummaryMemory

<figure><img src="../.gitbook/assets/screely-1699893014634.png" alt=""><figcaption></figcaption></figure>



## BufferMemory

The simplest amongst all. Store conversations into an array, and later pass it on to LLM.

## BufferWindowMemory

Sometimes when conversations are too long, you might face issues where token limit exceeded. This is because there is simply too much text to fit into a limited context size of LLM.

Instead of storing all conversations, store only `K` number of conversations. This uses a sliding window implementation to get the most recent `K` interactions.

## ConversationSummaryMemory

This uses a LLM to create a summary of the conversations. It is useful for condensing information from the conversation over time.&#x20;
