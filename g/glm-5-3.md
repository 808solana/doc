---
title: GLM 5.3
definition: GLM 5.3 is a model served by luv13 under the id luv13/glm-5.3, with text input.
category: luv13
author: Ink
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- The model id is `luv13/glm-5.3`. Use it exactly as written in the `model` field.
- Input: text. Output: text.
- Price: a flat $0.33 per 1M tokens, input the same as output, like every luv13 model.

## Details

| Field | Value |
|---|---|
| Name on luv13.ai | GLM 5.3 |
| Model id | `luv13/glm-5.3` |
| Input | text |
| Output | text |
| Price | $0.33 per 1M tokens, input = output |
| Endpoint | `POST https://api.luv13.ai/v1/chat/completions` |

The id is from live `GET /v1/models` and the name and input types from luv13.ai/models, both on 2026-09-30. If they ever disagree with [Models](/docs/models), that page wins.

It accepts text only and returns text. To send images, pick a model that lists image input; see [Models](/docs/models).

Note the display names differ in punctuation: this one is "GLM 5.3" with a space, its sibling is "GLM-5.3 Flash" with a hyphen. The ids are `luv13/glm-5.3` and `luv13/glm-5.3-flash`.

luv13 also serves [GLM-5.3 Flash](/docs/g/glm-5-3-flash) (`luv13/glm-5.3-flash`). luv13.ai doesn't publish how the two differ in speed or quality, so test both on your own prompts. Price is the same, so cost isn't a reason to pick one over the other.
<!-- TODO: ask the operator for a one-line, checkable difference between GLM 5.3 and GLM-5.3 Flash. -->

## Using it

Put `luv13/glm-5.3` in the `model` field of a request to `POST https://api.luv13.ai/v1/chat/completions`. The full request format is on [Chat Completions](/docs/c/chat-completions).

<!-- TODO: confirm with the operator whether luv13/glm-5.3 supports streaming and tool calling. -->
<!-- NOTE: per-model pages are now owned by Spec; this draft is handed over for merge or removal. -->
