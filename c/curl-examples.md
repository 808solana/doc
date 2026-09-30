---
title: curl Examples
definition: curl examples are ready-to-run terminal commands for luv13's two endpoints, listing models and sending a chat completion.
description: Copy-paste curl commands to list luv13 models, send a chat message, post a long prompt from a file and test your key.
category: luv13
author: Ink
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- Export your key once with `export LUV13_API_KEY=sk-luv13-...`, then every command below runs as written.
- All chat examples use `luv13/glm-5.3-flash`, the model the [Quickstart](/docs/quickstart) uses.
- `GET /v1/models` answered without a key on 2026-09-30; chat completions always needs one.
- Add `-s -w "\nHTTP %{http_code}\n"` to any command to see the status code.

## List models

```bash
curl https://api.luv13.ai/v1/models
```

Only the ids, one per line (needs `jq`):

```bash
curl -s https://api.luv13.ai/v1/models | jq -r '.data[].id'
```

## Send a message

```bash
curl https://api.luv13.ai/v1/chat/completions \
  -H "Authorization: Bearer $LUV13_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"model": "luv13/glm-5.3-flash", "messages": [{"role": "user", "content": "ping"}]}'
```

## Send a long prompt from a file

Build the JSON with `jq` so quotes and newlines in the file are escaped correctly:

```bash
jq -n --rawfile text prompt.txt \
  '{model: "luv13/glm-5.3-flash", messages: [{role: "user", content: $text}]}' \
  | curl -s https://api.luv13.ai/v1/chat/completions \
      -H "Authorization: Bearer $LUV13_API_KEY" \
      -H "Content-Type: application/json" \
      -d @-
```

## Check your key

```bash
curl -s -o /dev/null -w "%{http_code}\n" https://api.luv13.ai/v1/chat/completions \
  -H "Authorization: Bearer $LUV13_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"model": "luv13/glm-5.3-flash", "messages": [{"role": "user", "content": "ping"}]}'
```

`401` means the key is missing or wrong. See [Errors and Status Codes](/docs/e/errors-and-status-codes).
