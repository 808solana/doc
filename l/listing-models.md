---
title: Listing Models
definition: Listing models means calling luv13's GET /v1/models endpoint to see every model id you can use right now.
description: The live GET /v1/models response, what each field holds, and a jq one-liner that prints just the seven ids.
category: luv13
author: Ink
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- The endpoint is `GET https://api.luv13.ai/v1/models`.
- It returns the seven models luv13 serves, in the OpenAI list format.
- The `id` of each model is the exact value to put in the `model` field of a request.
- On 2026-09-30 it answered without an API key, so it also works as a quick connectivity check. See [Health Checks](/docs/h/health-checks).
- Always copy ids from this list. A small typo makes a request fail.

## Example

```bash
curl https://api.luv13.ai/v1/models \
  -H "Authorization: Bearer $LUV13_API_KEY"
```

Sending your key is harmless and keeps the call working if luv13 starts requiring it. On 2026-09-30 the same call without the header also returned HTTP 200.

## The response

The live response on 2026-09-30, trimmed to two of the seven entries:

```json
{
  "data": [
    {"created": 1700000000, "id": "luv13/deepseek-v4-pro", "object": "model", "owned_by": "luv13"},
    {"created": 1700000000, "id": "luv13/deepseek-v4.1-flash", "object": "model", "owned_by": "luv13"}
  ],
  "object": "list"
}
```

| Field | What it holds on luv13 |
|---|---|
| `object` (top level) | `list` |
| `data` | One entry per model |
| `id` | The model id, such as `luv13/kimi-k3`. Use it as `model` in requests. |
| `object` (per entry) | `model` |
| `created` | `1700000000` for every model. It's the same fixed value for all seven, so don't read it as the date a model was added. |
| `owned_by` | `luv13` for every model |

The full list on 2026-09-30 was `luv13/deepseek-v4-pro`, `luv13/deepseek-v4.1-flash`, `luv13/glm-5.3`, `luv13/glm-5.3-flash`, `luv13/kimi-k3`, `luv13/kimi-k3-fast` and `luv13/qwen-3.8-27b`.

Each model has its own page: [DeepSeek V4-Pro](/docs/m/deepseek-v4-pro), [DeepSeek V4.1 Flash](/docs/m/deepseek-v4-1-flash), [GLM 5.3](/docs/m/glm-5-3), [GLM-5.3 Flash](/docs/m/glm-5-3-flash), [Kimi K3](/docs/m/kimi-k3), [Kimi K3 Fast](/docs/m/kimi-k3-fast) and [Qwen 3.8 27B](/docs/m/qwen-3-8-27b).

The response has no context length, modality or price fields. Those are on the model list at [luv13.ai/#models](https://luv13.ai/#models) and on [Pricing](/docs/p/pricing).

To print only the ids:

```bash
curl -s https://api.luv13.ai/v1/models | jq -r '.data[].id'
```

## Why it matters

Most OpenAI-compatible tools call this endpoint to fill their model picker. If a tool shows no models, check the base URL first; it must be exactly `https://api.luv13.ai/v1`. See [Base URL](/docs/b/base-url).

There's no single-model endpoint: `GET /v1/models/luv13/kimi-k3` returns HTTP 404. Filter the list instead.
