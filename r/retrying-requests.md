---
title: Retrying Requests
definition: Retrying requests means sending a failed luv13 call again after a growing wait, and only for errors that can succeed on a second try.
description: Which luv13 errors to retry, how to back off, and the retry settings built into the OpenAI SDKs and curl.
category: luv13
author: Ink
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- luv13 runs on single-provider capacity. When it's saturated, requests queue or fail, and luv13.ai/docs asks you to retry with backoff rather than hammering.
- Retry timeouts, 429 and 5xx errors (including Cloudflare's 522). Don't retry 401, 404 or 405; they'll fail the same way.
- Wait longer after each failure (for example 1, 2, 4, 8 seconds) and add a little random jitter.
- Failed calls aren't charged, so retrying a failure doesn't cost extra.
- If a model is unavailable, the error names it. Switching to another id can beat waiting. See [Model Fallback](/docs/m/model-fallback).

## What to retry

| Result | Retry? |
|---|---|
| Timeout or dropped connection | Yes |
| 429 | Yes, and honor `Retry-After` if it's sent |
| 500, 502, 503, 504, 522 | Yes |
| 401, 404, 405 | No, fix the request |

<!-- TODO: confirm with the operator the exact status codes luv13 returns for saturation and rate limits, and whether it sends Retry-After. -->

## With the OpenAI SDKs

Both official SDKs already retry 408, 409, 429 and 5xx responses with backoff (checked in `openai` Python 3.22.1 and Node.js 7.25.0). Python defaults to 2 retries. Raise it if you'd rather wait than fail:

```python
import os
from openai import OpenAI

client = OpenAI(
    base_url="https://api.luv13.ai/v1",
    api_key=os.environ["LUV13_API_KEY"],
    max_retries=5,
    timeout=120,
)
```

In Node.js, pass `maxRetries: 5` to `new OpenAI({...})`.

## With curl

```bash
curl --retry 5 --retry-delay 0 --retry-max-time 120 \
  https://api.luv13.ai/v1/chat/completions \
  -H "Authorization: Bearer $LUV13_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"model": "luv13/glm-5.3-flash", "messages": [{"role": "user", "content": "ping"}]}'
```

`--retry-delay 0` keeps curl's own doubling backoff. curl retries timeouts, 408, 429, 500, 502, 503, 504 and a few other codes, but not 522. For 522, use a loop or the SDKs.

<!-- TODO: confirm with the operator whether a request the client times out on is charged if the model finished, and whether luv13 supports an idempotency key. -->
