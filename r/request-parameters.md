---
title: Request Parameters
definition: Request parameters are the fields in the JSON body of a luv13 chat completion request; model and messages are the ones luv13 documents today.
description: The two fields luv13 documents, model and messages, and links for the optional OpenAI fields whose support is unconfirmed.
category: luv13
author: Ink
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- luv13.ai/docs shows two fields in its example request: `model` and `messages`.
- `model` must be one of the seven ids from `GET /v1/models`, such as `luv13/glm-5.3-flash`.
- luv13 uses the OpenAI chat completions format, so any other field uses its OpenAI name.
- luv13 hasn't published which optional fields each model honors. Test any you rely on.

## Documented fields

| Field | What it is |
|---|---|
| `model` | The model id, exactly as `GET /v1/models` lists it. See [Model IDs](/docs/m/model-ids). |
| `messages` | The conversation, as a list of messages with a `role` and `content`. |

```bash
curl https://api.luv13.ai/v1/chat/completions \
  -H "Authorization: Bearer $LUV13_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"model": "luv13/glm-5.3-flash", "messages": [{"role": "user", "content": "ping"}]}'
```

## Optional fields

These OpenAI fields have general pages. Whether luv13 honors each one isn't published yet.

| Field | General page |
|---|---|
| `temperature` | [Temperature](/docs/t/temperature) |
| `top_p` | [Nucleus Sampling](/docs/n/nucleus-sampling) |
| `stop` | [Stop Sequences](/docs/s/stop-sequences) |
| `stream` | [Streaming on luv13](/docs/s/streaming-on-luv13) |
| `tools` | [Tool Calling on luv13](/docs/t/tool-calling-on-luv13) |
| `response_format` | [Structured Outputs](/docs/s/structured-outputs) |

<!-- TODO: fill in from the operator's answer on which parameters luv13 honors, per model, and what happens to an unsupported one. -->

To test a field yourself, send the same prompt with and without it and compare the replies.
