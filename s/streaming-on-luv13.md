---
title: Streaming on luv13
definition: Streaming means receiving a chat completion reply in pieces as it's generated; on luv13 it would be requested in the OpenAI format, and per-model support isn't published yet.
category: luv13
author: Ink
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- In the OpenAI chat completions format, streaming is requested with `"stream": true` on `POST https://api.luv13.ai/v1/chat/completions`.
- luv13 hasn't published which of its seven models support streaming.
- The price is the same flat $0.33 per 1M tokens on every model, input the same as output.
- A missing or wrong key returns a normal 401 JSON error. See [Errors and Status Codes](/docs/e/errors-and-status-codes).

<!-- TODO: fill in from the operator's answer on streaming support per model, and verify a streamed response with a real key before adding an example. -->

## How streaming works in general

See [Streaming](/docs/s/streaming) and [Server-Sent Events](/docs/s/server-sent-events) for the OpenAI format and how to read a stream.

## Testing it on luv13

`-N` makes curl print output as it arrives:

```bash
curl -N https://api.luv13.ai/v1/chat/completions \
  -H "Authorization: Bearer $LUV13_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"model": "luv13/glm-5.3-flash", "messages": [{"role": "user", "content": "Count to 5."}], "stream": true}'
```

If the reply arrives as a series of `data:` lines, streaming worked for that model.
