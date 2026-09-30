---
title: FAQ
definition: The luv13 FAQ answers the questions people ask most about the API, each in a sentence or two, using only facts luv13 has published or that were checked live.
category: luv13
author: Ink
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- luv13 is an OpenAI-compatible API at `https://api.luv13.ai/v1` serving seven open-weight models.
- Every model costs $0.33 per 1M tokens, input the same as output, from prepaid credit.
- Keys start with `sk-luv13-` and come from the dashboard.
- Answers marked "not published yet" are waiting on the operator.

## Getting started

**What is the base URL?**
`https://api.luv13.ai/v1`.

**How do I get a key?**
Sign in to the [dashboard](https://luv13.ai/dashboard) with Google or email and create one. It's shown once. See [Authentication](/docs/a/auth).

**Which models can I use?**
Seven, listed by `GET /v1/models`: `luv13/deepseek-v4-pro`, `luv13/deepseek-v4.1-flash`, `luv13/glm-5.3`, `luv13/glm-5.3-flash`, `luv13/kimi-k3`, `luv13/kimi-k3-fast` and `luv13/qwen-3.8-27b` (as of 2026-09-30). See [the model list](https://luv13.ai/#models).

**Does my OpenAI code work?**
Chat completion code does, after changing the base URL, key and model id. Embeddings, the Responses API and other endpoints aren't served. See [Migrating from OpenAI](/docs/m/migrating-from-openai).

## Price and billing

**How much does it cost?**
$0.33 per 1M tokens on every model; input and output cost the same. 800k in + 200k out = 1.0M tokens = $0.33. See [Pricing](/docs/p/pricing).

**Is there a subscription?**
No. You top up prepaid credit by card (through Stripe) from $5, and usage draws it down.

**Am I charged for errors?**
No. luv13.ai/docs says failed or empty calls aren't charged.

**What happens when my balance runs out?**
Not published yet.
<!-- TODO: fill in from the operator's answer on out-of-balance behaviour. -->

## Using it

**Can models read images?**
Five of the seven accept images; all return text. See [Image Input](/docs/i/image-input).

**Can I call luv13 from a web page?**
Not directly; luv13 rejects cross-origin browser calls, and your key would be exposed. Use your own server. See [Browser Requests](/docs/b/browser-requests).

**Does it support streaming and tool calling?**
Both are requested in the OpenAI format, but per-model support isn't published yet. See [Streaming on luv13](/docs/s/streaming-on-luv13) and [Tool Calling on luv13](/docs/t/tool-calling-on-luv13).

**What are the rate limits?**
See [Limits](/docs/l/limits). luv13 runs on single-provider capacity, so under load requests can queue or fail; retry with backoff.

**Is there a status page?**
Not published yet. `GET /v1/models` works as a free check; see [Health Checks](/docs/h/health-checks).

## Data

**Are my prompts logged or used for training?**
Not published yet. The privacy notice is still being written. See [Data and Privacy](/docs/d/data-and-privacy).

**How do I contact luv13?**
Email hi@luv13.ai. See [Contact and Support](/docs/c/contact-and-support).

## Try it

```bash
curl https://api.luv13.ai/v1/chat/completions \
  -H "Authorization: Bearer $LUV13_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"model": "luv13/glm-5.3-flash", "messages": [{"role": "user", "content": "ping"}]}'
```
