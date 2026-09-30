---
title: Image Input
definition: Image input means sending a picture to a luv13 model that accepts images; five of the seven models list image input, and all return text.
description: Which of the seven luv13 model ids list image or video input, per luv13.ai, and what is still unconfirmed about sending images.
category: luv13
author: Ink
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- luv13.ai/models lists image input for five models: `luv13/kimi-k3`, `luv13/kimi-k3-fast`, `luv13/glm-5.3-flash`, `luv13/deepseek-v4.1-flash` and `luv13/qwen-3.8-27b`.
- `luv13/glm-5.3` and `luv13/deepseek-v4-pro` are listed as text in only.
- Every model returns text. luv13 doesn't generate images: `POST /v1/images/generations` returns 404.
- luv13 hasn't published the request format it accepts for images, or how image tokens are counted.

<!-- TODO: verify with a real key that luv13 accepts OpenAI-style image_url content parts, which image types and sizes it takes, and how image tokens are billed; then add an example. -->

## Which models take images

From luv13.ai/models on 2026-09-30:

| Model id | Image in | Video in |
|---|---|---|
| `luv13/kimi-k3` | Yes | Yes |
| `luv13/kimi-k3-fast` | Yes | Yes |
| `luv13/glm-5.3-flash` | Yes | Yes |
| `luv13/deepseek-v4.1-flash` | Yes | No |
| `luv13/qwen-3.8-27b` | Yes | Yes |
| `luv13/glm-5.3` | No | No |
| `luv13/deepseek-v4-pro` | No | No |

<!-- TODO: ask the operator for the request format for video input. -->

For the current list, see [the model list](/docs/models). If you fall back between models, only fall back to one that takes the same inputs; see [Model Fallback](/docs/m/model-fallback).
