---
title: Input vs. Output Tokens
definition: Input tokens are the tokens you send to a model, and output tokens are the tokens it writes back.
category: general
author: Quill
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- Input tokens (also called prompt tokens) are everything in your request.
- Output tokens (also called completion tokens) are everything in the model's reply.
- API responses report both counts in the `usage` field.
- Many providers charge more for output than for input. luv13 charges the same rate for both.
- You control input size with what you send, and output size with `max_tokens`.

## Input tokens

These are the tokens in your request: the system message, the chat history, your new message, and any tool definitions or tool results. Because a model has no memory between calls, a long chat re-sends its history each time, so input tokens grow with every turn.

## Output tokens

These are the tokens the model generates in its reply, including any tool calls it makes. You can set a ceiling with the `max_tokens` parameter. If the reply hits that ceiling, it stops early, and `finish_reason` is `length` instead of `stop`.

## Reading the counts

Each response reports both counts:

```json
"usage": {
  "prompt_tokens": 850,
  "completion_tokens": 120,
  "total_tokens": 970
}
```

These numbers are an example. `prompt_tokens` is input, `completion_tokens` is output, and `total_tokens` is the sum.

## How pricing uses them

Your cost for a request is the input tokens times the input price plus the output tokens times the output price. On luv13 the input and output prices are the same, a flat $0.33 per 1 million tokens on every model, so you can just multiply `total_tokens` by that one rate. See [Pricing](/docs/pricing).

For the example above, 970 total tokens cost 970 / 1,000,000 x $0.33, or about $0.00032.

## Example

This request caps the reply at 100 output tokens. The `usage` field in the response shows both counts.

```bash
curl https://api.luv13.ai/v1/chat/completions \
  -H "Authorization: Bearer $LUV13_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "luv13/glm-5.3-flash",
    "messages": [{"role": "user", "content": "Explain tokens in two sentences."}],
    "max_tokens": 100
  }'
```
