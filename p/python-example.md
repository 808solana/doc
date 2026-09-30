---
title: Python Example
definition: The Python example is a complete script that calls luv13 with the official OpenAI Python SDK, from listing models to handling errors.
category: luv13
author: Ink
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- Install the OpenAI SDK with `pip install openai` and point it at `https://api.luv13.ai/v1`.
- Read your key from the `LUV13_API_KEY` environment variable.
- The script below was run on 2026-09-30 with `openai` 3.22.1: listing models returned all seven ids, and the chat call raised `AuthenticationError` (401) as expected without a key.
- For how the SDKs work with OpenAI-compatible APIs in general, see [Using the OpenAI SDKs](/docs/u/using-the-openai-sdks).

## The script

```python
import os
import openai
from openai import OpenAI

client = OpenAI(
    base_url="https://api.luv13.ai/v1",
    api_key=os.environ["LUV13_API_KEY"],
)

# 1. List the models you can use.
for model in client.models.list():
    print(model.id)

# 2. Send a message.
try:
    reply = client.chat.completions.create(
        model="luv13/glm-5.3-flash",
        messages=[{"role": "user", "content": "Say hello in five words."}],
    )
except openai.AuthenticationError:
    raise SystemExit("401: check LUV13_API_KEY.")
except openai.APIStatusError as e:
    raise SystemExit(f"luv13 returned HTTP {e.status_code}: {e.body}")

print(reply.choices[0].message.content)

# 3. What it cost: a flat $0.33 per 1M tokens, input = output.
if reply.usage:
    print(f"{reply.usage.total_tokens} tokens = ${reply.usage.total_tokens * 0.33 / 1_000_000:.6f}")
```

Run it:

```bash
pip install openai
export LUV13_API_KEY=sk-luv13-...
python luv13_example.py
```

## Notes

- `models.list()` returns the ids exactly as `GET /v1/models` lists them. Use those strings as `model`. See [Model IDs](/docs/m/model-ids).
- Without a valid key, the error body is `{'code': 401, 'message': 'unauthorized', 'type': 'invalid_auth'}`.
- The SDK retries 429 and 5xx responses twice by default. Pass `max_retries=` to change that. See [Retrying Requests](/docs/r/retrying-requests).
- To stream, add `stream=True` and loop over the result. See [Streaming on luv13](/docs/s/streaming-on-luv13).

<!-- TODO: run step 2 with a real key and confirm luv13 returns usage in the response. -->
