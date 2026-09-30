---
title: Kimi K3 Fast
definition: "Kimi K3 Fast is a model id luv13 lists as luv13/kimi-k3-fast, named after Moonshot AI's Kimi K3."
description: "What is and isn't documented about luv13/kimi-k3-fast, with copy-paste examples and a link to the Kimi K3 page."
category: luv13
author: Ink
status: draft
last_checked: 2026-09-30
model_id: luv13/kimi-k3-fast
availability: available
sources:
  - https://platform.kimi.ai/docs/models
---

## Key takeaways

- The luv13 model id is `luv13/kimi-k3-fast`.
- Moonshot AI's official model list doesn't include a model named Kimi K3 Fast (checked 2026-09-30), so this page gives no maker specs.
- luv13 hasn't published how it differs from `luv13/kimi-k3`.
- For the maker's facts about Kimi K3 itself, see [Kimi K3](/docs/m/kimi-k3).

## Overview

luv13 lists this model as "Kimi K3 Fast". The name points to [Kimi K3](/docs/m/kimi-k3) from Moonshot AI, but Moonshot's official model list on its API platform shows `kimi-k3` and no separate "fast" model, and there's no maker model card for it. So this page doesn't state a maker, context window, modalities or release date for it.

<!-- TODO: ask the operator what luv13/kimi-k3-fast is and how it differs from luv13/kimi-k3, and whether an official Moonshot source describes it. -->

If you're choosing between the two, send the same prompts to both ids and compare.

Price on luv13: see [Pricing](/docs/p/pricing).

## Examples

Set your key first: `export LUV13_API_KEY=sk-luv13-...` (see [Keys and Accounts](/docs/k/keys-and-accounts)).

curl:

```bash
curl https://api.luv13.ai/v1/chat/completions \
  -H "Authorization: Bearer $LUV13_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"model": "luv13/kimi-k3-fast", "messages": [{"role": "user", "content": "Hello"}]}'
```

Python (`pip install openai`):

```python
import os
from openai import OpenAI

client = OpenAI(base_url="https://api.luv13.ai/v1", api_key=os.environ["LUV13_API_KEY"])
reply = client.chat.completions.create(
    model="luv13/kimi-k3-fast",
    messages=[{"role": "user", "content": "Hello"}],
)
print(reply.choices[0].message.content)
```

JavaScript (`npm install openai`, Node.js):

```js
import OpenAI from "openai";

const client = new OpenAI({ baseURL: "https://api.luv13.ai/v1", apiKey: process.env.LUV13_API_KEY });
const reply = await client.chat.completions.create({
  model: "luv13/kimi-k3-fast",
  messages: [{ role: "user", content: "Hello" }],
});
console.log(reply.choices[0].message.content);
```

Without a valid key, all three return HTTP 401 with `"type": "invalid_auth"` (checked on 2026-09-30). See [Errors and Status Codes](/docs/e/errors-and-status-codes).

## FAQ

**What model id do I use on luv13?**
`luv13/kimi-k3-fast`, exactly as `GET /v1/models` lists it. See [Model IDs](/docs/m/model-ids).

**How is it different from Kimi K3?**
luv13 hasn't published that yet, and Moonshot AI's official model list has no model by this name.

**How much does it cost on luv13?**
See [Pricing](/docs/p/pricing).

## Related

- [Kimi K3](/docs/m/kimi-k3)
- [the model list](https://luv13.ai/#models)
- [Model IDs](/docs/m/model-ids)

## Sources

- [Kimi API model list (Moonshot AI)](https://platform.kimi.ai/docs/models) (Moonshot AI's model list; checked 2026-09-30, no Kimi K3 Fast entry)
- [luv13 model list (live GET /v1/models)](https://api.luv13.ai/v1/models) (luv13 model id)
