---
title: DeepSeek V4.1 Flash
definition: "DeepSeek V4.1 Flash is DeepSeek's MIT-licensed multimodal Mixture-of-Experts model, available on luv13 as luv13/deepseek-v4.1-flash."
description: "DeepSeek's image-and-text V4.1 Flash on luv13: the maker's key facts, copy-paste curl, Python and JavaScript examples, and a short FAQ."
category: luv13
author: Ink
status: draft
last_checked: 2026-09-30
model_id: luv13/deepseek-v4.1-flash
maker: "DeepSeek"
open_weights: true
modalities:
  - text
  - image
context_window: "1M tokens"
released: 2026-09-10
availability: available
sources:
  - https://huggingface.co/deepseek-ai/DeepSeek-V4.1-Flash
  - https://api-docs.deepseek.com/news/news260910
  - https://api-docs.deepseek.com/updates
---

## Key takeaways

- The luv13 model id is `luv13/deepseek-v4.1-flash`, with a dot in `v4.1`.
- Made by DeepSeek. The weights are open, under the MIT license.
- DeepSeek says it reads images and text and generates text.
- DeepSeek publishes support for contexts up to 1M tokens. That's the maker's figure, not a luv13 limit.

## Overview

DeepSeek-V4.1-Flash is a multimodal Mixture-of-Experts model with 552B backbone parameters. DeepSeek describes it as the smallest model in its new architecture family, trained from scratch on a 45T-token multimodal corpus, with image understanding built in from the start of pre-training.

DeepSeek says it scores ahead of its own V4-Pro on the benchmarks in its release notes, and it has replaced DeepSeek's earlier V4-Flash models on DeepSeek's own API.

The id comes from live `GET https://api.luv13.ai/v1/models`; every other fact comes from the maker's sources below. The context window is the maker's published figure, not a luv13 limit; luv13 hasn't published its own per-model limits.

Price on luv13: see [Pricing](/docs/p/pricing).

## Examples

Set your key first: `export LUV13_API_KEY=sk-luv13-...` (see [Keys and Accounts](/docs/k/keys-and-accounts)).

curl:

```bash
curl https://api.luv13.ai/v1/chat/completions \
  -H "Authorization: Bearer $LUV13_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"model": "luv13/deepseek-v4.1-flash", "messages": [{"role": "user", "content": "Hello"}]}'
```

Python (`pip install openai`):

```python
import os
from openai import OpenAI

client = OpenAI(base_url="https://api.luv13.ai/v1", api_key=os.environ["LUV13_API_KEY"])
reply = client.chat.completions.create(
    model="luv13/deepseek-v4.1-flash",
    messages=[{"role": "user", "content": "Hello"}],
)
print(reply.choices[0].message.content)
```

JavaScript (`npm install openai`, Node.js):

```js
import OpenAI from "openai";

const client = new OpenAI({ baseURL: "https://api.luv13.ai/v1", apiKey: process.env.LUV13_API_KEY });
const reply = await client.chat.completions.create({
  model: "luv13/deepseek-v4.1-flash",
  messages: [{ role: "user", content: "Hello" }],
});
console.log(reply.choices[0].message.content);
```

Without a valid key, all three return HTTP 401 with `"type": "invalid_auth"` (checked on 2026-09-30). See [Errors and Status Codes](/docs/e/errors-and-status-codes).

## FAQ

**What model id do I use on luv13?**
`luv13/deepseek-v4.1-flash`, exactly as `GET /v1/models` lists it. See [Model IDs](/docs/m/model-ids).

**Who makes DeepSeek V4.1 Flash?**
DeepSeek.

**Are the weights open?**
Yes. DeepSeek released them under the MIT license.

**Is the context window a luv13 limit?**
No. It's the maker's published figure. luv13 hasn't published its own per-model limits; see [Limits](/docs/l/limits).

**How much does it cost on luv13?**
See [Pricing](/docs/p/pricing).

## Related

- [DeepSeek V4-Pro](/docs/m/deepseek-v4-pro)
- [the model list](https://luv13.ai/#models)
- [Model IDs](/docs/m/model-ids)

## Sources

- [DeepSeek-V4.1-Flash model card (Hugging Face)](https://huggingface.co/deepseek-ai/DeepSeek-V4.1-Flash)
- [DeepSeek-V4.1-Flash release news (DeepSeek API docs)](https://api-docs.deepseek.com/news/news260910)
- [DeepSeek API change log](https://api-docs.deepseek.com/updates)
- [luv13 model list (live GET /v1/models)](https://api.luv13.ai/v1/models) (luv13 model id)
