---
title: Qwen 3.8 27B
definition: Qwen 3.8 27B is a model served by luv13 under the id luv13/qwen-3.8-27b, with a 1M-token context and text, image and video input.
category: luv13
author: Ink
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- The model id is `luv13/qwen-3.8-27b`. Use it exactly as written in the `model` field.
- luv13.ai lists a context of 1M tokens.
- Input: text, image and video. Output: text.
- Price: a flat $0.33 per 1M tokens, input the same as output, like every luv13 model.

## Details

| Field | Value |
|---|---|
| Name on luv13.ai | Qwen 3.8 27B |
| Model id | `luv13/qwen-3.8-27b` |
| Context | 1M tokens |
| Input | text, image and video |
| Output | text |
| Price | $0.33 per 1M tokens, input = output |
| Endpoint | `POST https://api.luv13.ai/v1/chat/completions` |

Values are from live `GET /v1/models` and luv13.ai/models on 2026-09-30. If they ever disagree with [Models](/docs/models), that page wins.

It accepts text, image and video input and returns text. For how to send an image, see [Image Input](/docs/i/image-input).

It's the only Qwen model luv13 lists, and the only one whose luv13 name includes a parameter count (27B).

luv13.ai doesn't publish speed or quality figures for it, so test it on your own prompts against the other six models. Price is the same on all seven.

## Example

```bash
curl https://api.luv13.ai/v1/chat/completions \
  -H "Authorization: Bearer $LUV13_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"model": "luv13/qwen-3.8-27b", "messages": [{"role": "user", "content": "ping"}]}'
```

A request that fills the whole 1M-token context costs about $0.33 in input alone, so trim history you don't need. See [Estimating Costs](/docs/e/estimating-costs).

<!-- TODO: confirm with the operator whether luv13/qwen-3.8-27b supports streaming and tool calling. -->
