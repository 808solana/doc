---
title: Qwen 3.8 27B
definition: Qwen 3.8 27B is a model served by luv13 under the id luv13/qwen-3.8-27b, with text, image and video input.
category: luv13
author: Ink
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- The model id is `luv13/qwen-3.8-27b`. Use it exactly as written in the `model` field.
- Input: text, image and video. Output: text.
- Price: a flat $0.33 per 1M tokens, input the same as output, like every luv13 model.

## Details

| Field | Value |
|---|---|
| Name on luv13.ai | Qwen 3.8 27B |
| Model id | `luv13/qwen-3.8-27b` |
| Input | text, image and video |
| Output | text |
| Price | $0.33 per 1M tokens, input = output |
| Endpoint | `POST https://api.luv13.ai/v1/chat/completions` |

The id is from live `GET /v1/models` and the name and input types from luv13.ai/models, both on 2026-09-30. If they ever disagree with [Models](/docs/models), that page wins.

It accepts text, image and video input and returns text. For how to send an image, see [Image Input](/docs/i/image-input).

It's the only Qwen model luv13 lists, and the only one whose luv13 name includes a parameter count (27B).

luv13.ai doesn't publish speed or quality figures for it, so test it on your own prompts against the other six models. Price is the same on all seven.

## Using it

Put `luv13/qwen-3.8-27b` in the `model` field of a request to `POST https://api.luv13.ai/v1/chat/completions`. The full request format is on [Chat Completions](/docs/c/chat-completions).

<!-- TODO: confirm with the operator whether luv13/qwen-3.8-27b supports streaming and tool calling. -->
<!-- NOTE: per-model pages are now owned by Spec; this draft is handed over for merge or removal. -->
