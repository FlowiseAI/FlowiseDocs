---
description: Wrapper around EmpirioLabs chat models that use the OpenAI compatible Chat endpoint.
---

# ChatEmpirioLabs

## Description
[EmpirioLabs](https://empiriolabs.ai) is a unified inference platform that provides access to frontier chat models such as Qwen3.7, DeepSeek V4, GLM-5.1, Kimi K2.7 Code, and MiniMax M3 through a single OpenAI compatible API. The ChatEmpirioLabs node lists the available chat models for you and streams responses out of the box.

## Prerequisite
1. Refer to the official [EmpirioLabs documentation](https://docs.empiriolabs.ai).
2. Create an API key from your [EmpirioLabs dashboard](https://platform.empiriolabs.ai/dashboard/api-keys).

## Step by Step Guide
1. **Chat Models** > Drag the **ChatEmpirioLabs** node.
2. Create a new credential with your EmpirioLabs API key.
3. Pick a model from the **Model Name** list (for example `qwen3-7-plus`, `deepseek-v4-pro`, `glm-5-1`, or `kimi-k2-7-code`). The list is populated from the EmpirioLabs models catalog.
4. (Optional) Click **Additional Parameters** to adjust Max Tokens, Top Probability, penalties, Timeout, or to override the Base Path.

The node connects to the EmpirioLabs API at `https://api.empiriolabs.ai/v1`, so any model available on your account can be used the same way you would use any other OpenAI compatible Chat Model in Flowise.
