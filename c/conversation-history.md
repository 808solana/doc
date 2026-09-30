---
title: Conversation History
definition: Conversation history is the list of earlier messages you send with each request so a model can follow an ongoing chat.
category: general
author: Quill
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- Chat APIs don't remember past requests. Each call only knows what's in its `messages` list.
- To continue a chat, you send the earlier user and assistant messages again, oldest first, plus the new one.
- The history counts as input tokens every time, so long chats cost more per turn.
- When the history gets too long for the [Context Window](/docs/c/context-window), trim or summarize it.
- Your app, not the API, stores the history.

## How it works

Turn one sends a system message and a user message. The model replies. For turn two, you send all three messages (system, user, assistant reply) plus the new user message. Turn three sends all five plus the next one, and so on.

Because the model sees the whole list, it can refer back to earlier answers. Because it sees *only* that list, anything you leave out is forgotten.

## Keeping it under control

- **Drop the oldest turns** once you pass a set number of messages or tokens. Keep the system message.
- **Summarize** older turns into one short message and keep the recent ones in full.
- **Remove bulky content** that's no longer needed, such as large tool results or pasted files.
- **Watch `usage.prompt_tokens`** to see how big the history has become. See [Input vs. Output Tokens](/docs/i/input-vs-output-tokens).

## Example

The second turn of a chat, with the first turn included so the model knows what "it" means:

```bash
curl https://api.luv13.ai/v1/chat/completions \
  -H "Authorization: Bearer $LUV13_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "luv13/glm-5.3-flash",
    "messages": [
      {"role": "system", "content": "You are a helpful travel assistant."},
      {"role": "user", "content": "Suggest a city for a weekend trip in October."},
      {"role": "assistant", "content": "Santa Fe, New Mexico, is a good pick. The weather is mild and the fall colors are out."},
      {"role": "user", "content": "What should I pack for it?"}
    ]
  }'
```
