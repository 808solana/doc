---
title: Open-Weight Models
definition: An open-weight model is a language model whose trained weights are published, so anyone allowed by its license can download and run it.
category: general
author: Quill
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- "Weights" are the learned numbers that make a model work. Publishing them lets others run the model on their own hardware.
- Open weights aren't always "open source". The training data and code may stay private, and the license may set rules.
- Hosted APIs like luv13 let you use such models without running the hardware yourself.
- Each model has its own license. Read it before you build on a model.

## Open weights vs. closed models

| | Open-weight | Closed |
|---|---|---|
| Can you download it? | Yes | No, API only |
| Can you run it yourself? | Yes, with enough hardware | No |
| Can you fine-tune it? | Often, depending on the license | Only if the vendor offers it |
| Who can host it? | Anyone the license allows | Only the vendor |

## Why it matters

- **Choice of host.** The same model can be served by many providers, so you can compare price and speed.
- **Control.** You can run it privately if you need to.
- **Customizing.** You can fine-tune or [quantize](/docs/q/quantization) it.

The catch is that running a large model yourself takes a lot of GPU memory and work. A hosted API handles that for you.

## On luv13

luv13 serves models from the DeepSeek, GLM, Kimi and Qwen families. Their makers have published open weights for many releases, but check each vendor's own release notes and license for the exact version you use. luv13's current models are listed at [luv13.ai](https://luv13.ai/#models), and the API list is covered on [Listing Models](/docs/l/listing-models).

## Example

```bash
curl https://api.luv13.ai/v1/models
```
