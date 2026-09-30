---
title: GLM 5.3
definition: GLM 5.3 is one of the seven models luv13 serves, called with the model id luv13/glm-5.3 at the flat $0.33 per 1M tokens.
category: luv13
author: Ink
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- The model id is `luv13/glm-5.3`. Use it exactly as written in the `model` field.
- It costs a flat $0.33 per 1M tokens, input the same as output, like every luv13 model.
- It's listed by live `GET https://api.luv13.ai/v1/models` (checked 2026-09-30).
- For its other details, see the model list at [luv13.ai/#models](https://luv13.ai/#models).

## What `/v1/models` says

The entry for this model on 2026-09-30:

```json
{"created": 1700000000, "id": "luv13/glm-5.3", "object": "model", "owned_by": "luv13"}
```

| Field | Value |
|---|---|
| `id` | `luv13/glm-5.3` |
| `owned_by` | `luv13` |
| `created` | `1700000000`, the same fixed value for all seven models, so it isn't the date this model was added |

Check it's still listed:

```bash
curl -s https://api.luv13.ai/v1/models | jq '.data[] | select(.id == "luv13/glm-5.3")'
```

## Notes

The display names differ in punctuation: "GLM 5.3" has a space and "GLM-5.3 Flash" has a hyphen. Both ids use a hyphen after `glm` and a dot in the version: `luv13/glm-5.3` and `luv13/glm-5.3-flash`. Copy the id from the list rather than typing it; see [Model IDs](/docs/m/model-ids).

luv13 also serves [GLM-5.3 Flash](/docs/g/glm-5-3-flash), id `luv13/glm-5.3-flash`, at the same price. luv13 hasn't published how the two differ, so compare them on your own prompts.
<!-- TODO: ask the operator for a one-line, checkable difference between GLM 5.3 and GLM-5.3 Flash. -->

## Using it

Put `luv13/glm-5.3` in the `model` field of a `POST https://api.luv13.ai/v1/chat/completions` request. The request format, with a working example, is on [Chat Completions](/docs/c/chat-completions).

<!-- TODO: add a curl with luv13/glm-5.3 once a request with this id has been confirmed to work with a real key. -->

<!-- TODO: confirm with the operator whether luv13/glm-5.3 supports streaming and tool calling. -->
