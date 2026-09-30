---
title: n8n
definition: n8n is a workflow automation tool whose OpenAI credential has a Base URL field, so its OpenAI nodes can call luv13.
description: Set up an OpenAI credential in n8n with luv13's base URL, and see which n8n OpenAI nodes will and won't work with it.
category: general
author: Quill
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- Create an **OpenAI** credential in n8n and set **Base URL** to `https://api.luv13.ai/v1`.
- Paste your luv13 key into **API Key**. Leave **Organization ID** empty.
- Use that credential in chat-based OpenAI nodes, such as the OpenAI Chat Model node, and pick a luv13 model id like `luv13/glm-5.3-flash`.
- Nodes that need endpoints luv13 doesn't serve, such as embeddings, images or audio, won't work with it.
- The field names come from n8n's own OpenAI credential definition.

## Setup

1. In n8n, go to **Credentials** and add a new **OpenAI** credential.
2. Fill in:
   - **API Key:** your luv13 key
   - **Organization ID:** leave blank
   - **Base URL:** `https://api.luv13.ai/v1`
3. Save the credential.
4. Add a chat node that uses OpenAI credentials, such as **OpenAI Chat Model** in an AI Agent or chain workflow, and select your luv13 credential.
5. Set the model to a luv13 id, such as `luv13/glm-5.3-flash`.

## What works and what doesn't

luv13 serves `GET /v1/models` and `POST /v1/chat/completions`. Features that call other OpenAI endpoints, such as the Embeddings OpenAI node or image and audio actions, return errors because luv13 doesn't serve those paths. See [Endpoints](/docs/e/endpoints).

## Check your settings first

If a workflow fails, test the same key and model outside n8n:

```bash
curl https://api.luv13.ai/v1/chat/completions \
  -H "Authorization: Bearer $LUV13_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"model": "luv13/glm-5.3-flash", "messages": [{"role": "user", "content": "Hi"}]}'
```

For errors you might see, see [Errors and Status Codes](/docs/e/errors-and-status-codes).
