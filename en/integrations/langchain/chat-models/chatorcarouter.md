# ChatOrcaRouter

## Description

OrcaRouter is an OpenAI-compatible LLM router that exposes 150+ models behind a single endpoint, with a workspace-configurable `auto` router that picks the best upstream per request based on price, latency, quality, and rate-limit signals.

## Prerequisites

1. Sign up at [https://www.orcarouter.ai](https://www.orcarouter.ai).
2. Create an API key at the [OrcaRouter console](https://www.orcarouter.ai/console). Keys begin with `sk-orca-`.
3. Browse the full model catalog at [https://www.orcarouter.ai/models](https://www.orcarouter.ai/models).

## Step by Step Guide

1. **Chat Models** > Drag the **ChatOrcaRouter** node.
2. Click **Connect Credential** and create a new credential with your OrcaRouter API key.
3. Pick a model from the **Model Name** dropdown, which is fetched live from the OrcaRouter catalog with `orcarouter/auto` as the default option (the workspace router). You can also type any model id from the [OrcaRouter catalog](https://www.orcarouter.ai/models).
4. Connect ChatOrcaRouter to an LLM Chain, Conversational Agent, or other downstream node and run the chatflow.

## Routing notes

- `orcarouter/auto` is a virtual router name, not a model. Configure its routing strategy (`cheapest`, `balanced`, `quality`, `adaptive`, `gated_adaptive`) and pool membership at [https://www.orcarouter.ai/console/routing](https://www.orcarouter.ai/console/routing).
- Reasoning models such as `anthropic/claude-opus-4.8`, the OpenAI `gpt-5` family, and `deepseek/deepseek-reasoner` reject the `temperature` field. Leave **Temperature** blank for those models.
- For self-hosted [OrcaRouter-O2](https://github.com/Continuum-AI-Corp/OrcaRouter-O2) deployments, override the **Base Path** under *Additional Parameters*.
