---
title: Model Fallback
definition: Model fallback means trying a second luv13 model id when the first one is unavailable, instead of failing or waiting.
description: A Python pattern that switches to another luv13 model id on availability errors but stops at once on a bad key.
category: luv13
author: Ink
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- All seven models cost the same $0.33 per 1M tokens, so falling back never changes the price.
- Fall back only on availability errors, not on 401 (a key problem affects every model) or 400-type request errors.
- Pick fallbacks that accept the same inputs. If you send images, only fall back to models that take images; see [the model list](/docs/models).

<!-- TODO: confirm with the operator how luv13 behaves under load, and the exact status code and body of a "model unavailable" error, so the check below can match it precisely. -->

## Python

```python
import os
import openai
from openai import OpenAI

client = OpenAI(base_url="https://api.luv13.ai/v1", api_key=os.environ["LUV13_API_KEY"])

MODELS = ["luv13/glm-5.3-flash", "luv13/kimi-k3-fast", "luv13/qwen-3.8-27b"]

def ask(messages):
    last_error = None
    for model in MODELS:
        try:
            return client.chat.completions.create(model=model, messages=messages)
        except openai.AuthenticationError:
            raise  # same key for every model; switching won't help
        except (openai.APIStatusError, openai.APIConnectionError) as e:
            last_error = e  # try the next model
    raise last_error

reply = ask([{"role": "user", "content": "ping"}])
print(reply.choices[0].message.content)
```

Each model is tried with the SDK's own retries first (2 by default), so a brief blip doesn't trigger a switch. See [Retrying Requests](/docs/r/retrying-requests).

## Things to keep in mind

- Different models give different answers. If output format matters, validate it after a fallback.
- Log which id you sent, so you know which model answered.
- For choosing between models on purpose (for example by task), see [Model Routing](/docs/m/model-routing).
- Check `GET /v1/models` now and then. If an id in your list disappears, replace it. See [Listing Models](/docs/l/listing-models).
