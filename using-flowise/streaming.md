---
description: Learn how Flowise streaming works
---

# Streaming

If streaming is set when making prediction, tokens will be sent as data-only [server-sent events](https://developer.mozilla.org/en-US/docs/Web/API/Server-sent\_events/Using\_server-sent\_events#Event\_stream\_format) as they become available.

### Using Python/TS Library

Flowise provides 2 libraries:

* [Python](https://pypi.org/project/flowise/): `pip install flowise`
* [Typescript](https://www.npmjs.com/package/flowise-sdk): `npm install flowise-sdk`

{% tabs %}
{% tab title="Python" %}
```python
from flowise import Flowise, PredictionData

def test_non_streaming():
    client = Flowise()

    # Test non-streaming prediction
    completion = client.create_prediction(
        PredictionData(
            chatflowId="<chatflow-id>",
            question="What is the capital of France?",
            streaming=False
        )
    )

    # Process and print the response
    for response in completion:
        print("Non-streaming response:", response)

def test_streaming():
    client = Flowise()

    # Test streaming prediction
    completion = client.create_prediction(
        PredictionData(
            chatflowId="<chatflow-id>",
            question="Tell me a joke!",
            streaming=True
        )
    )

    # Process and print each streamed chunk
    print("Streaming response:")
    for chunk in completion:
        print(chunk)


if __name__ == "__main__":
    # Run non-streaming test
    test_non_streaming()

    # Run streaming test
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
      chatflowId: '<chatflow-id>',
      question: 'What is the capital of France?',
      streaming: true,
    });

    for await (const chunk of prediction) {
        console.log(chunk);
    }
    
  } catch (error) {
    console.error('Error:', error);
  }
}

async function test_non_streaming() {
    const client = new FlowiseClient({ baseUrl: 'http://localhost:3000' });
  
    try {
      // For streaming prediction
      const prediction = await client.createPrediction({
        chatflowId: '<chatflow-id>',
        question: 'Tell me a joke!',
      });
  
      console.log(prediction);
      
    } catch (error) {
      console.error('Error:', error);
    }
}

// Run non-streaming test
test_non_streaming()

// Run streaming test
test_streaming()
```
{% endtab %}

{% tab title="cURL" %}
```bash
curl https://localhost:3000/api/v1/predictions/{chatflow-id} \
  -H "Content-Type: application/json" \
  -d '{
    "question": "Hello world!",
    "streaming": true
  }'
```
{% endtab %}
{% endtabs %}
