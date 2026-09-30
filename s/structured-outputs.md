---
title: Structured Outputs
definition: Structured outputs are chat completion replies constrained to JSON, requested on luv13 with the OpenAI response_format field.
category: luv13
author: Ink
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- In the OpenAI format, `response_format` asks the model for JSON instead of free text.
- `{"type": "json_object"}` (often called JSON mode) asks for any valid JSON. `{"type": "json_schema", ...}` asks for JSON that matches a schema you give.
- luv13 hasn't confirmed which models honor either form. Validate every reply in your code.
- Asking for JSON in the prompt as well makes the result more reliable on any model.

<!-- TODO: confirm with the operator whether luv13 passes response_format through, which types (json_object, json_schema) each of the seven models honors, and what happens when a model doesn't support it. -->

## JSON mode

```bash
curl https://api.luv13.ai/v1/chat/completions \
  -H "Authorization: Bearer $LUV13_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "luv13/deepseek-v4-pro",
    "messages": [
      {"role": "system", "content": "Reply with a JSON object with keys name and year."},
      {"role": "user", "content": "The first Moon landing."}
    ],
    "response_format": {"type": "json_object"}
  }'
```

The JSON arrives as a string in `choices[0].message.content`. Parse it yourself.

## JSON schema

```json
"response_format": {
  "type": "json_schema",
  "json_schema": {
    "name": "event",
    "schema": {
      "type": "object",
      "properties": {"name": {"type": "string"}, "year": {"type": "integer"}},
      "required": ["name", "year"]
    }
  }
}
```

## Handling the reply safely

- Parse inside a try/except (or try/catch). If parsing fails, retry once or fall back.
- Check the `finish_reason`. If it's `length`, the JSON was cut off by the token limit; raise `max_tokens`. See [Finish Reasons](/docs/f/finish-reasons).
- Check required keys and types before you use the data.

If you need the model to call your code rather than return data, use [Tool Calling on luv13](/docs/t/tool-calling-on-luv13).
