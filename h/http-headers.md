---
title: HTTP Headers
definition: HTTP headers are the name-value lines sent with each luv13 request and response; you need two on requests, Authorization and Content-Type.
description: The two headers every luv13 chat request needs, and the response headers you'll see, such as content-type and cf-ray.
category: luv13
author: Ink
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- Every chat completion request needs `Authorization: Bearer <your key>` and `Content-Type: application/json`.
- `GET /v1/models` answered on 2026-09-30 without any headers.
- JSON responses come back with `content-type: application/json`. 404 and 405 errors come back as HTML.
- luv13 is served through Cloudflare, so each response carries a `cf-ray` header that identifies that request.

## Request headers

| Header | Value | Needed for |
|---|---|---|
| `Authorization` | `Bearer sk-luv13-...` | `POST /v1/chat/completions` |
| `Content-Type` | `application/json` | Any request with a JSON body |

```bash
curl https://api.luv13.ai/v1/chat/completions \
  -H "Authorization: Bearer $LUV13_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"model": "luv13/glm-5.3-flash", "messages": [{"role": "user", "content": "ping"}]}'
```

The word `Bearer` and one space come before the key. A missing `Bearer`, extra quotes, or a stray newline at the end of the key gives a 401.

## Response headers you'll see

Seen on 2026-09-30:

| Header | Value | Use |
|---|---|---|
| `content-type` | `application/json` for API responses, `text/html` for 404 and 405 | Decide whether to parse the body as JSON |
| `server` | `cloudflare` | Shows the request went through Cloudflare |
| `cf-ray` | A Cloudflare request id | Identifies that request at Cloudflare |

No rate-limit headers (such as `x-ratelimit-remaining`) or `retry-after` were seen on those responses.

<!-- TODO: confirm with the operator whether luv13 sends rate-limit or retry-after headers under load, and whether support can trace a request by cf-ray. -->

To print response headers:

```bash
curl -s -D - -o /dev/null https://api.luv13.ai/v1/models
```

See also [Errors and Status Codes](/docs/e/errors-and-status-codes).
