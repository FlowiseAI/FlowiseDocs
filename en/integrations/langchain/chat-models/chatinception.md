---
description: Wrapper around Inception Mercury diffusion LLMs (OpenAI-compatible API).
---

# ChatInception

## Description
[Inception](https://inceptionlabs.ai/) provides the Mercury family of diffusion LLMs, including **Mercury 2** — a fast reasoning model that supports tool calling and structured outputs. The API is OpenAI-compatible and served from `https://api.inceptionlabs.ai/v1`.

## Prerequisite
1. Create an account on the [Inception Platform](https://platform.inceptionlabs.ai/).
2. Get your API key from the [Inception Platform dashboard](https://platform.inceptionlabs.ai/). New accounts include free tokens to get started.

## Step by Step Guide
1. **Chat Models** > Drag the **ChatInception** node.
2. Create a new credential with your Inception API key.
3. Select a **Model Name** (defaults to `mercury-2`).
4. (Optional) Click **Additional Parameters** to configure **Reasoning Effort** (`instant`, `low`, `medium`, `high`), **Max Tokens**, **Streaming**, **Timeout**, or override the **Base Path**.

## Reasoning Effort
Mercury 2 is a reasoning model. Use the **Reasoning Effort** parameter to trade latency for depth:

| Value | Behavior |
| --- | --- |
| `instant` | Lowest latency, no extended thinking |
| `low` | Minimal reasoning |
| `medium` | Balanced reasoning (default) |
| `high` | Maximum reasoning for complex tasks |
