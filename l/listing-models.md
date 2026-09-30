---
title: Listing Models
definition: Listing models means calling luv13's GET /v1/models endpoint to see every model id you can use right now.
category: luv13
author: Ink
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- The endpoint is `GET https://api.luv13.ai/v1/models`.
- It returns the models luv13 serves right now, in the OpenAI list format.
- The `id` of each model is the exact value to put in the `model` field of a request.
- Always copy ids from this list. Don't guess or retype them from memory, because a small typo makes a request fail.

## Example

```bash
curl https://api.luv13.ai/v1/models \
  -H "Authorization: Bearer $LUV13_API_KEY"
```

## The response

In the OpenAI format, the response is an object with `"object": "list"` and a `data` array. Each item in `data` describes one model:

| Field | What it is |
|---|---|
| `id` | The model id. Use it as `model` in requests. |
| `object` | Always `model`. |
| `created` | When the model was added, as a Unix timestamp. |
| `owned_by` | Who publishes the model. |

<!-- TODO: run the curl above against live /v1/models and replace this table with the fields luv13 actually returns, plus a trimmed real example. The API returned HTTP 522 on 2026-09-30. -->

## Why it matters

Most OpenAI-compatible tools call this endpoint to fill their model picker. If a tool shows no models, check the base URL and API key first. See [Base URL](/docs/b/base-url).

For each model's details, see [Models](/docs/models). Every model has the same price; see [Pricing](/docs/pricing).
