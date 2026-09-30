---
title: Server-Sent Events
definition: Server-Sent Events (SSE) is a simple web standard for a server to push a stream of text messages to a client over one open HTTP connection.
description: The text/event-stream format behind streamed replies, and how to read a luv13 stream line by line in Python.
category: general
author: Quill
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- SSE is a one-way stream: the server sends, the client listens.
- Each message is a line starting with `data:` followed by a blank line.
- OpenAI-compatible APIs use SSE to deliver streamed replies, with one JSON chunk per `data:` line.
- In the OpenAI format, the stream ends with a final `data: [DONE]` line.
- Most SDKs parse SSE for you. You only need the details if you read the raw stream yourself.

## The format

SSE is defined in the HTML standard. The server responds with the content type `text/event-stream` and keeps the connection open. It then writes messages like this:

```
data: {"choices":[{"delta":{"content":"Hel"}}]}

data: {"choices":[{"delta":{"content":"lo"}}]}

data: [DONE]
```

These chunks are shortened examples. Real chunks carry more fields, such as `id`, `model` and `finish_reason`.

The rules are short:

- A line that starts with `data:` holds the message text.
- A blank line marks the end of one message.
- Lines starting with `:` are comments. Servers sometimes send them to keep the connection alive, and clients should ignore them.

## Reading a stream by hand

1. Read the response line by line.
2. Skip empty lines and lines starting with `:`.
3. For each `data:` line, strip the prefix.
4. If the rest is `[DONE]`, stop. Otherwise parse it as JSON and take `choices[0].delta.content`.

A network read can split a line in the middle, so buffer text until you reach a newline before you parse.

## On luv13

To ask luv13 for a streamed reply, send `"stream": true`. See [Streaming](/docs/s/streaming) for the idea and [Streaming on luv13](/docs/s/streaming-on-luv13) for what's confirmed on luv13.

## Example

This Python sketch reads the raw SSE stream from luv13 using the `requests` library.

```python
import json, os, requests

resp = requests.post(
    "https://api.luv13.ai/v1/chat/completions",
    headers={"Authorization": f"Bearer {os.environ['LUV13_API_KEY']}"},
    json={
        "model": "luv13/glm-5.3-flash",
        "messages": [{"role": "user", "content": "Say hello."}],
        "stream": True,
    },
    stream=True,
    timeout=60,
)
for line in resp.iter_lines(decode_unicode=True):
    if not line or line.startswith(":") or not line.startswith("data:"):
        continue
    data = line[len("data:"):].strip()
    if data == "[DONE]":
        break
    chunk = json.loads(data)
    if chunk.get("choices"):
        print(chunk["choices"][0]["delta"].get("content") or "", end="", flush=True)
```
