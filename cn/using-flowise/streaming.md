---
description: 了解 Flowise 流式传输的工作原理
---

# 流式输出

如果在进行预测时设置了流式传输，则令牌将在可用时作为纯数据[服务器发送事件](https://developer.mozilla.org/en-US/docs/Web/API/Server-sent_events/Using_server-sent_events#Event_stream_format)发送。

### 使用 Python/TS 库

Flowise 提供 2 个库：

* [Python](https://pypi.org/project/flowise/)：`pip install flowise`
* [打字稿](https://www.npmjs.com/package/flowise-sdk)：`npm install flowise-sdk`

{% tabs %}
{% tab title="Python" %}
```python
from flowise import Flowise, PredictionData

def test_streaming():
    client = Flowise()

    # Test streaming prediction
    completion = client.create_prediction(
        PredictionData(
            chatflowId="<flow-id>",
            question="Tell me a joke!",
            streaming=True
        )
    )

    # Process and print each streamed chunk
    print("Streaming response:")
    for chunk in completion:
        # {event: "token", data: "hello"}
        print(chunk)


if __name__ == "__main__":
    test_streaming()
```
{% endtab %}

{% tab title="Typescript" %}
```javascript
import { FlowiseClient } from 'flowise-sdk'

async function test_streaming() {
  const client = new FlowiseClient({ baseUrl: 'http://localhost:3000' });

  try {
    // For streaming prediction
    const prediction = await client.createPrediction({
      chatflowId: '<flow-id>',
      question: 'What is the capital of France?',
      streaming: true,
    });

    for await (const chunk of prediction) {
        // {event: "token", data: "hello"}
        console.log(chunk);
    }
    
  } catch (error) {
    console.error('Error:', error);
  }
}

// Run streaming test
test_streaming()
```
{% endtab %}

{% tab title="cURL" %}
```bash
curl https://localhost:3000/api/v1/predictions/{flow-id} \
  -H "Content-Type: application/json" \
  -d '{
    "question": "Hello world!",
    "streaming": true
  }'
```
{% endtab %}
{% endtabs %}

```html
event: token
data: Once upon a time...
```

预测的事件流由以下事件类型组成：

|活动 |描述 |
| --------------- | ----------------------------------------------------------------------------------------------------------------------------------- |
|开始 |流式输出开始 |
|代币|当预测流式传输新令牌输出时发出 |
|错误 |当预测返回错误时发出 |
|结束 |预测完成时发出 |
|元数据 |相关流的所有元数据，例如 chatId、messageId。在所有令牌完成流式传输之后、结束事件之前发出 |
|来源文档 |当流从向量存储返回源时发出 |
|使用工具 |流量使用时发出的工具|

### Streamlit 应用程序

[https://github.com/HenryHengZJ/flowise-streamlit](https://github.com/HenryHengZJ/flowise-streamlit)
