---
title: Usage and Billing
definition: Usage and billing is how luv13 charges the tokens your requests use against your prepaid credit, at a flat $0.33 per 1M tokens.
description: "How luv13 bills: one rate for input and output, and prepaid USD credit you top up in the dashboard from $5."
category: luv13
author: Ink
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- You pay per token: a flat $0.33 per 1M tokens on every model, and input tokens cost the same as output tokens.
- Billing is prepaid credit in USD. You top up any amount from $5 in the dashboard (confirmed by the luv13 operator team, 2026-10-02). Usage draws down the balance.
- Your balance is in the [dashboard](https://dash.luv13.ai) (confirmed by the luv13 operator team on 2026-09-17). <!-- TODO: confirm with the operator whether recent usage is shown in the dashboard. -->

The price is from live [models.luv13.ai](https://models.luv13.ai), checked on 2026-10-02. The $5 minimum top-up was confirmed by the luv13 operator team on 2026-10-02.

<!-- TODO: confirm with the operator the payment method, and whether there's any subscription. -->

## How a request is charged

Because input and output cost the same, only the total matters:

```
cost in USD = total tokens / 1,000,000 × 0.33
```

Worked example: 800,000 input tokens plus 200,000 output tokens is 1,000,000 tokens, so the cost is $0.33.

For more worked numbers, see [Estimating Costs](/docs/e/estimating-costs). For input and output tokens in general, see [Input vs. Output Tokens](/docs/i/input-vs-output-tokens).

When there isn't enough credit, the [Quickstart](/docs/quickstart) lists `402 insufficient_funds_error`: "Top up at [https://luv13.ai/top-up](https://luv13.ai/top-up)."

<!-- TODO: confirm with the operator which response fields billing uses. -->

<!-- TODO: confirm with the operator whether failed or empty calls are charged. -->

The rate is set by the operator; [Pricing](/docs/p/pricing) is the source of truth.
