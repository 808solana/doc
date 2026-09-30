---
title: Python Requests
definition: The Python requests library can call luv13's OpenAI-compatible API directly with plain HTTP, without an SDK.
category: general
author: Quill
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- `requests` is a popular Python HTTP library. Install it with `pip install requests`.
- A chat call is one `POST` to `https://api.luv13.ai/v1/chat/completions` with a JSON body.
- Send your key in an `Authorization: Bearer` header, read from an environment variable.
- Always set a `timeout`. By default `requests` waits forever.
- Call `raise_for_status()` or check `status_code` before reading the reply.

## When to use it

The [OpenAI SDKs](/docs/u/using-the-openai-sdks) handle retries and parsing for you. Plain `requests` is handy when you want no extra dependencies, want to see exactly what's sent, or are debugging.

## Example

```python
import os
import requests

resp = requests.post(
    "https://api.luv13.ai/v1/chat/completions",
    headers={"Authorization": f"Bearer {os.environ['LUV13_API_KEY']}"},
    json={
        "model": "luv13/glm-5.3-flash",
        "messages": [{"role": "user", "content": "Give me one tip for clean code."}],
    },
    timeout=60,
)
resp.raise_for_status()
data = resp.json()
print(data["choices"][0]["message"]["content"])
print(data["usage"])
```

Passing a dict to `json=` encodes the body and sets `Content-Type: application/json` for you.

## Handling errors

- A `401` means the key is missing or wrong.
- A `429` means you're sending too fast. Wait and retry. See [Retrying Requests](/docs/r/retrying-requests).
- A `5xx` means a server-side problem. Retrying later often works.

For luv13's own error codes, see [Errors and Status Codes](/docs/e/errors-and-status-codes). To read a streamed reply with `requests`, see [Server-Sent Events](/docs/s/server-sent-events).
