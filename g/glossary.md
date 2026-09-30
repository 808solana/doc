---
title: Glossary
definition: The luv13 glossary defines the terms, ids and response fields you meet when using the luv13 API, each in one line.
category: luv13
author: Ink
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- Every entry here is specific to how luv13 uses the term.
- Fields marked "OpenAI format" follow the OpenAI chat completions layout; luv13 hasn't confirmed every one with a live authenticated response.
- For general concepts such as tokens and context windows, see [What Is a Token](/docs/w/what-is-a-token) and [Context Window](/docs/c/context-window).

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
| Single-provider capacity | luv13's setup; under saturation, requests queue or fail |
| Modalities | What a model accepts and returns; every luv13 model returns text |

## Response fields

| Field | Where | Meaning |
|---|---|---|
| `data[].id` | `GET /v1/models` | A model id |
| `data[].owned_by` | `GET /v1/models` | `luv13` for every model |
| `data[].created` | `GET /v1/models` | `1700000000` for every model; not a real add date |
| `error.code` | Error body | The HTTP status as a number, such as `401` |
| `error.message` | Error body | Short text, such as `unauthorized` |
| `error.type` | Error body | Machine-readable kind, such as `invalid_auth` |
| `choices[0].message.content` | Chat completion (OpenAI format) | The reply text |
| `choices[0].finish_reason` | Chat completion (OpenAI format) | Why the reply stopped; see [Finish Reasons](/docs/f/finish-reasons) |
| `usage.prompt_tokens` | Chat completion (OpenAI format) | Input tokens |
| `usage.completion_tokens` | Chat completion (OpenAI format) | Output tokens |
| `usage.total_tokens` | Chat completion (OpenAI format) | The total the $0.33 per 1M rate applies to |

<!-- TODO: verify the chat completion fields against a real authenticated luv13 response. -->

See one live:

```bash
curl -s https://api.luv13.ai/v1/models | jq '.data[0]'
```
