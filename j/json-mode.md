---
title: JSON Mode
definition: JSON mode is a request option that tells a model to reply with valid JSON instead of free text.
category: general
author: Quill
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- In the OpenAI format it's `"response_format": {"type": "json_object"}`.
- It aims for output that parses as JSON. It doesn't enforce a particular set of keys.
- You still need to say in the prompt that you want JSON, and describe the fields.
- For a fixed schema, providers offer structured outputs. See luv13's [Structured Outputs](/docs/s/structured-outputs).
- Always parse and validate the reply in code.

## Why use it

When code reads the model's answer, free text is fragile. The model might add "Sure! Here's your data:" before the JSON, or wrap it in a code fence. JSON mode is meant to stop that, so `json.loads()` or `JSON.parse()` works on the reply.

## JSON mode vs. structured outputs

| | JSON mode | Structured outputs |
|---|---|---|
| Output parses as JSON | Aimed for | Aimed for |
| Matches your exact schema | No | Yes, where supported |
| How you set it | `{"type": "json_object"}` | `{"type": "json_schema", ...}` |

Support for each varies by provider and model. For what luv13 supports, see [Structured Outputs](/docs/s/structured-outputs) and [Request Parameters](/docs/r/request-parameters).

## Tips

- Describe the shape in the prompt: field names, types, and an example.
- Set `max_tokens` high enough. A reply cut off early (`finish_reason: length`) is broken JSON.
- If your code gets bad JSON, retry once, then fall back or report the error.

## Example

```bash
curl https://api.luv13.ai/v1/chat/completions \
  -H "Authorization: Bearer $LUV13_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "luv13/glm-5.3-flash",
    "messages": [
      {"role": "system", "content": "Reply with JSON only, shaped like {\"city\": string, \"country\": string}."},
      {"role": "user", "content": "Where is the Eiffel Tower?"}
    ],
    "response_format": {"type": "json_object"}
  }'
```

If luv13 or the model doesn't support `response_format`, the prompt instructions alone often still produce JSON, but validate it either way.
