---
title: Using the OpenAI SDKs
definition: The official OpenAI SDKs for Python and JavaScript can call luv13 by setting their base URL to https://api.luv13.ai/v1 and using a luv13 API key.
category: general
author: Quill
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- Install `openai` from PyPI (Python) or npm (JavaScript and TypeScript).
- Pass `base_url` in Python or `baseURL` in JavaScript, set to `https://api.luv13.ai/v1`.
- Pass your luv13 key as `api_key` or `apiKey`. Read it from an environment variable, never hard-code it.
- Use a model id from luv13's live list, such as `luv13/glm-5.3-flash`.
- The SDKs retry some failed requests and time out after 10 minutes by default. Both are configurable.

## Install

```bash
pip install openai        # Python
npm install openai        # JavaScript / TypeScript
```

## Python

```python
import os
from openai import OpenAI

client = OpenAI(
    base_url="https://api.luv13.ai/v1",
    api_key=os.environ["LUV13_API_KEY"],
)

resp = client.chat.completions.create(
    model="luv13/glm-5.3-flash",
    messages=[{"role": "user", "content": "Say hello in five words."}],
)
print(resp.choices[0].message.content)
```

The Python SDK also reads the `OPENAI_BASE_URL` environment variable if you don't pass `base_url`.

## JavaScript and TypeScript

```js
import OpenAI from "openai";

const client = new OpenAI({
  baseURL: "https://api.luv13.ai/v1",
  apiKey: process.env.LUV13_API_KEY,
});

const resp = await client.chat.completions.create({
  model: "luv13/glm-5.3-flash",
  messages: [{ role: "user", content: "Say hello in five words." }],
});
console.log(resp.choices[0].message.content);
```

## Retries and timeouts

Both SDKs retry certain errors (such as connection errors, 429 and 5xx responses) twice by default with a short backoff. Set `max_retries` (Python) or `maxRetries` (JavaScript) to change that, and `timeout` to change the 10-minute default. See [Retries and Backoff](/docs/r/retries-and-backoff) and [Timeouts](/docs/t/timeouts).

## Things to know

- The SDKs are built for OpenAI, so some methods call endpoints luv13 may not offer. Stick to chat completions and model listing unless a luv13 page says otherwise. See [Chat Completions](/docs/c/chat-completions) and [Listing Models](/docs/l/listing-models).
- For the base URL idea in general, see [Base URL](/docs/b/base-url).
