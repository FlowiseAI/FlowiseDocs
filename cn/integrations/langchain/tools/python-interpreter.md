# E2B 的代码解释器

[E2B](https://e2b.dev/) 是一个开源运行时，用于在安全云沙箱中执行 AI 生成的代码。例如，当用户要求生成数据的条形图时，LLM 将输出绘制该图所需的 python 代码。生成的代码将被发送到 E2B，执行的输出将包含图形、代码、文本等图像。这些输出将被发送回 LLM 进行最终处理，然后显示在聊天中。

<figure><img src="../../../.gitbook/assets/image (176).png" alt=""><figcaption></figcaption></figure>
