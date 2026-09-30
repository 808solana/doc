---
title: DeepSeek V4-Pro
definition: "DeepSeek V4-Pro is DeepSeek's large MIT-licensed Mixture-of-Experts text model, listed on luv13 as luv13/deepseek-v4-pro and temporarily unavailable."
description: "DeepSeek's 1.6T-parameter V4-Pro, temporarily unavailable on luv13: the maker's key facts, the luv13 id, and what to use meanwhile."
category: luv13
author: Ink
status: draft
last_checked: 2026-09-30
model_id: luv13/deepseek-v4-pro
maker: "DeepSeek"
open_weights: true
modalities:
  - text
context_window: "1M tokens"
released: 2026-04-24
availability: unavailable
sources:
  - https://huggingface.co/deepseek-ai/DeepSeek-V4-Pro
  - https://api-docs.deepseek.com/updates
  - https://api-docs.deepseek.com/quick_start/pricing
---

## Key takeaways

- **Temporarily unavailable on luv13.** Requests to this id don't work right now; use another model id meanwhile.
- The luv13 model id is `luv13/deepseek-v4-pro`, with no dot in `v4`.
- Made by DeepSeek. The weights are open, under the MIT license.
- DeepSeek lists a 1M-token context length and no vision support. The context figure is the maker's, not a luv13 limit.

## Overview

DeepSeek-V4-Pro is the larger model in DeepSeek's V4 series: a Mixture-of-Experts language model with 1.6T total parameters and 49B active. DeepSeek first released it as a preview on 2026-04-24 and rolled out its general-availability version on 2026-08-13.

DeepSeek designed the V4 series around efficient million-token contexts, using a hybrid attention scheme to cut the cost of long inputs. DeepSeek's API docs list vision as not supported for V4-Pro.

The id comes from live `GET https://api.luv13.ai/v1/models`; every other fact comes from the maker's sources below. The context window is the maker's published figure, not a luv13 limit; luv13 hasn't published its own per-model limits.

Price on luv13: see [Pricing](/docs/p/pricing).

## Examples

This model is temporarily unavailable on luv13, so there are no runnable examples here. When it's back, it uses the same request as any other model, with `"model": "luv13/deepseek-v4-pro"`; see [Chat Completions](/docs/c/chat-completions). Until then, use another id from [the model list](https://luv13.ai/#models), such as [DeepSeek V4.1 Flash](/docs/m/deepseek-v4-1-flash).

<!-- TODO: add curl, Python and JS examples once the operator confirms luv13/deepseek-v4-pro is available again. -->

## FAQ

**What model id do I use on luv13?**
`luv13/deepseek-v4-pro`, exactly as `GET /v1/models` lists it. See [Model IDs](/docs/m/model-ids).

**Who makes DeepSeek V4-Pro?**
DeepSeek.

**Are the weights open?**
Yes. DeepSeek released them under the MIT license.

**Is the context window a luv13 limit?**
No. It's the maker's published figure. luv13 hasn't published its own per-model limits; see [Limits](/docs/limits).

**How much does it cost on luv13?**
See [Pricing](/docs/p/pricing).

## Related

- [DeepSeek V4.1 Flash](/docs/m/deepseek-v4-1-flash)
- [the model list](https://luv13.ai/#models)
- [Model IDs](/docs/m/model-ids)

## Sources

- [DeepSeek-V4-Pro model card (Hugging Face)](https://huggingface.co/deepseek-ai/DeepSeek-V4-Pro)
- [DeepSeek API change log](https://api-docs.deepseek.com/updates)
- [DeepSeek models and pricing (DeepSeek API docs)](https://api-docs.deepseek.com/quick_start/pricing)
- [luv13 model list (live GET /v1/models)](https://api.luv13.ai/v1/models) (luv13 model id)
