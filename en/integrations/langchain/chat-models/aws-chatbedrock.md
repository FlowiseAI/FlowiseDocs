---
description: Wrapper around AWS ChatBedrock large language models that use the Chat endpoint.
---

# AWS ChatBedrock


## Prerequisites

1. [Create or log in](https://signin.aws.amazon.com/signup?request_type=register) to your AWS account.

2. [Connect to Amazon Bedrock](https://docs.aws.amazon.com/appstudio/latest/userguide/connectors-bedrock.html) to enable model access.
   <ol type="a">
   <li> Choose the region.</li>
   <li> Enable the foundation models you intend to use in Flowise.
   </ol>

3. In the [IAM Console](https://console.aws.amazon.com/iam/), create an IAM User and API keys.
  <ol type="a">
  <li>On the new user, attach policies that grant necessary permission. Example: Attach a policy that allows <code>bedrock:InvokeModel</code> and <code>bedrock:InvokeModelWithResponseStream</code> on the models you intend to use.</li>

  <li>Generate an Access key and Secret access key. Store them for later use in Flowise. </li>
  </ol>

## Setup

1. [Install and run Flowise](https://docs.flowiseai.com/getting-started).

2. In **Chatflows**, create a new chatflow.

3. Add a node. Search and drag the **AWS ChatBedrock** node into the canvas.

4. Configure the AWS ChatBedrock node.
<figure><img src="../../../.gitbook/assets/image (10) (1) (1) (1) (1) (2).png" alt="" width="265"><figcaption><p>AWS ChatBedrock inputs</p></figcaption></figure>
<ol type="a">
  <li>Configure the AWS Credential.
  <ul>
  <li>Credential name</li>
  <li>AWS access key</li>
  <li>AWS secret access key</li>
  </ul>
  </li>
  <li>Your AWS region</li>

  <li>Model name that you enabled in Amazon Bedrock.

   Alternatively, enter a custom model name.</li>

  <li>If you intend to upload images from the chat interface, enable Allow Image Uploads.</li>

  <li>In Additional Parameters, adjust these parameters as needed:
  <ul>
  <li>Streaming</li>
  <li>Temperature</li>
  <li>Max Tokens to Sample</li>
  <li>Latency Optimized</li>
  </ul>
</li>
</ol>
5. Build a chatflow.
<ol type="a">
  <li>Add an input node.

     Use a Chat Input or similar node to capture the user prompt.

  <li>Connect to AWS ChatBedrock.

    Wire the Chat Input node to the AWS ChatBedrock node.

  <li>Add an output node.

    To display the chat response, add a Chat Output node and connect it to the AWS ChatBedrock node.
  <li>Save and open the chat interface to test.
</ol>

For information about deploying Flowise on AWS, see [AWS](../../../configuration/deployment/aws.md).