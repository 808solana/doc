---
title: Temperature
definition: Temperature is a sampling setting that controls how random a model's word choices are.
category: general
author: Quill
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- Low temperature makes replies more focused and repeatable. High temperature makes them more varied.
- In the OpenAI format it's the `temperature` field, usually a number from 0 to 2.
- A value of 0 is close to always picking the most likely next token, but it doesn't promise identical replies every time.
- Change temperature or [Nucleus Sampling](/docs/n/nucleus-sampling) (`top_p`), not usually both.
- Check [Request Parameters](/docs/r/request-parameters) for how luv13 handles it.

## What it does

At each step, a model gives every possible next token a probability. Temperature reshapes those probabilities before one is picked:

- **Below 1**, the likely tokens get even more likely. Output becomes steadier and more predictable.
- **At 1**, the probabilities are used as the model gave them.
- **Above 1**, the odds flatten out, so less likely tokens get picked more often. Output becomes more surprising, and at high values it can drift into nonsense.

## Picking a value

These are rough starting points, not rules:

| Task | Example temperature |
|---|---|
| Code, data extraction, factual answers | 0 to 0.3 |
| General chat and writing | 0.5 to 0.8 |
| Brainstorming and creative writing | 0.9 to 1.2 |

Some models, especially [Reasoning Models](/docs/r/reasoning-models), have their own recommended settings or ignore temperature. Test on your own prompts.

## Example

```bash
curl https://api.luv13.ai/v1/chat/completions \
  -H "Authorization: Bearer $LUV13_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "luv13/glm-5.3-flash",
    "messages": [{"role": "user", "content": "Suggest a name for a coffee shop."}],
    "temperature": 1.0
  }'
```

Run it a few times, then try `0.2`, and compare how much the answers change.
