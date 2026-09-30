---
title: Reasoning Models
definition: A reasoning model is a language model trained to work through a problem in intermediate steps before it gives its final answer.
description: How reasoning models trade speed and tokens for better answers on hard problems, and tips for using them through luv13.
category: general
author: Quill
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- Reasoning models spend extra tokens "thinking" before they answer.
- They tend to do better on math, logic, planning and hard coding tasks.
- They're usually slower and use more output tokens per answer.
- Some APIs return the thinking in a separate field, some hide it, and some mix it into the reply.
- For simple tasks, a fast non-reasoning model is often the better choice.

## How they differ

A standard chat model starts writing its answer right away. A reasoning model first produces a chain of intermediate steps, then the answer. The steps let it catch mistakes and break a big problem into smaller ones.

That extra work has costs:

- **Latency.** The first word of the final answer can take much longer. See [Latency](/docs/l/latency).
- **Tokens.** The reasoning usually counts as output tokens, so answers cost more. See [Input vs. Output Tokens](/docs/i/input-vs-output-tokens).
- **Settings.** Some reasoning models ignore or limit [Temperature](/docs/t/temperature) and similar options.

## Working with them

- Leave a generous `max_tokens`. If thinking uses up the budget, the answer can be cut short.
- Ask for the result you want, not a long list of steps. The model plans on its own.
- Some OpenAI-compatible tools send a `reasoning_effort` setting. Whether a given model or provider honors it varies.
- If a client shows a reasoning field, don't feed it back to users as the final answer.

## On luv13

luv13 lists its models at `https://api.luv13.ai/v1/models`, but that list doesn't say which ones are reasoning models. See the [luv13 model list](https://luv13.ai/#models), [Listing Models](/docs/l/listing-models) and [Request Parameters](/docs/r/request-parameters) for what luv13 supports. For a prompting approach that works on any model, see [Chain-of-Thought Prompting](/docs/c/chain-of-thought-prompting).

## Example

A hard multi-step question with plenty of room for the answer:

```bash
curl https://api.luv13.ai/v1/chat/completions \
  -H "Authorization: Bearer $LUV13_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "luv13/glm-5.3-flash",
    "messages": [{"role": "user", "content": "A train leaves at 2:40 p.m. and the trip takes 3 hours 35 minutes. What time does it arrive?"}],
    "max_tokens": 2000
  }'
```
