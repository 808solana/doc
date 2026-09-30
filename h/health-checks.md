---
title: Health Checks
definition: A luv13 health check is a quick request, usually GET /v1/models, that tells you whether the API is reachable before you debug your own code.
description: A free GET /v1/models check to see if luv13 is up, what 200, 522 and 000 mean, and a second check for your key.
category: luv13
author: Ink
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- `GET https://api.luv13.ai/v1/models` is the simplest check. On 2026-09-30 it answered HTTP 200 without a key and costs no tokens.
- HTTP 200 with seven model ids means the API is reachable.
- HTTP 522 means Cloudflare couldn't reach luv13's servers. It happened on the morning of 2026-09-30 and cleared by 10:36 PT. It's not a problem with your code or key.
- A models check doesn't prove chat works. To test your key and a model end to end, send a one-token chat request.

## Level 1: is the API up?

```bash
curl -s -m 20 -o /dev/null -w "%{http_code}\n" https://api.luv13.ai/v1/models
```

| Output | Meaning |
|---|---|
| `200` | Reachable |
| `522` | luv13's servers are unreachable behind Cloudflare. Wait and retry. |
| `000` | No response within 20 seconds, or a network or DNS problem on your side |

<!-- TODO: ask the operator whether luv13 has, or plans, a public status page to link here. -->

## Level 2: do my key and a model work?

```bash
curl -s -m 60 -w "\nHTTP %{http_code}\n" https://api.luv13.ai/v1/chat/completions \
  -H "Authorization: Bearer $LUV13_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"model": "luv13/glm-5.3-flash", "messages": [{"role": "user", "content": "ping"}]}'
```

When the key works, this is billed like any request, a tiny fraction of a cent at $0.33 per 1M tokens. A `401` means the key is wrong; anything else, see [Errors and Status Codes](/docs/e/errors-and-status-codes).

## Monitoring

- Use Level 1 for frequent automated checks. It's free.
- Run Level 2 rarely, because it draws on your balance.
- Alert on repeated failures, not one. luv13 runs on single-provider capacity, and brief failures under load are expected; see [Retrying Requests](/docs/r/retrying-requests).
- Check that the response still lists the model id your app uses. If it's gone, switch ids. See [Listing Models](/docs/l/listing-models).
