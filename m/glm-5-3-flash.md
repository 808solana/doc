---
title: GLM-5.3 Flash
definition: "GLM-5.3 Flash is Z.ai's natively multimodal, MIT-licensed GLM-5 model, available on luv13 as luv13/glm-5.3-flash."
description: "Z.ai's multimodal GLM-5.3-Flash on luv13: maker facts, the model used in luv13.ai's first example, and curl, Python and JS code."
category: luv13
author: Ink
status: draft
last_checked: 2026-09-30
model_id: luv13/glm-5.3-flash
maker: "Z.ai"
open_weights: true
modalities:
  - text
  - image
  - video
  - file
context_window: "1M tokens"
released: 2026-08-26
availability: available
sources:
  - https://huggingface.co/zai-org/GLM-5.3-Flash
  - https://docs.z.ai/guides/llm/glm-5.3-flash
  - https://docs.z.ai/release-notes/new-released
---

## Key takeaways

- The luv13 model id is `luv13/glm-5.3-flash`. It's the model the [Quickstart](/docs/quickstart) uses in its first example.
- Made by Z.ai. The weights are open, under the MIT license.
- Z.ai lists video, image, text and file input, with text output.
- Z.ai publishes a 1M-token context window. That's the maker's figure, not a luv13 limit.

## Overview

GLM-5.3-Flash is the first natively multimodal model in Z.ai's GLM-5 series. Unlike GLM-5.3, it starts from a newly trained base model. It has 320B total parameters with 18B active, and Z.ai says it's the first open frontier model to mix sparse and linear attention, which cuts the cost of long contexts.

Z.ai aims it at coding with visual feedback, where the model looks at interfaces and rendered output and keeps improving its work, and at office and document tasks such as research and building finished files. Z.ai's documentation lists a maximum output of 128K tokens on its own platform.

The id comes from live `GET https://api.luv13.ai/v1/models`; every other fact comes from the maker's sources below. The context window is the maker's published figure, not a luv13 limit; luv13 hasn't published its own per-model limits.

Z.ai writes the name as GLM-5.3-Flash; luv13 lists it as GLM-5.3 Flash.

Price on luv13: see [Pricing](/docs/p/pricing).

## Examples

Set your key first: `export LUV13_API_KEY=sk-luv13-...` (see [Keys and Accounts](/docs/k/keys-and-accounts)).

curl:

```bash
curl https://api.luv13.ai/v1/chat/completions \
  -H "Authorization: Bearer $LUV13_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"model": "luv13/glm-5.3-flash", "messages": [{"role": "user", "content": "Hello"}]}'
```

Python (`pip install openai`):

```python
import os
from openai import OpenAI

client = OpenAI(base_url="https://api.luv13.ai/v1", api_key=os.environ["LUV13_API_KEY"])
reply = client.chat.completions.create(
    model="luv13/glm-5.3-flash",
    messages=[{"role": "user", "content": "Hello"}],
)
print(reply.choices[0].message.content)
```

JavaScript (`npm install openai`, Node.js):

```js
import OpenAI from "openai";

const client = new OpenAI({ baseURL: "https://api.luv13.ai/v1", apiKey: process.env.LUV13_API_KEY });
const reply = await client.chat.completions.create({
  model: "luv13/glm-5.3-flash",
  messages: [{ role: "user", content: "Hello" }],
});
console.log(reply.choices[0].message.content);
```

Without a valid key, all three return HTTP 401 with `"type": "invalid_auth"` (checked on 2026-09-30). See [Errors and Status Codes](/docs/e/errors-and-status-codes).

## FAQ

**What model id do I use on luv13?**
`luv13/glm-5.3-flash`, exactly as `GET /v1/models` lists it. See [Model IDs](/docs/m/model-ids).

**Who makes GLM-5.3 Flash?**
Z.ai.

**Are the weights open?**
Yes. Z.ai released them under the MIT license.

**Is the context window a luv13 limit?**
No. It's the maker's published figure. luv13 hasn't published its own per-model limits; see [Limits](/docs/limits).

**How much does it cost on luv13?**
See [Pricing](/docs/p/pricing).

## Related

- [GLM 5.3](/docs/m/glm-5-3)
- [the model list](/docs/models)
- [Model IDs](/docs/m/model-ids)

## Sources

- [GLM-5.3-Flash model card (Hugging Face)](https://huggingface.co/zai-org/GLM-5.3-Flash)
- [GLM-5.3-Flash guide (Z.ai docs)](https://docs.z.ai/guides/llm/glm-5.3-flash)
- [Z.ai release notes](https://docs.z.ai/release-notes/new-released)
- [luv13 model list (live GET /v1/models)](https://api.luv13.ai/v1/models) (luv13 model id)
