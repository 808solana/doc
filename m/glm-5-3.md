---
title: GLM 5.3
definition: "GLM 5.3 is Z.ai's flagship open-weight coding and agent model, available on luv13 as luv13/glm-5.3."
description: "Z.ai's text-only GLM-5.3 on luv13: the maker's key facts, copy-paste examples in curl, Python and JavaScript, and a short FAQ."
category: luv13
author: Ink
status: draft
last_checked: 2026-09-30
model_id: luv13/glm-5.3
maker: "Z.ai"
open_weights: true
modalities:
  - text
context_window: "1M tokens"
released: 2026-08-18
availability: available
sources:
  - https://huggingface.co/zai-org/GLM-5.3
  - https://docs.z.ai/guides/llm/glm-5.3
  - https://docs.z.ai/release-notes/new-released
---

## Key takeaways

- The luv13 model id is `luv13/glm-5.3`.
- Made by Z.ai. The weights are open, under Z.ai's own glm-5.3 license.
- Z.ai says it takes text input only.
- Z.ai publishes a 1M-token context window. That's the maker's figure, not a luv13 limit.

## Overview

GLM-5.3 is Z.ai's flagship model in the GLM-5 series. Z.ai built it on the same base model as GLM-5.2 and says every improvement came from post-training.

Z.ai positions it for complex software engineering and long-horizon agent tasks. It reports a 50% gain over GLM-5.2 on its in-house Z.ai Code Bench, and strong results on security work such as vulnerability discovery. Z.ai's documentation lists a maximum output of 128K tokens on its own platform.

The id comes from live `GET https://api.luv13.ai/v1/models`; every other fact comes from the maker's sources below. The context window is the maker's published figure, not a luv13 limit; luv13 hasn't published its own per-model limits.

Z.ai writes the name as GLM-5.3; luv13 lists it as GLM 5.3.

Price on luv13: see [Pricing](/docs/p/pricing).

## Examples

Set your key first: `export LUV13_API_KEY=sk-luv13-...` (see [Keys and Accounts](/docs/k/keys-and-accounts)).

curl:

```bash
curl https://api.luv13.ai/v1/chat/completions \
  -H "Authorization: Bearer $LUV13_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"model": "luv13/glm-5.3", "messages": [{"role": "user", "content": "Hello"}]}'
```

Python (`pip install openai`):

```python
import os
from openai import OpenAI

client = OpenAI(base_url="https://api.luv13.ai/v1", api_key=os.environ["LUV13_API_KEY"])
reply = client.chat.completions.create(
    model="luv13/glm-5.3",
    messages=[{"role": "user", "content": "Hello"}],
)
print(reply.choices[0].message.content)
```

JavaScript (`npm install openai`, Node.js):

```js
import OpenAI from "openai";

const client = new OpenAI({ baseURL: "https://api.luv13.ai/v1", apiKey: process.env.LUV13_API_KEY });
const reply = await client.chat.completions.create({
  model: "luv13/glm-5.3",
  messages: [{ role: "user", content: "Hello" }],
});
console.log(reply.choices[0].message.content);
```

Without a valid key, all three return HTTP 401 with `"type": "invalid_auth"` (checked on 2026-09-30). See [Errors and Status Codes](/docs/e/errors-and-status-codes).

## FAQ

**What model id do I use on luv13?**
`luv13/glm-5.3`, exactly as `GET /v1/models` lists it. See [Model IDs](/docs/m/model-ids).

**Who makes GLM 5.3?**
Z.ai.

**Are the weights open?**
Yes. Z.ai released them under its custom glm-5.3 license; read it before commercial use.

**Is the context window a luv13 limit?**
No. It's the maker's published figure. luv13 hasn't published its own per-model limits; see [Limits](/docs/l/limits).

**How much does it cost on luv13?**
See [Pricing](/docs/p/pricing).

## Related

- [GLM-5.3 Flash](/docs/m/glm-5-3-flash)
- [the model list](https://luv13.ai/#models)
- [Model IDs](/docs/m/model-ids)

## Sources

- [GLM-5.3 model card (Hugging Face)](https://huggingface.co/zai-org/GLM-5.3)
- [GLM-5.3 guide (Z.ai docs)](https://docs.z.ai/guides/llm/glm-5.3)
- [Z.ai release notes](https://docs.z.ai/release-notes/new-released)
- [luv13 model list (live GET /v1/models)](https://api.luv13.ai/v1/models) (luv13 model id)
