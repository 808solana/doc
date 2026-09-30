---
title: Finish Reasons
definition: A finish reason is the field in an OpenAI-format chat completion that says why the model stopped; which values luv13 returns isn't verified yet.
description: Where to find finish_reason in a luv13 reply and how to check it, while luv13's exact values are still being confirmed.
category: luv13
author: Ink
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- luv13 uses the OpenAI chat completions format, in which each choice carries a `finish_reason`.
- Which values luv13 returns, per model, hasn't been verified or published yet.
- Check the field on every response before trusting that a reply is complete.

<!-- TODO: verify with a real authenticated luv13 response which finish_reason values each model returns, then add the table of values. -->

## In general

In the OpenAI format, `stop` means the model ended on its own and `length` means it ran out of room, so the text is cut off. See [Stop Sequences](/docs/s/stop-sequences) and [Context Window](/docs/c/context-window).

## Check it

```bash
curl -s https://api.luv13.ai/v1/chat/completions \
  -H "Authorization: Bearer $LUV13_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"model": "luv13/glm-5.3-flash", "messages": [{"role": "user", "content": "ping"}]}' \
  | jq '.choices[].finish_reason'
```
