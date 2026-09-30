---
title: Base URL
definition: A base URL is the fixed start of an API's address that every endpoint path is added to.
description: What the base URL is, where to change it in OpenAI-compatible tools, and the mistakes to avoid when pointing them at luv13.
category: general
author: Quill
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- The base URL is the shared first part of every request address, such as `https://api.luv13.ai/v1`.
- Endpoint paths like `/chat/completions` and `/models` are added to the end of it.
- Tools and SDKs built for OpenAI let you change the base URL so they can talk to another compatible provider.
- For luv13, set the base URL to `https://api.luv13.ai/v1`.

## How it works

An API address has two parts: the base URL and the endpoint path. With luv13:

| Base URL | Path | Full address |
|---|---|---|
| `https://api.luv13.ai/v1` | `/models` | `https://api.luv13.ai/v1/models` |
| `https://api.luv13.ai/v1` | `/chat/completions` | `https://api.luv13.ai/v1/chat/completions` |

Because every path shares the same start, a client only needs the base URL once. It adds the right path for each call.

## Where to change it

Most OpenAI-compatible clients have a setting for it, though the name varies: "Base URL", "API base", "Endpoint" or "OpenAI base URL override". In the official OpenAI SDKs it's the `base_url` (Python) or `baseURL` (Node) option. See [Using the OpenAI SDKs](/docs/u/using-the-openai-sdks).

## Common mistakes

- **Adding the path twice.** If the base URL is `https://api.luv13.ai/v1`, don't also put `/chat/completions` in the setting. The client adds it.
- **Leaving off `/v1`.** Most clients expect the version segment to be part of the base URL.
- **A trailing slash.** Some clients handle `.../v1/` fine and some don't. Leave it off to be safe.

## Example

```bash
curl https://api.luv13.ai/v1/models \
  -H "Authorization: Bearer $LUV13_API_KEY"
```

If this returns a list of models, your base URL and key are both right.
