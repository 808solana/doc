---
title: Chain-of-Thought Prompting
definition: Chain-of-thought prompting means asking a model to work through a problem step by step before it gives the final answer.
category: general
author: Quill
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- Asking for step-by-step reasoning often improves answers on math, logic and multi-step problems.
- It works on regular chat models. [Reasoning Models](/docs/r/reasoning-models) do something similar on their own.
- The steps cost output tokens, so answers get longer and pricier.
- Ask for the final answer in a clear, separate spot so your code can find it.
- The technique was described in the 2022 paper "Chain-of-Thought Prompting Elicits Reasoning in Large Language Models" by Wei and others.

## How to use it

The simplest form is one added line: "Think step by step, then give the answer." You can also show a worked example with the steps written out, which is chain-of-thought combined with [Few-Shot Prompting](/docs/f/few-shot-prompting).

To keep the output usable:

- Ask for the answer on its own final line, or inside tags like `<answer>`. See [XML Prompts](/docs/x/xml-prompts).
- Only show the final answer to users unless they need the working.
- Leave enough `max_tokens` for both the steps and the answer.

## When to skip it

For simple lookups, rewrites or classification, step-by-step reasoning mostly adds tokens and time. With reasoning models, adding it is often unnecessary, because they already plan internally.

## Example

```bash
curl https://api.luv13.ai/v1/chat/completions \
  -H "Authorization: Bearer $LUV13_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "luv13/glm-5.3-flash",
    "messages": [
      {"role": "user", "content": "A shirt costs $20 after a 20% discount. What was the original price? Think step by step, then put only the final answer in <answer> tags."}
    ],
    "max_tokens": 400
  }'
```

The right answer is $25.
