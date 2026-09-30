---
title: Streaming
definition: Streaming means the API sends a model's reply in small pieces as it is written, instead of all at once at the end.
category: general
author: Quill
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- Without streaming, you wait for the whole reply and get it in one response.
- With streaming, text starts arriving after the first few tokens are ready, so the reply feels faster.
- OpenAI-compatible APIs turn streaming on with `"stream": true` in the request body.
- The pieces arrive as Server-Sent Events, each holding a small "delta" of new text.
- For how luv13 handles it, see [Streaming on luv13](/docs/s/streaming-on-luv13).

## How it works

A model writes its reply one token at a time. A normal request holds all of those tokens on the server and sends them together when the reply is done. A streaming request sends each new bit of text as soon as it exists.

The total time to finish is about the same either way. What changes is how soon you see something. For a chat window or a coding tool, that early first word makes a big difference to how the app feels.

## What you get back

In the OpenAI format, a streamed reply is a series of chunks. Each chunk has a `choices[0].delta` object with the new piece of text in `delta.content`. You join the pieces in order to rebuild the full reply. The last chunk carries a `finish_reason`, such as `stop` or `length`.

The wire format for these chunks is covered in [Server-Sent Events](/docs/s/server-sent-events).

## When to use it

- **Use it** for chat interfaces, editors and anything a person watches in real time.
- **Skip it** for background jobs where you only need the final text. A single response is simpler to handle.

## Example

The `-N` flag tells curl not to buffer, so you see chunks as they arrive. Check [Streaming on luv13](/docs/s/streaming-on-luv13) for the exact behavior luv13 supports.

```bash
curl -N https://api.luv13.ai/v1/chat/completions \
  -H "Authorization: Bearer $LUV13_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "luv13/glm-5.3-flash",
    "messages": [{"role": "user", "content": "Count from 1 to 5."}],
    "stream": true
  }'
```
