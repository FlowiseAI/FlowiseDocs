# 设置/获取变量

如果您正在运行自定义函数或 LLM 链，您可能希望在其他节点中重用结果，而不必再次重新计算/重新运行相同的操作。您可以将输出结果保存为变量，并将其重新用于流程路径下游的其他节点。

<figure><img src="../../.gitbook/assets/savereuse.png" alt=""><figcaption></figcaption></figure>

### 设置变量

从输出 `string, number, boolean, json, array,` 的任何节点获取输入，我们可以为其分配一个变量名称。

<figure><img src="../../.gitbook/assets/image (11) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="270"><figcaption></figcaption></figure>

### 获取变量

稍后您可以从变量名称中获取变量值：

<figure><img src="../../.gitbook/assets/image (12) (1) (2).png" alt="" width="563"><figcaption></figcaption></figure>
