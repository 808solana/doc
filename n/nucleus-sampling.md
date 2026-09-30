---
title: Nucleus Sampling
definition: Nucleus sampling, set with top_p, makes a model pick each next token only from the smallest group of likely tokens whose probabilities add up to a set share.
category: general
author: Quill
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- It's also called top-p sampling. In the OpenAI format the field is `top_p`, a number from 0 to 1.
- With `top_p` at 0.9, the model only picks from the most likely tokens that together cover 90% of the probability.
- Lower values cut off unlikely words and make output more focused. `1` means no cutoff.
- It was introduced in the 2019 paper "The Curious Case of Neural Text Degeneration" by Holtzman and others.
- Adjust `top_p` or [Temperature](/docs/t/temperature), usually not both at once.

## How it works

At each step the model ranks every possible next token by probability. Nucleus sampling:

1. Sorts the tokens from most to least likely.
2. Adds up their probabilities until the total reaches `top_p`.
3. Throws away everything below that line (the long tail).
4. Picks the next token at random from what's left, weighted by probability.

The group that's left is the "nucleus". When the model is confident, the nucleus may be one or two tokens. When it's unsure, the nucleus is bigger. That's the main difference from a fixed "top-k" cutoff, which always keeps the same number of tokens.

## Picking a value

As an example, many apps leave `top_p` at 1 and tune temperature instead. If you do use it, values from 0.8 to 0.95 trim the oddest word choices while still leaving room for variety.

## On luv13

Check [Request Parameters](/docs/r/request-parameters) to confirm whether luv13 passes `top_p` through for your model.

## Example

```bash
curl https://api.luv13.ai/v1/chat/completions \
  -H "Authorization: Bearer $LUV13_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "luv13/glm-5.3-flash",
    "messages": [{"role": "user", "content": "Write one line about the desert."}],
    "top_p": 0.9
  }'
```
