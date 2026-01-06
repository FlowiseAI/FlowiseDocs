---
description: Wrapper around AWS Bedrock large language models.
---

# AWS Bedrock


The **AWS Bedrock LLM** node integrates with Amazon Bedrock, a managed service that provides access to foundation models for building generative AI applications. 

To use AWS Bedrock in Flowise, add the **AWS Bedrock LLM** node to your Chatflow. 

# Setup

1. Configure AWS credentials for Flowise.

In Amazon Bedrock, ensure that your account has access to the model you want to use with Flowise. 

2. Add the AWS Bedrock node to your Chatflow.

In the Flowise canvas, drag and drop the AWS Bedrock LLM node into your Chatflow. 

3. Configure the AWS Bedrock inputs:
<figure><img src="../../../.gitbook/assets/image (2) (5).png" alt="" width="275"><figcaption><p>AWS Bedrock Node</p></figcaption></figure>

* AWS Credential: Select an existing AWS credential or create a new one. The associated IAM user or role must have permissions for `bedrock:InvokeModel` and any other required AWS services in your Chatflow.
* Region: The AWS region where your Bedrock models are available.
* Model Name: The AWS Bedrock foundational model for your conversational AI.

4. Connect the AWS Bedrock node in your Chatflow.

After you add other Chatflow components (such as input nodes, output nodes, memory nodes), connect the AWS Bedrock LLM node to the appropriate components to create the Chatflow.

For information about deploying Flowise on AWS, see [AWS](../../../configuration/deployment/aws.md).  


