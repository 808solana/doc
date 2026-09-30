---
title: System Prompts
definition: A system prompt is an instruction at the start of a conversation that sets how the model should behave for every reply that follows.
category: general
author: Quill
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- In the OpenAI format it's a message with `"role": "system"`, placed first in `messages`.
- Use it for lasting rules: role, tone, format, and what to avoid.
- The model has no memory, so you send the system prompt again with every request.
- It counts toward input tokens and the [Context Window](/docs/c/context-window) each time.
- It guides the model but doesn't guarantee behavior, so still check outputs in code.

## What goes in it

A good system prompt is short and specific. Common parts:

- **Role:** "You are a support assistant for a bike shop."
- **Tone and length:** "Answer in plain English, in three sentences or fewer."
- **Format:** "Reply in Markdown" or "Reply with JSON only."
- **Limits:** "If you don't know, say so. Don't make up prices."

Put the task itself (the question, the document) in the user message. Keep the system prompt for rules that apply to every turn.

## Tips

- State rules plainly. One instruction per sentence is easier for the model to follow.
- Say what to do, not only what not to do.
- Test with tricky inputs. Users may ask the model to ignore its rules. See [Prompt Injection](/docs/p/prompt-injection).
- Different models follow system prompts with different strictness. Test on the model you'll use.

## Example

```bash
curl https://api.luv13.ai/v1/chat/completions \
  -H "Authorization: Bearer $LUV13_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "luv13/glm-5.3-flash",
    "messages": [
      {"role": "system", "content": "You are a friendly tutor. Answer in two sentences or fewer."},
      {"role": "user", "content": "What is a context window?"}
    ]
  }'
```
