---
title: Quantization
definition: Quantization shrinks a model by storing its weights with fewer bits, which saves memory and speeds it up at some cost to accuracy.
description: How storing model weights with fewer bits saves memory, what it can cost in quality, and why it matters when using hosted models.
category: general
author: Quill
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- Model weights are usually trained in 16-bit or 32-bit numbers. Quantization stores them in 8, 4 or even fewer bits.
- Fewer bits means less memory and often faster replies.
- Going too low can hurt quality, especially on hard reasoning and code.
- It's most common when people run [Open-Weight Models](/docs/o/open-weight-models) on their own hardware.
- Two services offering "the same model" may run different quantizations, so results and speed can differ.

## How it works

Each weight is a number. At 16 bits, a model with 30 billion weights needs about 60 GB just for the weights. At 4 bits, the same model needs about 15 GB. (These are rough example numbers. Real memory use is higher once you add working memory.)

To go from many bits to few, the method rounds each weight to a nearby value it can store. Good methods pick the rounding carefully so the model's behavior changes as little as possible.

## Common formats you'll see

- **8-bit (INT8, FP8):** usually very close to the original quality.
- **4-bit:** a big memory saving with a small, often acceptable, quality drop.
- **File formats** like GGUF (used by llama.cpp) often label the level in the file name, such as `Q4` or `Q8`.

## Why it matters on an API

When you call a hosted model, you don't see its weights. The provider picks how to run it. If you compare the same model across providers and get different answers or speeds, quantization can be one reason. luv13's model list at `https://api.luv13.ai/v1/models` doesn't include quantization details. Ask the operator if it matters for your use.

## Example

A quick way to compare quality is to send the same fixed prompt to a model and review the answers against your own checks:

```bash
curl https://api.luv13.ai/v1/chat/completions \
  -H "Authorization: Bearer $LUV13_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "luv13/glm-5.3-flash",
    "messages": [{"role": "user", "content": "What is 17 times 23? Reply with the number only."}],
    "temperature": 0
  }'
```
