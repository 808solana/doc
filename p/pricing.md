---
title: Pricing
definition: luv13 charges one flat price, $0.33 per 1M tokens, on every model, and input tokens cost the same as output tokens.
description: "luv13's flat rate for all seven models, prepaid USD credit from $5, what isn't charged, and a worked cost example with sample token counts."
category: luv13
author: Ink
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- Every model costs $0.33 per 1M tokens.
- Input costs the same as output, so only the total token count matters.
- Billing is prepaid credit in USD. You top up any amount from $5 in the dashboard.
- Top-ups are by card, through Stripe, and usage draws down the balance. There's no subscription.
- Failed or empty calls are not charged.

Checked on 2026-09-30 against live luv13.ai/pricing and luv13's original docs page.

## Price by model

All seven ids from live `GET https://api.luv13.ai/v1/models`, with the price listed on luv13.ai/pricing:

| Model | Id | Price per 1M tokens |
|---|---|---|
| [Kimi K3](/docs/m/kimi-k3) | `luv13/kimi-k3` | $0.33 |
| [Kimi K3 Fast](/docs/m/kimi-k3-fast) | `luv13/kimi-k3-fast` | $0.33 |
| [GLM 5.3](/docs/m/glm-5-3) | `luv13/glm-5.3` | $0.33 |
| [GLM-5.3 Flash](/docs/m/glm-5-3-flash) | `luv13/glm-5.3-flash` | $0.33 |
| [DeepSeek V4.1 Flash](/docs/m/deepseek-v4-1-flash) | `luv13/deepseek-v4.1-flash` | $0.33 |
| [DeepSeek V4-Pro](/docs/m/deepseek-v4-pro) | `luv13/deepseek-v4-pro` | $0.33 (temporarily unavailable) |
| [Qwen 3.8 27B](/docs/m/qwen-3-8-27b) | `luv13/qwen-3.8-27b` | $0.33 |

The same rate applies to input and output tokens on every model.

## How to work out a cost

```
cost in USD = (input tokens + output tokens) / 1,000,000 × $0.33
```

**Example 1** (the example in the [Quickstart](/docs/quickstart)): 800,000 input tokens + 200,000 output tokens = 1,000,000 tokens. 1,000,000 / 1,000,000 × $0.33 = **$0.33**.

**Example 2** (sample numbers for one request): 12,000 input tokens + 3,000 output tokens = 15,000 tokens. 15,000 / 1,000,000 × $0.33 = **$0.00495**.

More worked numbers are on [Estimating Costs](/docs/e/estimating-costs).

## Paying

| What | Detail |
|---|---|
| Model | Prepaid credit in USD |
| Top-up | Any amount from $5, in the [dashboard](https://luv13.ai/dashboard) |
| Payment | By card, through Stripe |
| Charges | Usage draws down the balance |
| Not charged | Failed or empty calls |
| Subscription | None |

Your balance and recent usage are in the dashboard. See [Usage and Billing](/docs/u/usage-and-billing) and [Keys and Accounts](/docs/k/keys-and-accounts).

<!-- TODO: ask the operator what happens when the balance reaches zero. -->

## Try it

```bash
curl https://api.luv13.ai/v1/chat/completions \
  -H "Authorization: Bearer $LUV13_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"model": "luv13/glm-5.3-flash", "messages": [{"role": "user", "content": "ping"}]}'
```
