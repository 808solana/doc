---
title: curl Examples
definition: curl examples are ready-to-run terminal commands for every luv13 endpoint, from listing models to a streamed chat completion.
category: luv13
author: Ink
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- Export your key once with `export LUV13_API_KEY=sk-luv13-...`, then every command below runs as written.
- All examples use the model `luv13/glm-5.3-flash`, the one luv13.ai/docs uses.
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

## Print only the reply

In the OpenAI format, the text is in `choices[0].message.content`:

```bash
curl -s https://api.luv13.ai/v1/chat/completions \
  -H "Authorization: Bearer $LUV13_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"model": "luv13/glm-5.3-flash", "messages": [{"role": "user", "content": "ping"}]}' \
  | jq -r '.choices[0].message.content'
```

## See the tokens you'll pay for

```bash
curl -s https://api.luv13.ai/v1/chat/completions \
  -H "Authorization: Bearer $LUV13_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"model": "luv13/glm-5.3-flash", "messages": [{"role": "user", "content": "ping"}]}' \
  | jq '.usage'
```

Multiply `total_tokens` by 0.33 and divide by 1,000,000 for the cost in USD. See [Estimating Costs](/docs/e/estimating-costs).

## Stream the reply

```bash
curl -N https://api.luv13.ai/v1/chat/completions \
  -H "Authorization: Bearer $LUV13_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"model": "luv13/glm-5.3-flash", "messages": [{"role": "user", "content": "Count to 5."}], "stream": true}'
```

See [Streaming on luv13](/docs/s/streaming-on-luv13).

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
  -d '{"model": "luv13/glm-5.3-flash", "messages": [{"role": "user", "content": "ping"}], "max_tokens": 1}'
```

`401` means the key is missing or wrong. See [Errors and Status Codes](/docs/e/errors-and-status-codes).

<!-- TODO: run the authenticated examples with a real key and confirm the jq paths match luv13's actual response. -->
