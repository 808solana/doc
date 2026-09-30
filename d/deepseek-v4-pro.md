---
title: DeepSeek V4-Pro
definition: DeepSeek V4-Pro is a model served by luv13 under the id luv13/deepseek-v4-pro, with a 1M-token context and text input.
category: luv13
author: Ink
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- The model id is `luv13/deepseek-v4-pro`. Use it exactly as written in the `model` field.
- luv13.ai lists a context of 1M tokens.
- Input: text. Output: text.
- Price: a flat $0.33 per 1M tokens, input the same as output, like every luv13 model.

## Details

| Field | Value |
|---|---|
| Name on luv13.ai | DeepSeek V4-Pro |
| Model id | `luv13/deepseek-v4-pro` |
| Context | 1M tokens |
| Input | text |
| Output | text |
| Price | $0.33 per 1M tokens, input = output |
| Endpoint | `POST https://api.luv13.ai/v1/chat/completions` |

Values are from live `GET /v1/models` and luv13.ai/models on 2026-09-30. If they ever disagree with [Models](/docs/models), that page wins.

It accepts text only and returns text. To send images, pick a model that lists image input; see [Modalities](/docs/m/modalities).

The version numbers differ between the two DeepSeek models: this one is V4, the Flash model is V4.1. The ids are `luv13/deepseek-v4-pro` and `luv13/deepseek-v4.1-flash`.

luv13 also serves [DeepSeek V4.1 Flash](/docs/d/deepseek-v4-1-flash) (`luv13/deepseek-v4.1-flash`). luv13.ai doesn't publish how the two differ in speed or quality, so test both on your own prompts. Price is the same, so cost isn't a reason to pick one over the other.
<!-- TODO: ask the operator for a one-line, checkable difference between DeepSeek V4-Pro and DeepSeek V4.1 Flash. -->

## Example

```bash
curl https://api.luv13.ai/v1/chat/completions \
  -H "Authorization: Bearer $LUV13_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"model": "luv13/deepseek-v4-pro", "messages": [{"role": "user", "content": "ping"}]}'
```

A request that fills the whole 1M-token context costs about $0.33 in input alone, so trim history you don't need. See [Estimating Costs](/docs/e/estimating-costs).

<!-- TODO: confirm with the operator whether luv13/deepseek-v4-pro supports streaming and tool calling. -->
