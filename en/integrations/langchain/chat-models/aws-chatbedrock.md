---
description: Wrapper around AWS ChatBedrock large language models that use the Chat endpoint.
---

# AWS ChatBedrock


## Prerequisites

1. [Create or log in](https://signin.aws.amazon.com/signup?request_type=register) to your AWS account.

2. [Connect to Amazon Bedrock](https://docs.aws.amazon.com/appstudio/latest/userguide/connectors-bedrock.html) to enable model access.

  a. Choose the region.
  b. Enable the foundation models you intend to use in Flowise.

3. In the [IAM Console](https://console.aws.amazon.com/iam/), create an IAM User and API keys.

  a. On the new user, attach policies that grant necessary permission. Example: Attach a policy that allows `bedrock:InvokeModel` and `bedrock:InvokeModelWithResponseStream` on the models you intend to use.

  b.Generate an Access key and Secret access key. Store them for later use in Flowise. 


## Setup

1. [Install and run Flowise](https://docs.flowiseai.com/getting-started).

2. In **Chatflows**, create a new chatflow.

3. Add a node.

  Search and drag the AWS ChatBedrock node into the canvas.

4. Configure the AWS ChatBedrock node.
<figure><img src="../../../.gitbook/assets/image (10) (1) (1) (1) (1) (2).png" alt="" width="265"><figcaption><p>AWS ChatBedrock</p></figcaption></figure>

  a. Configure the AWS Credential.
  * Credential name
  * AWS access key
  * AWS secret access key
  
  b. Your AWS region

  c. Model name that you enabled in Amazon Bedrock.

   Alternatively, enter a custom model name.

  d. If you intend to upload images from the chat interface, enable Allow Image Uploads.

  e. In Additional Parameters, adjust these parameters as needed:
  * Streaming
  * Temperature
  * Max Tokens to Sample
  * Latency Optimized

5. Build a chatflow.

  a. Add an input node.

     Use a Chat Input or similar node to capture the user prompt.

  b. Connect to AWS ChatBedrock.

    Wire the Chat Input node to the AWS ChatBedrock node.

  c. Add an output node.

    To display the chat response, add a Chat Output node and connect it to the AWS ChatBedrock node.

  d. Save and open the chat interface to test.


For information about deploying Flowise on AWS, see [AWS](../../../configuration/deployment/aws.md).