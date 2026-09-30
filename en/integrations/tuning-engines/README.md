---
description: Learn how Flowise integrates with Tuning Engines
---

# Tuning Engines

Use [Tuning Engines](https://tuningengines.com/) with Flowise as an OpenAI-compatible inference endpoint for governed model routing.

Tuning Engines can sit between Flowise and your model providers so your team can use tenant-scoped inference keys, model aliases, policy controls, routing, fallback, usage tracking, and audit trails without changing Flowise nodes for every provider.

## How to use Tuning Engines with Flowise

### Step 1: Create an inference key

In Tuning Engines, create an inference key with access to the model aliases you want Flowise to use.

Keep the key in your Flowise credential store or environment variables. Do not paste provider credentials into Flowise when you are using a Tuning Engines inference key.

### Step 2: Configure an OpenAI-compatible chat model

In Flowise, use the standard OpenAI-compatible chat model node.

- Set `BasePath` to `https://api.tuningengines.com/v1`
- Set the API key to your Tuning Engines inference key
- Set the model name to a model alias enabled for that key, for example `gpt-4o-mini`

### Step 3: Test the flow

Run a simple chatflow message and confirm that the request appears in Tuning Engines usage and trace views.

If the model is denied, verify that the inference key's tenant, role, and model permissions allow the selected alias.
