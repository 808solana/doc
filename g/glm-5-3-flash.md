---
title: GLM-5.3 Flash
definition: GLM-5.3 Flash is a model served by luv13 under the id luv13/glm-5.3-flash, with a 1M-token context and text, image and video input.
category: luv13
author: Ink
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- The model id is `luv13/glm-5.3-flash`. Use it exactly as written in the `model` field.
- luv13.ai lists a context of 1M tokens.
- Input: text, image and video. Output: text.
- Price: a flat $0.33 per 1M tokens, input the same as output, like every luv13 model.

## Details

| Field | Value |
|---|---|
| Name on luv13.ai | GLM-5.3 Flash |
| Model id | `luv13/glm-5.3-flash` |
| Context | 1M tokens |
| Input | text, image and video |
| Output | text |
| Price | $0.33 per 1M tokens, input = output |
| Endpoint | `POST https://api.luv13.ai/v1/chat/completions` |

Values are from live `GET /v1/models` and luv13.ai/models on 2026-09-30. If they ever disagree with [Models](/docs/models), that page wins.

It accepts text, image and video input and returns text. For how to send an image, see [Image Input](/docs/i/image-input).

It's the model luv13.ai/docs uses in its first example request.

luv13 also serves [GLM 5.3](/docs/g/glm-5-3) (`luv13/glm-5.3`). luv13.ai doesn't publish how the two differ in speed or quality, so test both on your own prompts. Price is the same, so cost isn't a reason to pick one over the other.
<!-- TODO: ask the operator for a one-line, checkable difference between GLM-5.3 Flash and GLM 5.3. -->

## Example

```bash
curl https://api.luv13.ai/v1/chat/completions \
  -H "Authorization: Bearer $LUV13_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"model": "luv13/glm-5.3-flash", "messages": [{"role": "user", "content": "ping"}]}'
```

A request that fills the whole 1M-token context costs about $0.33 in input alone, so trim history you don't need. See [Estimating Costs](/docs/e/estimating-costs).

<!-- TODO: confirm with the operator whether luv13/glm-5.3-flash supports streaming and tool calling. -->
