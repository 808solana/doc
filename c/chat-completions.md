---
title: Chat Completions
definition: Chat completions is the luv13 endpoint that takes a list of messages and returns the model's next reply.
category: luv13
author: Ink
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- The endpoint is `POST https://api.luv13.ai/v1/chat/completions`.
- It uses the OpenAI chat completions format, so OpenAI SDKs and tools can call it once their base URL points at luv13.
- A request needs two fields: `model` (an id from `GET /v1/models`, such as `luv13/glm-5.3-flash`) and `messages` (the conversation so far).
- It needs an API key. Without a valid one it returns HTTP 401 with `"type": "invalid_auth"`.
- In the OpenAI format, the reply is in `choices[0].message.content` and the token counts you're billed for are in `usage`.

## The request

Send JSON with your API key in the `Authorization` header.

| Field | Required | What it is |
|---|---|---|
| `model` | Yes | The model id to use, exactly as `GET /v1/models` lists it. |
| `messages` | Yes | A list of messages, oldest first. Each one has a `role` (`system`, `user` or `assistant`) and `content`. |
| `stream` | No | Set to `true` to get the reply in pieces as it's generated. See [Streaming on luv13](/docs/s/streaming-on-luv13). |

Other optional settings, such as `temperature` and `max_tokens`, are covered in [Request Parameters](/docs/r/request-parameters).
<!-- TODO: confirm with the operator which optional parameters luv13 honors before this page is checked. -->

The model doesn't remember earlier requests. To continue a conversation, send the whole history again with the new message at the end. You pay for the resent history each time; see [Estimating Costs](/docs/e/estimating-costs).

## Example

This is the first request shown on luv13.ai/docs:

```bash
curl https://api.luv13.ai/v1/chat/completions \
  -H "Authorization: Bearer $LUV13_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "luv13/glm-5.3-flash",
    "messages": [
      {"role": "system", "content": "You are a helpful assistant."},
      {"role": "user", "content": "Say hello in five words."}
    ]
  }'
```

Without a valid key, the same request returns HTTP 401 and this body (checked live on 2026-09-30):

```json
{"error":{"code":401,"message":"unauthorized","type":"invalid_auth"}}
```

## The response

In the OpenAI format, the parts you'll use most are:

- `choices[0].message.content`, the model's reply.
- `choices[0].finish_reason`, why it stopped. See [Finish Reasons](/docs/f/finish-reasons).
- `usage`, with `prompt_tokens` (input), `completion_tokens` (output) and `total_tokens`. This is what you're billed for. See [Usage and Billing](/docs/u/usage-and-billing).

<!-- TODO: a successful response needs an API key, which the docs writer doesn't have. Paste a real trimmed response from the curl above so these field names are verified, not assumed. -->

If something goes wrong, see [Errors and Status Codes](/docs/e/errors-and-status-codes).
