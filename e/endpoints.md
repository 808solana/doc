---
title: Endpoints
definition: luv13's endpoints are the two URL paths under https://api.luv13.ai/v1 that it serves, one to list models and one to create chat completions.
description: Which paths under api.luv13.ai/v1 answer, which return 404, and a short shell loop to check them yourself.
category: luv13
author: Ink
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- luv13 serves `GET /v1/models` and `POST /v1/chat/completions`.
- Both sit under the base URL `https://api.luv13.ai/v1`.
- Other OpenAI paths return HTTP 404, including `/v1/completions`, `/v1/embeddings`, `/v1/responses` and `/v1/models/{id}`.
- Paths must match exactly. `/v1/models/` with a trailing slash returns 404.

## Served

Checked live on 2026-09-30:

| Method and path | Key needed | What it does | Page |
|---|---|---|---|
| `GET /v1/models` | No (answered without one on 2026-09-30) | Lists the seven model ids | [Listing Models](/docs/l/listing-models) |
| `POST /v1/chat/completions` | Yes | Sends messages, returns the model's reply | [Chat Completions](/docs/c/chat-completions) |

`GET /v1/chat/completions` returns 405 (method not allowed).

## Not served

These returned HTTP 404 with an HTML body on 2026-09-30:

- `POST /v1/completions` (the older text completion API)
- `POST /v1/embeddings`
- `POST /v1/responses`
- `GET /v1/models/{id}`, for example `/v1/models/luv13/kimi-k3`
- Paths without `/v1`, such as `https://api.luv13.ai/chat/completions`

If a tool needs one of these, that feature of the tool won't work with luv13.

## Check it yourself

```bash
for p in models chat/completions embeddings; do
  printf "%-18s " "$p"
  curl -s -o /dev/null -w "%{http_code}\n" -X POST "https://api.luv13.ai/v1/$p" -d '{}'
done
```

Expected: `models` 405 (it's GET only), `chat/completions` 401 (served, needs a key), `embeddings` 404 (not served).

<!-- TODO: re-run this check when luv13 adds endpoints, and ask the operator whether any are planned. -->
