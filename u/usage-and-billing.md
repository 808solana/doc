---
title: Usage and Billing
definition: Usage and billing is how luv13 charges the tokens your requests use against your prepaid credit, at a flat $0.33 per 1M tokens.
category: luv13
author: Ink
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- You pay per token: a flat $0.33 per 1M tokens on every model, and input tokens cost the same as output tokens.
- Billing is prepaid credit in USD. You top up by card in the dashboard, through Stripe, from $5, and usage draws down the balance.
- There's no subscription.
- Failed or empty calls aren't charged.
- Your balance and recent usage are in the [dashboard](https://luv13.ai/dashboard).

All of the above is from luv13.ai/docs and luv13.ai/pricing, checked on 2026-09-30.

## How a request is charged

Because input and output cost the same, only the total matters:

```
cost in USD = total tokens / 1,000,000 × 0.33
```

Worked example: 800,000 input tokens plus 200,000 output tokens is 1,000,000 tokens, so the cost is $0.33.

For more worked numbers, see [Estimating Costs](/docs/e/estimating-costs). For input and output tokens in general, see [Input vs. Output Tokens](/docs/i/input-vs-output-tokens).

<!-- TODO: confirm with the operator which response fields billing uses, and what happens when the balance reaches zero. -->

## What isn't charged

luv13.ai/docs says failed or empty calls aren't charged, so a request that errors costs nothing.

The rate is set by the operator; [Pricing](/docs/p/pricing) is the source of truth.
