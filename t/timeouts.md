---
title: Timeouts
definition: A timeout is the longest your code will wait for a request to finish before it gives up.
description: How to choose connect and read timeouts for model calls, the defaults in common clients, and a curl example for luv13.
category: general
author: Quill
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- Without a timeout, a stuck request can hang your program forever.
- Model replies can take many seconds, so allow more time than for a normal web API.
- Longer replies take longer. Setting `max_tokens` helps keep the time predictable.
- With [Streaming](/docs/s/streaming), time to the first chunk and time between chunks matter more than total time.
- Pair timeouts with [Retrying Requests](/docs/r/retrying-requests).

## Picking a value

How long a reply takes depends on the model, the prompt length, and how many tokens it writes. [Reasoning Models](/docs/r/reasoning-models) can take much longer than fast models. As an example, 60 seconds is a reasonable start for short chat replies, and several minutes may be needed for long outputs. Measure your real requests and set the limit a bit above the slow ones.

## Types of timeout

- **Connect timeout:** how long to wait to open the connection. Keep this short, a few seconds.
- **Read timeout:** how long to wait for data once connected. For a non-streamed reply, that means waiting for the whole answer.
- **Total timeout:** a cap on the whole request.

## Defaults in common tools

- Python `requests` has no timeout unless you pass `timeout=`.
- The official OpenAI SDKs default to 10 minutes and accept a `timeout` option. See [Using the OpenAI SDKs](/docs/u/using-the-openai-sdks).
- Node's `fetch` has no timeout by default. Use `AbortSignal.timeout()`. See [Node.js Fetch](/docs/n/nodejs-fetch).

## Example

curl's `--max-time` sets a total limit in seconds:

```bash
curl --max-time 60 https://api.luv13.ai/v1/chat/completions \
  -H "Authorization: Bearer $LUV13_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "luv13/glm-5.3-flash",
    "messages": [{"role": "user", "content": "Write a haiku about rain."}],
    "max_tokens": 100
  }'
```
