---
title: Stop Sequences
definition: A stop sequence is a string that tells the model to stop writing as soon as it would produce that text.
category: general
author: Quill
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- In the OpenAI format it's the `stop` field: one string or a short list of strings.
- When the model's output would include a stop string, generation ends right there.
- The stop string itself is left out of the reply.
- When a stop sequence ends the reply, `finish_reason` is `stop`.
- Check [Request Parameters](/docs/r/request-parameters) to confirm luv13 supports `stop` for your model.

## Why use them

Stop sequences keep replies from running past the part you want. Common uses:

- **One item only.** Stop at `"\n"` to get a single line.
- **Fixed formats.** If you've asked for an answer followed by a marker like `END`, stop at `"END"`.
- **Role play or transcripts.** Stop at `"User:"` so the model doesn't write the other side of the conversation.

They also save output tokens, because the model stops early instead of writing text you'd throw away.

## Things to watch

- Pick strings that won't show up by accident in a good answer.
- The match is on exact text. `"END"` won't match `"end"`.
- Stop sequences are a cutoff, not a format guarantee. If you need structured data, see [JSON Mode](/docs/j/json-mode).

## Example

This asks for a list but stops after the first line.

```bash
curl https://api.luv13.ai/v1/chat/completions \
  -H "Authorization: Bearer $LUV13_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "luv13/glm-5.3-flash",
    "messages": [{"role": "user", "content": "List three fruits, one per line."}],
    "stop": ["\n"]
  }'
```
