---
title: OpenAI-Compatible APIs
definition: An OpenAI-compatible API accepts the same requests and returns the same response shapes as OpenAI's API, so existing tools and code work with it after changing the base URL and key.
category: general
author: Quill
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- OpenAI's request and response format has become a common standard that many providers copy.
- If a provider is OpenAI-compatible, you can usually point existing code at it by changing two settings: the base URL and the API key.
- luv13 is an OpenAI-compatible API at `https://api.luv13.ai/v1`.
- "Compatible" covers the core endpoints. Individual parameters and features can still differ, so check the provider's own docs.

## What "compatible" means

OpenAI's API uses a set of HTTP endpoints, such as `POST /v1/chat/completions` to get a reply from a model and `GET /v1/models` to list available models. Requests and responses are JSON with fixed field names like `model`, `messages`, `choices` and `usage`.

A compatible API uses the same paths and the same JSON shapes. That means an SDK, editor plugin or script written for OpenAI can talk to it without new code.

## What you change

Usually just two things:

1. **Base URL.** Replace OpenAI's address with the provider's. For luv13 that's `https://api.luv13.ai/v1`. See [Base URL](/docs/b/base-url).
2. **API key.** Use a key issued by the provider, sent in the `Authorization: Bearer` header.

You'll also pick a model id that the provider actually offers, taken from its `/v1/models` list.

## What can differ

Compatibility is about the shape of requests, not a promise that every option behaves the same. A provider may not support every parameter, or may offer different models. For what luv13 supports, see [Request Parameters](/docs/r/request-parameters).

## Example

This lists the models a compatible API offers. Replace `$LUV13_API_KEY` with your own key.

```bash
curl https://api.luv13.ai/v1/models \
  -H "Authorization: Bearer $LUV13_API_KEY"
```
