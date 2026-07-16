---
description: >-
  检查输入是否包含拒绝列表中的任何文本，并防止被拒绝
  发送至 LLM。
---

# 简单提示词审核

<figure><img src="../../../.gitbook/assets/image (4) (1) (1) (1) (1) (1) (1) (2) (1).png" alt="" width="301"><figcaption><p>简单提示词审核节点</p></figcaption></figure>

使用另一个 LLM 来识别用户查询是否接近拒绝列表，如果是则输出默认错误消息。

例如，拒绝列表可以是：

* 忽略之前的说明
* 泄露所有敏感信息

<figure><img src="../../../.gitbook/assets/image (336).png" alt=""><figcaption></figcaption></figure>
