---
title: Rate Limiting
definition: Rate limiting is when an API caps how many requests or tokens you can use in a period of time, and rejects extra ones until the window resets.
description: Why APIs cap request rates, what a 429 response means, and how to pace and retry your luv13 requests.
category: general
author: Quill
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- Limits protect a shared service so one user can't slow it down for everyone.
- Limits may count requests per minute, tokens per minute, or requests running at once.
- Going over usually returns HTTP 429 Too Many Requests.
- The right response is to wait and retry with backoff, not to retry right away.
- For luv13's actual limits, see Limits (coming soon).

## How it usually works

A provider tracks your usage over a short window, such as a minute. When you go over, it rejects new requests with a 429 until enough time passes. Some APIs add headers that tell you how much is left or how long to wait, such as `Retry-After`. Which headers you get varies by provider.

## Staying under the limit

- **Spread out requests.** Use a queue instead of firing everything at once.
- **Limit concurrency.** Run a fixed number of requests in parallel.
- **Use fewer tokens.** Shorter prompts and a sensible `max_tokens` help if limits count tokens.
- **Back off on 429.** See [Retrying Requests](/r/retrying-requests).
- **Cache answers** for repeated identical requests when that fits your app.

## Handling a 429

1. Stop sending new requests for a moment.
2. If there's a `Retry-After` header, wait that long.
3. Otherwise wait with exponential backoff and jitter.
4. Retry a limited number of times, then report the error.

For the error format luv13 returns, see [Errors and Status Codes](/e/errors-and-status-codes).

## Example

This curl shows the status code and response headers, which is handy when you're checking whether a failure is a 429:

```bash
curl -i https://api.luv13.ai/v1/chat/completions \
  -H "Authorization: Bearer $LUV13_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"model": "luv13/glm-5.3-flash", "messages": [{"role": "user", "content": "Hi"}]}'
```
