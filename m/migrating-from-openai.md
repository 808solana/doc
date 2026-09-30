---
title: Migrating from OpenAI
definition: Migrating from OpenAI means moving code that calls the OpenAI API over to luv13 by changing the base URL, the API key and the model id.
category: luv13
author: Ink
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- Change three things: base URL to `https://api.luv13.ai/v1`, API key to your `sk-luv13-` key, and `model` to a luv13 id such as `luv13/glm-5.3-flash`.
- luv13 serves two endpoints: `GET /v1/models` and `POST /v1/chat/completions`. Code that only uses chat completions moves over most easily.
- Endpoints such as `/v1/embeddings`, `/v1/completions` and `/v1/responses` return 404 on luv13. Keep those calls where they are or remove them.
- The official OpenAI Python and Node.js SDKs work unchanged apart from the client settings (checked on 2026-09-30).

## The three changes

| Setting | OpenAI | luv13 |
|---|---|---|
| Base URL | `https://api.openai.com/v1` | `https://api.luv13.ai/v1` |
| API key | `sk-...` from OpenAI | `sk-luv13-...` from the luv13 dashboard |
| Model | An OpenAI model name | One of the seven ids from `GET /v1/models` |

## Python

```python
import os
from openai import OpenAI

client = OpenAI(
    base_url="https://api.luv13.ai/v1",
    api_key=os.environ["LUV13_API_KEY"],
)
reply = client.chat.completions.create(
    model="luv13/glm-5.3-flash",
    messages=[{"role": "user", "content": "ping"}],
)
print(reply.choices[0].message.content)
```

If your code builds the client with no arguments, you can set `OPENAI_BASE_URL=https://api.luv13.ai/v1` and `OPENAI_API_KEY` to your luv13 key instead. Both SDKs read those variables. See [Environment Variables](/docs/e/environment-variables).

## Checklist

1. Find every place a model name is hard-coded and replace it with a luv13 id. See [Model IDs](/docs/m/model-ids).
2. Remove or reroute calls to endpoints luv13 doesn't serve. See [Endpoints](/docs/e/endpoints).
3. If you send images, pick a model that accepts them. See [Models](/docs/models).
4. Test the optional features you depend on (streaming, tools, JSON output). luv13 hasn't published per-model support yet. See [Request Parameters](/docs/r/request-parameters).
5. Update cost math: one rate, $0.33 per 1M tokens, input the same as output. See [Estimating Costs](/docs/e/estimating-costs).

<!-- TODO: confirm with the operator which OpenAI request fields luv13 ignores or rejects, so this checklist can name them. -->
