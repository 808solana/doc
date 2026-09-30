---
title: Chat Templates
definition: A chat template is the fixed text format a model uses to turn a list of chat messages into the single token sequence it was trained on.
description: How chat messages become one token sequence, why the format differs by model, and why luv13 users can skip templates entirely.
category: general
author: Quill
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- Under the hood, a model reads one long string, not a list of messages.
- The chat template adds special markers that show where each system, user and assistant message starts and ends.
- Each model family has its own template. Using the wrong one can make a model behave badly.
- When you call an API like luv13, the server applies the template for you. You just send `messages`.
- You deal with templates directly only when you run [Open-Weight Models](/docs/o/open-weight-models) yourself or use raw text completion.

## What a template does

Say you send:

```json
[
  {"role": "system", "content": "Be brief."},
  {"role": "user", "content": "Hi!"}
]
```

The server turns that into one string with role markers and special tokens, then adds the marker that means "the assistant speaks next". The exact markers differ by model family. That's the whole job of the template.

Templates also decide how tool definitions and tool results are laid out, which is one reason [Tool Calling](/docs/t/tool-calling) quality varies between models.

## When it matters to you

- **Running models locally.** Libraries such as Hugging Face Transformers store the template with the model and apply it with `apply_chat_template`. Local servers usually do it for you too.
- **Odd output.** If a self-hosted model repeats role names or never stops, the template is a common cause.
- **APIs.** With luv13 you never write a template. Send OpenAI-style `messages` to [Chat Completions](/docs/c/chat-completions) and luv13 handles the format.

## Example

```bash
curl https://api.luv13.ai/v1/chat/completions \
  -H "Authorization: Bearer $LUV13_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "luv13/glm-5.3-flash",
    "messages": [
      {"role": "system", "content": "Be brief."},
      {"role": "user", "content": "Hi!"}
    ]
  }'
```
