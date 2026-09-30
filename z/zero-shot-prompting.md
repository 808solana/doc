---
title: Zero-Shot Prompting
definition: Zero-shot prompting means asking a model to do a task with instructions only, without giving it any examples.
description: When a plain instruction with no examples is enough, and the signs that a luv13 prompt needs examples or a stricter format.
category: general
author: Quill
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- You describe the task and the model does it, with no sample answers.
- It's the simplest prompt and the cheapest in input tokens.
- It works well for common tasks like summarizing, translating and answering questions.
- If the output format drifts or labels are inconsistent, add examples. See [Few-Shot Prompting](/docs/f/few-shot-prompting).

## When it's enough

Modern chat models are trained to follow instructions, so many tasks need nothing more than a clear request. Zero-shot is a good first try because it's quick to write and easy to change.

It works best when:

- The task is common and well defined.
- You spell out the output format in words ("Reply with one word", "Use a bulleted list").
- There's little room for different reasonable answers.

## When to add more

Move to few-shot prompting or stricter formats when:

- The model uses slightly different labels or wording each time.
- The task uses your own categories or style that it can't guess.
- You need machine-readable output. See [JSON Mode](/docs/j/json-mode).

## Example

```bash
curl https://api.luv13.ai/v1/chat/completions \
  -H "Authorization: Bearer $LUV13_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "luv13/glm-5.3-flash",
    "messages": [
      {"role": "user", "content": "Translate to Spanish. Reply with the translation only: Where is the train station?"}
    ]
  }'
```
