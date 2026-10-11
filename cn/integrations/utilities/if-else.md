# 如果否则

Flowise 允许您根据 If/Else 条件将聊天流分成不同的分支。

<figure><img src="../../.gitbook/assets/image (5) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (2) (1).png" alt=""><figcaption></figcaption></figure>

### 输入变量

<figure><img src="../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

从上图中可以看出，它接收具有 `json` 输出的任何节点。一些示例包括：自定义函数、LLM 链输出预测、获取/设置变量。

<figure><img src="../../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (2) (1) (1).png" alt=""><figcaption></figcaption></figure>

然后您可以指定变量名称：

<figure><img src="../../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

然后，可以在 [If 函数](if-else.md#if-function) 和 [Else 函数](if-else.md#else-function) 中使用该变量（前缀为 `$`）。例如：

```
$output
```

### 如果是其他名称

您可以为节点命名，以便更轻松地直观地了解其功能。

### 如果函数

这是一段在 Node 沙箱上运行的 JS 代码。它必须：

* 包含 `if` 语句
* 返回 `if` 语句中的值

<figure><img src="../../.gitbook/assets/image (5) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (2) (1) (1).png" alt="" width="312"><figcaption></figcaption></figure>

这为用户提供了更大的灵活性来进行复杂的比较，如正则表达式、日期比较等等。

### 其他函数

与 If 函数类似，它必须返回一个值。仅当 [If Function](if-else.md#if-function) 不返回值时才会运行此函数。

<figure><img src="../../.gitbook/assets/image (6) (1) (1) (1) (1) (1) (1) (2) (1) (1).png" alt="" width="317"><figcaption></figcaption></figure>

### 输出

<figure><img src="../../.gitbook/assets/image (8) (1) (1) (1) (1) (1) (1) (2) (1) (1).png" alt=""><figcaption></figcaption></figure>

当 [If Function](if-else.md#if-function) 成功返回一个值时，它将被传递到 **True** 输出点，如上所示。这允许用户将值传递到下一个节点。

否则，[Else Function](if-else.md#else-function) 的返回值将传递到 **False** 输出点。

用户还可以查看市场中的 If Else 模板：

<figure><img src="../../.gitbook/assets/image (9) (1) (1) (1) (1) (2) (1) (1).png" alt=""><figcaption></figcaption></figure>
