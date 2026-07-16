---
description: 了解如何有效使用聊天流工具和自定义工具
---

# 调用子流程

***

Flowise 的强大功能之一是您可以将流程变成工具。例如，有一个主要流程来协调哪些/何时使用必要的工具。每个工具都被设计用来执行一项特定的任务。

这提供了一些好处：

* 每个子流程作为工具将单独执行，并具有单独的内存以允许更清晰的输出
* 将每个子流的详细输出聚合到最终代理，通常会产生更高质量的输出

您可以使用以下工具来实现此目的：

* 聊天流工具
* 自定义工具

## 聊天流工具

1. 准备好聊天流。在本例中，我们创建了一个可以经历多个链接的思想链聊天流。

<figure><img src="../.gitbook/assets/image (169).png" alt=""><figcaption></figcaption></figure>

2. 使用工具代理 + 聊天流工具创建另一个聊天流。选择您想要从工具中调用的聊天流。在本例中，它是思想链聊天流。给它一个名称和适当的描述，让 LLM 知道何时使用此工具：

<figure><img src="../.gitbook/assets/image (35).png" alt="" width="245"><figcaption></figcaption></figure>

3. 测试一下！

<figure><img src="../.gitbook/assets/image (168).png" alt=""><figcaption></figcaption></figure>

4. 从响应中，您可以看到 聊天流 工具 的输入和输出：

<figure><img src="../.gitbook/assets/image (170).png" alt=""><figcaption></figcaption></figure>

## 自定义工具

使用与上面相同的示例，我们将创建一个自定义工具，它将调用思想链聊天流的 [预测 API](broken-reference)。

1. 创建一个新工具：

<table><thead><tr><th width="180">工具名称</th><th>工具说明</th></tr></thead><tbody><tr><td>想法流</td><td>当您需要实现特定目标时使用此工具</td></tr></tbody></table>

输入架构：

<table><thead><tr><th>财产</th><th>类型</th><th>描述</th><th data-type="checkbox">必填</th></tr></thead><tbody><tr><td>输入</td><td>字符串</td><td>输入问题</td><td>真实</td></tr></tbody></table>

<figure><img src="../.gitbook/assets/image (95) (1).png" alt=""><figcaption></figcaption></figure>

Javascript工具功能：

```javascript
const fetch = require('node-fetch');
const url = 'http://localhost:3000/api/v1/prediction/<chatflow-id>'; // replace with specific chatflow id

const body = {
	"question": $input
};

const options = {
	method: 'POST',
	headers: {
		'Content-Type': 'application/json'
	},
	body: JSON.stringify(body)
};

try {
	const response = await fetch(url, options);
	const resp = await response.json();
	return resp.text;
} catch (error) {
	console.error(error);
	return '';
}
```

2. 创建工具代理+自定义工具。在自定义工具中指定我们在步骤 1 中创建的工具。

<figure><img src="../.gitbook/assets/image (97).png" alt=""><figcaption></figcaption></figure>

3. 从响应中，您可以看到自定义工具的输入和输出：

<figure><img src="../.gitbook/assets/image (99).png" alt=""><figcaption></figcaption></figure>

## 结论

在此示例中，我们成功演示了通过聊天流工具和自定义工具将其他聊天流转变为工具的两种方法。两者在底层都使用相同的代码逻辑。
