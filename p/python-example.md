---
title: Python Example
definition: The Python example is a short script that calls luv13 with the official OpenAI Python SDK.
category: luv13
author: Ink
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- Install the SDK with `pip install openai` and set `base_url` to `https://api.luv13.ai/v1`.
- Read your key from the `LUV13_API_KEY` environment variable.
- Tested on 2026-09-30 with `openai` 3.22.1: listing models returned all seven ids, and the chat call without a key raised `AuthenticationError` (401).
- For plain HTTP without the SDK, see [Python Requests](/docs/p/python-requests).

## The script

```python
import os
import openai
from openai import OpenAI

client = OpenAI(
    base_url="https://api.luv13.ai/v1",
    api_key=os.environ["LUV13_API_KEY"],
)

for model in client.models.list():
    print(model.id)

try:
    reply = client.chat.completions.create(
        model="luv13/glm-5.3-flash",
        messages=[{"role": "user", "content": "ping"}],
    )
    print(reply.choices[0].message.content)
except openai.AuthenticationError:
    raise SystemExit("401: check LUV13_API_KEY.")
```

```bash
pip install openai
export LUV13_API_KEY=sk-luv13-...
python luv13_example.py
```

The SDK retries 429 and 5xx responses twice by default; see [Retrying Requests](/docs/r/retrying-requests). For the SDKs in general, see [Using the OpenAI SDKs](/docs/u/using-the-openai-sdks).
