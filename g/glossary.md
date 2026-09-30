---
title: Glossary
definition: The luv13 glossary defines the terms, ids and fields you meet when using the luv13 API, each in one line.
description: One-line meanings for luv13 terms such as base URL, model id, flat rate, prepaid credit and invalid_auth, plus the model list fields.
category: luv13
author: Ink
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- Every entry is specific to luv13 and was checked on luv13.ai or the live API on 2026-09-30.
- For general concepts, see [What Is a Token](/docs/w/what-is-a-token) and [Context Window](/docs/c/context-window).
- For the models themselves, see [the model list](/docs/models).

## Terms

| Term | Meaning on luv13 |
|---|---|
| Base URL | `https://api.luv13.ai/v1`, the address every request path is added to |
| API key | Your secret, starting `sk-luv13-`, sent as `Authorization: Bearer <key>` |
| Model id | The exact string in `model`, such as `luv13/glm-5.3-flash`; always starts with `luv13/` |
| Flat rate | $0.33 per 1M tokens on every model, input the same as output |
| Prepaid credit | USD balance topped up by card from $5; usage draws it down |
| Dashboard | luv13.ai/dashboard, where you sign in, create keys, top up and see usage |
| `invalid_auth` | The `error.type` in a 401 response: missing or wrong key |
| Single-provider capacity | How luv13 runs; when it's saturated, requests queue or fail |

## Fields

| Field | Where | Meaning |
|---|---|---|
| `data[].id` | `GET /v1/models` | A model id |
| `data[].object` | `GET /v1/models` | `model` |
| `data[].owned_by` | `GET /v1/models` | `luv13` for every model |
| `data[].created` | `GET /v1/models` | `1700000000` for every model, so not a real add date |
| `error.code` | Error body | The HTTP status as a number, such as `401` |
| `error.message` | Error body | Short text, such as `unauthorized` |
| `error.type` | Error body | The kind of error, such as `invalid_auth` |

See one live:

```bash
curl -s https://api.luv13.ai/v1/models | jq '.data[0]'
```
