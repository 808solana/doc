---
title: What Is luv13
definition: luv13 is an OpenAI-compatible API that serves several AI models from one base URL and one API key.
category: luv13
author: Ink
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- luv13 speaks the same request and response format as the OpenAI API, so most OpenAI tools and SDKs work with it after two changes: the base URL and the API key.
- The base URL is `https://api.luv13.ai/v1`.
- One API key works for every model luv13 serves. The current list is always at `GET /v1/models`.
- Every model has the same flat price per token, and input costs the same as output. See [Pricing](/docs/pricing).

## How it works

You send a request to a luv13 endpoint, name the model you want in the `model` field, and get back a response in the OpenAI format. You don't need a separate account or key for each model.

The two endpoints most people start with are:

| Endpoint | What it does |
|---|---|
| `GET /v1/models` | Lists the models you can call. See [Listing Models](/docs/l/listing-models). |
| `POST /v1/chat/completions` | Sends a conversation to a model and returns its reply. See [Chat Completions](/docs/c/chat-completions). |

New to base URLs? See [Base URL](/docs/b/base-url) and [OpenAI-Compatible APIs](/docs/o/openai-compatible-apis).

## Pricing

<!-- TODO: verify against live luv13.ai/pricing before this page is checked. -->
Every model costs a flat $0.33 per 1M tokens, and input tokens cost the same as output tokens. The [Pricing](/docs/pricing) page is the source of truth.

## Getting started

1. Get an API key. See [Authentication](/docs/auth).
2. Point your client at `https://api.luv13.ai/v1`.
3. Pick a model id from `GET /v1/models` and send a request.

```bash
curl https://api.luv13.ai/v1/models \
  -H "Authorization: Bearer $LUV13_API_KEY"
```

For a full first request, see [Quickstart](/docs/quickstart).
