---
title: Quickstart
definition: The luv13 quickstart gets you from no account to a first working request in three steps.
description: Get a luv13 key, set the base URL, and send one curl request. Nothing else.
category: luv13
author: Ink
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- Get a key from the dashboard. It starts with `sk-luv13-`.
- The base URL is `https://api.luv13.ai/v1`.
- One curl request with `luv13/glm-5.3-flash` confirms everything works.

## 1. Get a key

Sign in at [luv13.ai/dashboard](https://luv13.ai/dashboard) and create a key. See [Keys and Accounts](/docs/k/keys-and-accounts).

```bash
export LUV13_API_KEY=sk-luv13-...
```

## 2. Base URL

```
https://api.luv13.ai/v1
```

## 3. Send a request

```bash
curl https://api.luv13.ai/v1/chat/completions \
  -H "Authorization: Bearer $LUV13_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"model": "luv13/glm-5.3-flash", "messages": [{"role": "user", "content": "ping"}]}'
```

Done. For code, see the [Python Example](/docs/p/python-example) or [JavaScript Example](/docs/j/javascript-example).
