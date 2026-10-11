# 评价

{% hint style="info" %}
评估仅适用于云和企业计划
{% endhint %}

评估可帮助您监控和了解 聊天流/智能体流程 应用程序的性能。从高层次来看，评估是一个从 聊天流/智能体流程 获取一组输入和相应输出并生成分数的过程。这些分数可以通过将输出与参考结果进行比较来得出，例如通过字符串匹配、数字比较，甚至利用 LLM 作为判断。这些评估是使用数据集和评估器进行的。

## 数据集

数据集是用于运行 聊天流/智能体流程 的输入，以及用于比较的相应输出。用户可以手动添加输入和预期输出，或上传包含 2 列的 CSV 文件：输入和输出。

<figure><img src="../.gitbook/assets/image (3) (3).png" alt=""><figcaption></figcaption></figure>

|输入|输出|
| --------------------------------- | ---------------------------- |
|英国的首都是哪里 |英国首都是伦敦 |
|一年有多少天 |一年有365天 |

## 评估者

评估器就像单元测试。在评估期间，来自数据集的输入在选定的流上运行，并使用选定的评估器评估输出。评估者有 3 种类型：

* **基于文本**：基于字符串的检查：
  * 包含任何
  * 包含全部
  * 不包含任何
  * 不包含全部
  * 开头为
  * 不开头于

<figure><img src="../.gitbook/assets/image (6) (2).png" alt=""><figcaption></figcaption></figure>

* **基于数字：** 数字类型检查：
  * 代币总数
  * 提示词令牌
  * 完成标记
  * API 延迟
  * LLM 延迟
  * 聊天流延迟
  * 智能体流程 延迟（即将推出）
  * 输出字符长度

<figure><img src="../.gitbook/assets/image (7) (2).png" alt=""><figcaption></figcaption></figure>

* **LLM 基于**：使用另一个 LLM 对输出进行分级
  * 幻觉
  * 正确性

<figure><img src="../.gitbook/assets/image (9) (2).png" alt=""><figcaption></figcaption></figure>

## 评价

现在我们已经准备好了数据集和评估器，我们可以开始运行评估。

1.) 选择要评估的数据集和聊天流。您可以选择多个数据集和聊天流。使用下面的示例，来自 Dataset1 的每个输入都将针对 2 个聊天流运行。由于 Dataset1 有 2 个输入，因此将生成和评估总共 4 个输出。

<figure><img src="../.gitbook/assets/image (10) (2).png" alt=""><figcaption></figcaption></figure>

2.) 选择评估者。在此阶段只能选择基于字符串和基于数字的求值器。

<figure><img src="../.gitbook/assets/image (11) (2).png" alt=""><figcaption></figcaption></figure>

3.) （可选）选择基于 LLM 的评估器。开始评估：

<figure><img src="../.gitbook/assets/image (12) (2).png" alt=""><figcaption></figcaption></figure>

4.) 等待评估完成：

<figure><img src="../.gitbook/assets/image (13) (2).png" alt=""><figcaption></figcaption></figure>

5.) 评估完成后，点击右侧图表图标可查看详细信息：

<figure><img src="../.gitbook/assets/image (14) (2).png" alt=""><figcaption></figcaption></figure>

上面的三张图表显示了评估的摘要：

* 通过/失败率
* 使用的平均提示词和完成标记
* 请求的平均延迟

图表下方的表格显示了每次执行的详细信息。

<figure><img src="../.gitbook/assets/image (15) (2).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (16) (2).png" alt="" width="355"><figcaption></figcaption></figure>

### 重新运行评估

当评估中使用的流程被更新/修改时，将显示一条警告消息：

<figure><img src="../.gitbook/assets/image (17) (2).png" alt=""><figcaption></figcaption></figure>

您可以使用右上角的重新运行评估按钮重新运行相同的评估。您将能够看到不同的版本：

<figure><img src="../.gitbook/assets/image (18) (2).png" alt=""><figcaption></figcaption></figure>

您还可以查看并比较不同版本的结果：

<figure><img src="../.gitbook/assets/image (19) (2).png" alt=""><figcaption></figcaption></figure>

## 视频教程

{% embed url="https://youtu.be/kgUttHMkGFg?si=3rLplEp_0TI0p6UV&t=486" %}
