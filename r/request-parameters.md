---
title: Request Parameters
definition: Request parameters are the fields you put in the JSON body of a luv13 chat completion request to choose the model, pass the conversation and shape the reply.
category: luv13
author: Ink
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- Two parameters are required: `model` and `messages`.
- `model` must be one of the seven ids from `GET /v1/models`, such as `luv13/kimi-k3`.
- luv13 uses the OpenAI chat completions format, so optional parameters use OpenAI's names.
- luv13 hasn't published which optional parameters each model honors. Test any you rely on.

## Required

| Parameter | Type | What it does |
|---|---|---|
| `model` | string | The model id, exactly as listed by `GET /v1/models`. See [Model IDs](/docs/m/model-ids). |
| `messages` | array | The conversation, oldest first. Each item has a `role` and `content`. |

## Common optional parameters

These are the OpenAI names. Whether luv13 passes each one through to every model is unconfirmed.

<!-- TODO: confirm with the operator which of these luv13 honors, per model, and what happens to an unsupported one (ignored or rejected). -->

| Parameter | In the OpenAI format it... |
|---|---|
| `max_tokens` | Caps how many output tokens the reply can use. Useful for keeping cost down, since output is billed at the same $0.33 per 1M rate as input. |
| `temperature` | Sets randomness. Lower is more predictable. |
| `top_p` | An alternative way to limit randomness. Change this or `temperature`, not both. |
| `stop` | One or more strings that end the reply when the model produces them. |
| `stream` | Returns the reply in pieces. See [Streaming on luv13](/docs/s/streaming-on-luv13). |
| `tools`, `tool_choice` | Let the model ask to call your functions. See [Tool Calling on luv13](/docs/t/tool-calling-on-luv13). |
| `response_format` | Asks for JSON output. See [Structured Outputs](/docs/s/structured-outputs). |

## Example

```bash
curl https://api.luv13.ai/v1/chat/completions \
  -H "Authorization: Bearer $LUV13_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "luv13/glm-5.3-flash",
    "messages": [{"role": "user", "content": "Name three primary colors."}],
    "max_tokens": 50,
    "temperature": 0.2
  }'
```

## Checking a parameter yourself

Send the same prompt twice, once with the parameter and once without, and compare. For `max_tokens`, a small value should give a shorter reply and a lower `usage.completion_tokens`. If nothing changes, the parameter may not be honored for that model.
