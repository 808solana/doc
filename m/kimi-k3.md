---
title: Kimi K3
definition: "Kimi K3 is Moonshot AI's open-weight multimodal model for long coding and agent work, available on luv13 as luv13/kimi-k3."
description: "Moonshot AI's 2.8T-parameter open-weight Kimi K3 on luv13: key facts from the maker, copy-paste examples, and a short FAQ."
category: luv13
author: Ink
status: draft
last_checked: 2026-09-30
model_id: luv13/kimi-k3
maker: "Moonshot AI"
open_weights: true
modalities:
  - text
  - image
  - video
context_window: "1,048,576 tokens"
availability: available
sources:
  - https://huggingface.co/moonshotai/Kimi-K3
  - https://www.kimi.com/blog/kimi-k3
  - https://platform.kimi.ai/docs/models
---

## Key takeaways

- The luv13 model id is `luv13/kimi-k3`.
- Made by Moonshot AI. The weights are open, under the Kimi K3 License.
- Moonshot describes it as a native multimodal model that reads text, images and video and replies in text.
- Moonshot publishes a 1,048,576-token context window. That's the maker's figure, not a luv13 limit.

## Overview

Kimi K3 is from Moonshot AI, which calls it its most capable model to date. It is a Mixture-of-Experts model with 2.8T total parameters, of which 104B are active for each token, and it's built on Moonshot's own Kimi Delta Attention and Attention Residuals designs.

Moonshot aims it at long, mostly unattended work: extended coding sessions across large repositories, driving terminal tools, and agent-style knowledge work such as research write-ups. Because vision is built in, it can also work from images, rendered output and video.

The id comes from live `GET https://api.luv13.ai/v1/models`; every other fact comes from the maker's sources below. The context window is the maker's published figure, not a luv13 limit; luv13 hasn't published its own per-model limits.

Moonshot's model card describes text, image and video understanding in its introduction; the card's summary table lists text and image.

Price on luv13: see [Pricing](/docs/p/pricing).

## Examples

Set your key first: `export LUV13_API_KEY=sk-luv13-...` (see [Keys and Accounts](/docs/k/keys-and-accounts)).

curl:

```bash
curl https://api.luv13.ai/v1/chat/completions \
  -H "Authorization: Bearer $LUV13_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"model": "luv13/kimi-k3", "messages": [{"role": "user", "content": "Hello"}]}'
```

Python (`pip install openai`):

```python
import os
from openai import OpenAI

client = OpenAI(base_url="https://api.luv13.ai/v1", api_key=os.environ["LUV13_API_KEY"])
reply = client.chat.completions.create(
    model="luv13/kimi-k3",
    messages=[{"role": "user", "content": "Hello"}],
)
print(reply.choices[0].message.content)
```

JavaScript (`npm install openai`, Node.js):

```js
import OpenAI from "openai";

const client = new OpenAI({ baseURL: "https://api.luv13.ai/v1", apiKey: process.env.LUV13_API_KEY });
const reply = await client.chat.completions.create({
  model: "luv13/kimi-k3",
  messages: [{ role: "user", content: "Hello" }],
});
console.log(reply.choices[0].message.content);
```

Without a valid key, all three return HTTP 401 with `"type": "invalid_auth"` (checked on 2026-09-30). See [Errors and Status Codes](/docs/e/errors-and-status-codes).

## FAQ

**What model id do I use on luv13?**
`luv13/kimi-k3`, exactly as `GET /v1/models` lists it. See [Model IDs](/docs/m/model-ids).

**Who makes Kimi K3?**
Moonshot AI.

**Are the weights open?**
Yes. Moonshot released them under the Kimi K3 License.

**Is the context window a luv13 limit?**
No. It's the maker's published figure. luv13 hasn't published its own per-model limits; see [Limits](/docs/limits).

**How much does it cost on luv13?**
See [Pricing](/docs/p/pricing).

## Related

- [Kimi K3 Fast](/docs/m/kimi-k3-fast)
- [the model list](/docs/models)
- [Model IDs](/docs/m/model-ids)

## Sources

- [Kimi K3 model card (Hugging Face)](https://huggingface.co/moonshotai/Kimi-K3)
- [Kimi K3 tech blog (Moonshot AI)](https://www.kimi.com/blog/kimi-k3)
- [Kimi API model list (Moonshot AI)](https://platform.kimi.ai/docs/models)
- [luv13 model list (live GET /v1/models)](https://api.luv13.ai/v1/models) (luv13 model id)
