---
title: GLM-5.3 Flash
definition: GLM-5.3 Flash is one of the seven models luv13 serves, called with the model id luv13/glm-5.3-flash at the flat $0.33 per 1M tokens.
category: luv13
author: Ink
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- The model id is `luv13/glm-5.3-flash`. Use it exactly as written in the `model` field.
- It costs a flat $0.33 per 1M tokens, input the same as output, like every luv13 model.
- It's listed by live `GET https://api.luv13.ai/v1/models` (checked 2026-09-30).
- For its other details, see the model list at [luv13.ai/#models](https://luv13.ai/#models).

## What `/v1/models` says

The entry for this model on 2026-09-30:

```json
{"created": 1700000000, "id": "luv13/glm-5.3-flash", "object": "model", "owned_by": "luv13"}
```

| Field | Value |
|---|---|
| `id` | `luv13/glm-5.3-flash` |
| `owned_by` | `luv13` |
| `created` | `1700000000`, the same fixed value for all seven models, so it isn't the date this model was added |

Check it's still listed:

```bash
curl -s https://api.luv13.ai/v1/models | jq '.data[] | select(.id == "luv13/glm-5.3-flash")'
```

## Notes

It's the model luv13.ai/docs uses in its first example request, and the one these docs use in every example. Copy the id from the list rather than typing it; see [Model IDs](/docs/m/model-ids).

luv13 also serves [GLM 5.3](/docs/g/glm-5-3), id `luv13/glm-5.3`, at the same price. luv13 hasn't published how the two differ, so compare them on your own prompts.
<!-- TODO: ask the operator for a one-line, checkable difference between GLM-5.3 Flash and GLM 5.3. -->

## Example

```bash
curl https://api.luv13.ai/v1/chat/completions \
  -H "Authorization: Bearer $LUV13_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"model": "luv13/glm-5.3-flash", "messages": [{"role": "user", "content": "ping"}]}'
```

The team verified this request end to end on 2026-09-30.

<!-- TODO: confirm with the operator whether luv13/glm-5.3-flash supports streaming and tool calling. -->
