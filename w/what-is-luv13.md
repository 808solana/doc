---
title: What Is luv13
definition: luv13 is an OpenAI-compatible API that serves seven open-weight models from one base URL and one API key, at one flat price per token.
description: "An overview of luv13: the base URL, its two endpoints, the seven model ids, the flat price and how to send a first request."
category: luv13
author: Ink
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- luv13 speaks the OpenAI request and response format, so most OpenAI tools and SDKs work with it after three changes: the base URL, the API key and the model id.
- The base URL is `https://api.luv13.ai/v1`.
- One API key works for all seven models. The current list is always at `GET /v1/models`.
- Every model costs a flat $0.33 per 1M tokens, and input costs the same as output. See [Pricing](/docs/p/pricing).
- Billing is prepaid: you top up any amount from $5 in the dashboard (confirmed by the luv13 operator team, 2026-10-02), and usage draws it down.

## How it works

You send a request to a luv13 endpoint, name the model you want in the `model` field, and get back a response in the OpenAI format. You don't need a separate account or key for each model.

The two endpoints luv13 serves are:

| Endpoint | What it does |
|---|---|
| `GET /v1/models` | Lists the models you can call. See [Listing Models](/docs/l/listing-models). |
| `POST /v1/chat/completions` | Sends a conversation to a model and returns its reply. See [Chat Completions](/docs/c/chat-completions). |

Other OpenAI endpoints, such as `/v1/embeddings`, aren't served. See [Endpoints](/docs/e/endpoints).

New to base URLs? See [Base URL](/docs/b/base-url) and [OpenAI-Compatible APIs](/docs/o/openai-compatible-apis).

## The models

As listed by live `GET /v1/models` on 2026-09-30:

| Model | Id |
|---|---|
| [DeepSeek V4-Pro](/docs/m/deepseek-v4-pro) | `luv13/deepseek-v4-pro` |
| [DeepSeek V4.1 Flash](/docs/m/deepseek-v4-1-flash) | `luv13/deepseek-v4.1-flash` |
| [GLM 5.3](/docs/m/glm-5-3) | `luv13/glm-5.3` |
| [GLM-5.3 Flash](/docs/m/glm-5-3-flash) | `luv13/glm-5.3-flash` |
| [Kimi K3](/docs/m/kimi-k3) | `luv13/kimi-k3` |
| [Kimi K3 Fast](/docs/m/kimi-k3-fast) | `luv13/kimi-k3-fast` |
| [Qwen 3.8 27B](/docs/m/qwen-3-8-27b) | `luv13/qwen-3.8-27b` |

Each name links to that model's page. The model list is also at [Models](/docs/models).

## Pricing

Every model costs a flat $0.33 per 1M tokens, and input tokens cost the same as output tokens (checked against live [models.luv13.ai](https://models.luv13.ai) on 2026-10-02). For example, 800,000 input tokens plus 200,000 output tokens is 1.0M tokens, which costs $0.33. The [Pricing](/docs/p/pricing) page is the source of truth.

## Getting started

1. Get an API key from the dashboard. See [Keys and Accounts](/docs/k/keys-and-accounts) and [Authentication](/docs/a/auth).
2. Point your client at `https://api.luv13.ai/v1`.
3. Pick a model id from `GET /v1/models` and send a request.

```bash
curl https://api.luv13.ai/v1/chat/completions \
  -H "Authorization: Bearer $LUV13_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"model": "luv13/glm-5.3-flash", "messages": [{"role": "user", "content": "ping"}]}'
```

For the request format, see [Chat Completions](/docs/c/chat-completions).
