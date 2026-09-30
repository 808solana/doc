---
title: Fine-Tuning
definition: Fine-tuning is further training of an existing model on your own examples so it learns a specific task, style or format.
description: What fine-tuning involves, when prompts or retrieval are a better choice, and how to steer luv13 models without it.
category: general
author: Quill
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- Fine-tuning changes a model's weights using a set of example inputs and ideal outputs.
- It's useful when prompting alone can't get a consistent style or format.
- It needs good training data, time and money, and the result has to be hosted somewhere.
- Try [Prompt Engineering](/docs/p/prompt-engineering), [Few-Shot Prompting](/docs/f/few-shot-prompting) and [Retrieval-Augmented Generation](/docs/r/retrieval-augmented-generation) first. They're cheaper and faster to change.
- luv13 doesn't offer fine-tuning. OpenAI's `/v1/fine_tuning/jobs` path returned 404 on 2026-09-30.

## How it works

1. **Collect examples.** Hundreds to thousands of input and output pairs that show exactly what you want.
2. **Train.** Start from a base model and train it a bit more on your examples.
3. **Evaluate.** Compare the tuned model with the original on test cases it didn't train on.
4. **Serve.** Host the new weights so you can call them.

Lighter methods such as LoRA train a small add-on instead of every weight. They need far less memory and are common with [Open-Weight Models](/docs/o/open-weight-models).

## Fine-tuning vs. other options

| Goal | Usually best |
|---|---|
| Answer from your documents or fresh facts | Retrieval-augmented generation |
| Follow a format a few times | Few-shot examples in the prompt |
| Match a narrow style every time, at scale | Fine-tuning |
| Teach new facts | Retrieval, not fine-tuning. Tuning is poor at adding reliable facts. |

## On luv13

luv13 serves the models in its live list at `https://api.luv13.ai/v1/models` as they are. To steer them, use prompts and examples. See [Endpoints](/docs/e/endpoints) for what luv13 serves.

## Example

A few-shot prompt is often enough to get the style you'd otherwise fine-tune for:

```bash
curl https://api.luv13.ai/v1/chat/completions \
  -H "Authorization: Bearer $LUV13_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "luv13/glm-5.3-flash",
    "messages": [
      {"role": "system", "content": "Rewrite product names in our house style: all lowercase, words joined by dots."},
      {"role": "user", "content": "Blue Water Bottle"},
      {"role": "assistant", "content": "blue.water.bottle"},
      {"role": "user", "content": "Travel Coffee Mug"}
    ]
  }'
```
