---
title: Authentication
definition: Authentication on luv13 means sending your sk-luv13- API key as a Bearer token in the Authorization header of each chat completion request.
description: "How luv13 checks your key: the Bearer header, the sk-luv13- format, the exact 401 body, and why browser calls should go through your server."
category: luv13
author: Ink
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- Send `Authorization: Bearer $LUV13_API_KEY` on every `POST /v1/chat/completions` request.
- Keys start with `sk-luv13-` and are shown once, when you create them in the dashboard.
- `GET /v1/models` currently answers without a key.
- A missing or invalid key on chat completions returns HTTP 401 with `"type": "invalid_auth"`.
- Keep the key on your server. Browser calls from other sites' origins are refused by CORS.

Checked on 2026-09-30 against luv13's original docs page and the live API.

## Sending the key

```bash
export LUV13_API_KEY=sk-luv13-...

curl https://api.luv13.ai/v1/chat/completions \
  -H "Authorization: Bearer $LUV13_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"model": "luv13/glm-5.3-flash", "messages": [{"role": "user", "content": "ping"}]}'
```

The header is the word `Bearer`, one space, then the key. In an SDK or tool, put the key in the API key setting and the base URL `https://api.luv13.ai/v1`; the SDK builds the header for you.

To get a key, see [Keys and Accounts](/docs/k/keys-and-accounts).

## Which endpoints need a key

| Endpoint | Key |
|---|---|
| `GET /v1/models` | Not currently required; it returned HTTP 200 without one |
| `POST /v1/chat/completions` | Required |

Sending the key to `/v1/models` as well does no harm.

## When the key is missing or wrong

Chat completions returns HTTP 401 with this body:

```json
{"error":{"code":401,"message":"unauthorized","type":"invalid_auth"}}
```

Check that the key is set in your environment, that the header starts with `Bearer `, and that no quotes or newline got copied with the key. See [Errors and Status Codes](/docs/e/errors-and-status-codes).

## Keep the key server-side

luv13 says to keep the key in an environment variable, never in client-side code. The API also refuses cross-origin browser calls: a CORS preflight from another site's origin returns HTTP 400. Call luv13 from your own server instead. See [Browser Requests](/docs/b/browser-requests) and [API Key Best Practices](/docs/a/api-key-best-practices).
