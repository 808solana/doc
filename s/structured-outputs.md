---
title: Structured Outputs
definition: Structured outputs are replies constrained to JSON; on luv13 they would be requested with the OpenAI response_format field, and support isn't published yet.
category: luv13
author: Ink
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- In the OpenAI chat completions format, JSON output is requested with the `response_format` field.
- luv13 hasn't published whether it passes `response_format` through, or for which models.
- Asking for JSON in the prompt and validating the reply in your code works whatever the model supports.

<!-- TODO: fill in from the operator's answer on response_format (json_object, json_schema) support per model. -->

## How it works in general

See [JSON Mode](/docs/j/json-mode) for the OpenAI `response_format` options.

## A safe pattern on luv13

1. Say in the prompt exactly which JSON keys you want.
2. Parse the reply inside a try/except (or try/catch).
3. Check required keys and types before using the data.
4. If parsing fails, retry once or fall back.

```bash
curl https://api.luv13.ai/v1/chat/completions \
  -H "Authorization: Bearer $LUV13_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"model": "luv13/glm-5.3-flash", "messages": [{"role": "user", "content": "Reply only with a JSON object with keys name and year for the first Moon landing."}]}'
```

If the model should call your code instead of returning data, see [Tool Calling on luv13](/docs/t/tool-calling-on-luv13).
