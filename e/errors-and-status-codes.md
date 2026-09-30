---
title: Errors and Status Codes
definition: Errors and status codes are the HTTP codes and bodies luv13 returns when a request can't be served, and what each one means you should do.
category: luv13
author: Ink
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- A missing or wrong API key returns HTTP 401 with a JSON body whose `error.type` is `invalid_auth`.
- A wrong path returns HTTP 404 and a wrong method returns HTTP 405. Both bodies are HTML, not JSON, so don't assume every error parses as JSON.
- HTTP 522 comes from Cloudflare and means luv13's servers couldn't be reached. Wait and retry.
- When capacity is full, requests queue or fail. Retry with backoff; if one model is unavailable, the error names it and you can switch to another id.
- Failed or empty calls aren't charged (per luv13.ai/docs).

## What each code means

All of these were seen live on 2026-09-30 unless marked TODO.

| Code | Body | Cause | What to do |
|---|---|---|---|
| 401 | JSON, `invalid_auth` | No key, a wrong key, or the key not sent as `Authorization: Bearer` | Send `Authorization: Bearer $LUV13_API_KEY` with a valid key |
| 404 | HTML "Not Found" | Path doesn't exist: missing `/v1`, a trailing slash such as `/v1/models/`, or an endpoint luv13 doesn't serve | Check the URL against [Endpoints](/docs/e/endpoints) |
| 405 | HTML "Method Not Allowed" | Right path, wrong method, such as `GET /v1/chat/completions` | Use `POST` for chat completions |
| 522 | Cloudflare page | luv13's origin servers are unreachable | Retry later with backoff; see [Health Checks](/docs/h/health-checks) |

<!-- TODO: confirm with the operator the status code and body for: an unknown model id, a model that's unavailable or over capacity, an empty balance, a malformed JSON body with a valid key, and rate limiting. -->

## The 401 body

```json
{"error":{"code":401,"message":"unauthorized","type":"invalid_auth"}}
```

It has three fields inside `error`: `code` (the HTTP status as a number), `message` and `type`. The key is checked before the body is read, so without a valid key even a malformed body returns 401. A 401 therefore doesn't tell you whether the rest of your request is correct.

The documented way to send the key is the `Authorization: Bearer` header. Other headers, such as `x-api-key`, aren't documented by luv13, so don't rely on them.

## Handling errors in code

- Read the status code first, then try to parse JSON. A 404 or 405 body is HTML.
- Don't retry a 401, 404 or 405. The same request will fail the same way.
- Retry 522 and capacity errors with exponential backoff. See [Retrying Requests](/docs/r/retrying-requests).
- If an error names a model as unavailable, switch ids. See [Model Fallback](/docs/m/model-fallback).

The OpenAI SDKs turn the 401 into an `AuthenticationError` (Python) or an error with `status` 401 (Node.js), with the JSON above as the error body.

## Check it yourself

```bash
curl -s -w "\nHTTP %{http_code}\n" https://api.luv13.ai/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{"model": "luv13/glm-5.3-flash", "messages": [{"role": "user", "content": "ping"}]}'
```

With no key, this prints the 401 body above and `HTTP 401`.

For limits, see [Limits](/docs/l/limits).
