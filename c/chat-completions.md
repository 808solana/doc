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
- The request luv13.ai/docs shows has two fields: `model` (an id from `GET /v1/models`, such as `luv13/glm-5.3-flash`) and `messages`.
- It needs an API key. Without a valid one it returns HTTP 401 with `"type": "invalid_auth"`.
- Every model costs $0.33 per 1M tokens, input the same as output.

## The request

Send JSON with your API key in the `Authorization: Bearer` header.

| Field | What it is |
|---|---|
| `model` | The model id, exactly as `GET /v1/models` lists it. See [Model IDs](/docs/m/model-ids). |
| `messages` | The conversation, as a list of messages with a `role` and `content`. |

Optional fields are covered on [Request Parameters](/docs/r/request-parameters).

## Example

This is the first request shown on luv13.ai/docs:

```bash
curl https://api.luv13.ai/v1/chat/completions \
  -H "Authorization: Bearer $LUV13_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"model": "luv13/glm-5.3-flash", "messages": [{"role": "user", "content": "ping"}]}'
```

Without a valid key, the same request returns HTTP 401 and this body (checked live on 2026-09-30):

```json
{"error":{"code":401,"message":"unauthorized","type":"invalid_auth"}}
```

## The response

A successful reply comes back in the OpenAI chat completions format. For how that format is laid out, see [Using the OpenAI SDKs](/docs/u/using-the-openai-sdks).

<!-- TODO: paste a real trimmed authenticated response here so the response fields are verified. -->

To keep a conversation going, see [Conversation History](/docs/c/conversation-history). If something goes wrong, see [Errors and Status Codes](/docs/e/errors-and-status-codes).
