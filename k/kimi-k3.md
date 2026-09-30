---
title: Kimi K3
definition: Kimi K3 is a model served by luv13 under the id luv13/kimi-k3, with text, image and video input.
category: luv13
author: Ink
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- The model id is `luv13/kimi-k3`. Use it exactly as written in the `model` field.
- Input: text, image and video. Output: text.
- Price: a flat $0.33 per 1M tokens, input the same as output, like every luv13 model.

## Details

| Field | Value |
|---|---|
| Name on luv13.ai | Kimi K3 |
| Model id | `luv13/kimi-k3` |
| Input | text, image and video |
| Output | text |
| Price | $0.33 per 1M tokens, input = output |
| Endpoint | `POST https://api.luv13.ai/v1/chat/completions` |

The id is from live `GET /v1/models` and the name and input types from luv13.ai/models, both on 2026-09-30. If they ever disagree with [Models](/docs/models), that page wins.

It accepts text, image and video input and returns text. For how to send an image, see [Image Input](/docs/i/image-input).

luv13 also serves [Kimi K3 Fast](/docs/k/kimi-k3-fast) (`luv13/kimi-k3-fast`). luv13.ai doesn't publish how the two differ in speed or quality, so test both on your own prompts. Price is the same, so cost isn't a reason to pick one over the other.
<!-- TODO: ask the operator for a one-line, checkable difference between Kimi K3 and Kimi K3 Fast. -->

## Using it

Put `luv13/kimi-k3` in the `model` field of a request to `POST https://api.luv13.ai/v1/chat/completions`. The full request format is on [Chat Completions](/docs/c/chat-completions).

<!-- TODO: confirm with the operator whether luv13/kimi-k3 supports streaming and tool calling. -->
<!-- NOTE: per-model pages are now owned by Spec; this draft is handed over for merge or removal. -->
