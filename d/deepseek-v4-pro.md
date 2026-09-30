---
title: DeepSeek V4-Pro
definition: DeepSeek V4-Pro is a model served by luv13 under the id luv13/deepseek-v4-pro, with text input.
category: luv13
author: Ink
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- The model id is `luv13/deepseek-v4-pro`. Use it exactly as written in the `model` field.
- Input: text. Output: text.
- Price: a flat $0.33 per 1M tokens, input the same as output, like every luv13 model.

## Details

| Field | Value |
|---|---|
| Name on luv13.ai | DeepSeek V4-Pro |
| Model id | `luv13/deepseek-v4-pro` |
| Input | text |
| Output | text |
| Price | $0.33 per 1M tokens, input = output |
| Endpoint | `POST https://api.luv13.ai/v1/chat/completions` |

The id is from live `GET /v1/models` and the name and input types from luv13.ai/models, both on 2026-09-30. If they ever disagree with [Models](/docs/models), that page wins.

It accepts text only and returns text. To send images, pick a model that lists image input; see [Models](/docs/models).

The version numbers differ between the two DeepSeek models: this one is V4, the Flash model is V4.1. The ids are `luv13/deepseek-v4-pro` and `luv13/deepseek-v4.1-flash`.

luv13 also serves [DeepSeek V4.1 Flash](/docs/d/deepseek-v4-1-flash) (`luv13/deepseek-v4.1-flash`). luv13.ai doesn't publish how the two differ in speed or quality, so test both on your own prompts. Price is the same, so cost isn't a reason to pick one over the other.
<!-- TODO: ask the operator for a one-line, checkable difference between DeepSeek V4-Pro and DeepSeek V4.1 Flash. -->

## Using it

Put `luv13/deepseek-v4-pro` in the `model` field of a request to `POST https://api.luv13.ai/v1/chat/completions`. The full request format is on [Chat Completions](/docs/c/chat-completions).

<!-- TODO: confirm with the operator whether luv13/deepseek-v4-pro supports streaming and tool calling. -->
<!-- TODO: the team reported on 2026-09-30 that requests to luv13/deepseek-v4-pro fail upstream. Confirm it works before this page is checked. -->
<!-- NOTE: per-model pages are now owned by Spec; this draft is handed over for merge or removal. -->
