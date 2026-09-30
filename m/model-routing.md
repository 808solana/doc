---
title: Model Routing
definition: Model routing means sending each request to the model best suited for it, based on rules like task type, cost or speed.
description: How to send each request to a suitable model by task, length or result, with a small Python router for luv13.
category: general
author: Quill
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- Not every task needs the biggest model. Simple ones can go to a faster model.
- A router can be a few `if` statements in your code or a separate service.
- Route on things you can check: task type, prompt length, user tier, or a failed first attempt.
- With luv13, routing is just changing the `model` field, since every model uses the same base URL and key.
- Test each route on real inputs. Model names hint at speed or size, but they aren't a guarantee.

## Common routing rules

- **By task.** Short classification or formatting goes to a fast model. Hard reasoning or long code goes to a larger one.
- **By length.** Very long prompts go to a model with more room. luv13's list doesn't publish context lengths, so test before relying on this. See [Context Window](/docs/c/context-window).
- **By result.** Try a fast model first. If the answer fails a check (bad JSON, failed test), retry on a stronger model.
- **By availability.** If one model errors out, switch to another. See [Retrying Requests](/docs/r/retrying-requests).

## On luv13

All luv13 models share one base URL, one key and one flat price per token (see [Pricing](/docs/p/pricing)), so routing on luv13 is about speed and quality rather than cost per token. The ids come from the live list at `https://api.luv13.ai/v1/models`. See [Model IDs](/docs/m/model-ids).

## Example

A tiny router in Python. The rule is only an example. Set `LUV13_STRONG_MODEL` to another id from the live list that you've tested, or leave it unset to use the same model for both routes.

```python
import os
from openai import OpenAI

client = OpenAI(base_url="https://api.luv13.ai/v1",
                api_key=os.environ["LUV13_API_KEY"])

def pick_model(task: str) -> str:
    if task in ("classify", "extract", "rewrite"):
        return "luv13/glm-5.3-flash"
    return os.environ.get("LUV13_STRONG_MODEL", "luv13/glm-5.3-flash")

resp = client.chat.completions.create(
    model=pick_model("classify"),
    messages=[{"role": "user", "content": "Is this spam? 'You won a free cruise!' Reply yes or no."}],
)
print(resp.choices[0].message.content)
```

## Related

This page covers the general idea. For how luv13 handles a request when a model is unavailable, see [Model Fallback](/docs/m/model-fallback).
