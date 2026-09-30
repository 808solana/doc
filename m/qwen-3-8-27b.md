---
title: Qwen 3.8 27B
definition: "Qwen 3.8 27B is Alibaba's Apache-2.0 dense vision-language model from the Qwen3.8 series, available on luv13 as luv13/qwen-3.8-27b."
description: "Alibaba Qwen's 27B-parameter dense Qwen3.8-27B on luv13: maker facts, image and video input, and curl, Python and JS examples."
category: luv13
author: Ink
status: draft
last_checked: 2026-09-30
model_id: luv13/qwen-3.8-27b
maker: "Qwen (Alibaba)"
open_weights: true
modalities:
  - text
  - image
  - video
context_window: "262,144 tokens native, extensible to 1,000,000"
released: 2026-08-14
availability: available
sources:
  - https://huggingface.co/Qwen/Qwen3.8-27B
  - https://github.com/QwenLM/Qwen3.8
---

## Key takeaways

- The luv13 model id is `luv13/qwen-3.8-27b`, all lowercase.
- Made by Alibaba's Qwen team. The weights are open, under the Apache 2.0 license.
- Qwen describes it as a native vision-language model that understands images and video as well as text.
- Qwen publishes a 262,144-token native context, extensible to 1,000,000 tokens. That's the maker's figure, not a luv13 limit.

## Overview

Qwen3.8-27B is the compact member of Alibaba's Qwen3.8 open-model series. It's a dense model with 27B parameters, built on the Qwen3.5 architecture.

Qwen aims it at coding, professional work, research and long multi-step agent tasks, and describes it as easy to deploy. Its vision support covers documents, diagrams and long videos.

The id comes from live `GET https://api.luv13.ai/v1/models`; every other fact comes from the maker's sources below. The context window is the maker's published figure, not a luv13 limit; luv13 hasn't published its own per-model limits.

Qwen writes the name as Qwen3.8-27B; luv13 lists it as Qwen 3.8 27B.

Price on luv13: see [Pricing](/docs/p/pricing).

## Examples

Set your key first: `export LUV13_API_KEY=sk-luv13-...` (see [Keys and Accounts](/docs/k/keys-and-accounts)).

curl:

```bash
curl https://api.luv13.ai/v1/chat/completions \
  -H "Authorization: Bearer $LUV13_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"model": "luv13/qwen-3.8-27b", "messages": [{"role": "user", "content": "Hello"}]}'
```

Python (`pip install openai`):

```python
import os
from openai import OpenAI

client = OpenAI(base_url="https://api.luv13.ai/v1", api_key=os.environ["LUV13_API_KEY"])
reply = client.chat.completions.create(
    model="luv13/qwen-3.8-27b",
    messages=[{"role": "user", "content": "Hello"}],
)
print(reply.choices[0].message.content)
```

JavaScript (`npm install openai`, Node.js):

```js
import OpenAI from "openai";

const client = new OpenAI({ baseURL: "https://api.luv13.ai/v1", apiKey: process.env.LUV13_API_KEY });
const reply = await client.chat.completions.create({
  model: "luv13/qwen-3.8-27b",
  messages: [{ role: "user", content: "Hello" }],
});
console.log(reply.choices[0].message.content);
```

Without a valid key, all three return HTTP 401 with `"type": "invalid_auth"` (checked on 2026-09-30). See [Errors and Status Codes](/docs/e/errors-and-status-codes).

## FAQ

**What model id do I use on luv13?**
`luv13/qwen-3.8-27b`, exactly as `GET /v1/models` lists it. See [Model IDs](/docs/m/model-ids).

**Who makes Qwen 3.8 27B?**
Qwen (Alibaba).

**Are the weights open?**
Yes. Qwen released them under the Apache 2.0 license.

**Is the context window a luv13 limit?**
No. It's the maker's published figure. luv13 hasn't published its own per-model limits; see [Limits](/docs/l/limits).

**How much does it cost on luv13?**
See [Pricing](/docs/p/pricing).

## Related

- [the model list](https://luv13.ai/#models)
- [Model IDs](/docs/m/model-ids)

## Sources

- [Qwen3.8-27B model card (Hugging Face)](https://huggingface.co/Qwen/Qwen3.8-27B)
- [Qwen3.8 repository (QwenLM on GitHub)](https://github.com/QwenLM/Qwen3.8)
- [luv13 model list (live GET /v1/models)](https://api.luv13.ai/v1/models) (luv13 model id)
