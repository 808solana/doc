---
title: Usage and Billing
definition: Usage and billing is how luv13 counts the tokens each request uses and charges them against your prepaid credit at a flat $0.33 per 1M tokens.
category: luv13
author: Ink
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- You pay per token: a flat $0.33 per 1M tokens on every model, and input tokens cost the same as output tokens.
- Billing is prepaid credit in USD. You top up by card in the dashboard (through Stripe), from $5 upward, and usage draws down the balance.
- There's no subscription.
- Failed or empty calls aren't charged.
- Your balance and recent usage are in the [dashboard](https://luv13.ai/dashboard).

All of the above is from luv13.ai/docs and luv13.ai/pricing, checked on 2026-09-30.

## How a request is charged

Because input and output cost the same, you only need the total token count:

```
cost in USD = total tokens / 1,000,000 × 0.33
```

Worked example: 800,000 input tokens plus 200,000 output tokens is 1,000,000 tokens, so the cost is $0.33.

In the OpenAI format, each chat completion response reports its tokens in `usage`:

| Field | Meaning |
|---|---|
| `prompt_tokens` | Input tokens: everything you sent, including the resent conversation history |
| `completion_tokens` | Output tokens: the model's reply |
| `total_tokens` | The sum of the two, which is what the flat rate applies to |

<!-- TODO: confirm with a real authenticated response that luv13 returns these usage fields, and confirm with the operator that billing uses exactly these counts. -->

For code that turns `usage` into dollars, see [Estimating Costs](/docs/e/estimating-costs).

## Balance

Top up any amount from $5 in the dashboard. Each successful request draws down the balance by its cost.

<!-- TODO: confirm with the operator what happens when the balance reaches zero: the status code and body returned, and whether a request that starts with some balance can finish below zero. -->

## What isn't charged

luv13.ai/docs says failed or empty calls aren't charged. So a request that errors, such as a 401 or a capacity failure, costs nothing, and retrying it doesn't double-charge you.

<!-- TODO: confirm with the operator what "empty" means exactly (for example, a reply with zero output tokens) and whether a request the client cancels mid-stream is charged for the tokens produced so far. -->

The price law is set by the operator and may change; [Pricing](/docs/pricing) is the source of truth.
