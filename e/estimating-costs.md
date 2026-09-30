---
title: Estimating Costs
definition: Estimating costs means turning luv13 token counts into dollars with one multiplication, because every model has the same flat rate and input costs the same as output.
description: Quick cost tables, a one-line formula and a worked multi-turn example for luv13's flat $0.33 per 1M tokens.
category: luv13
author: Ink
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- Cost in USD = total tokens ÷ 1,000,000 × $0.33. There's no per-model table and no separate input and output rate.
- 800,000 input + 200,000 output = 1,000,000 tokens = $0.33.
- $5, the smallest top-up on luv13.ai/pricing, covers about 15.15M tokens.
- If you resend the whole conversation each turn, input grows every turn, and that's usually the biggest cost.

<!-- TODO: confirm with the operator whether failed or empty calls are charged. -->

The rate is set on [Pricing](/docs/p/pricing); if it changes, change the constant in your code.

## Quick numbers

At $0.33 per 1M tokens:

| Tokens | Cost |
|---|---|
| 1,000 | $0.00033 |
| 100,000 | $0.033 |
| 1,000,000 | $0.33 |
| 15,151,515 | about $5.00 |

```python
RATE_PER_MILLION = 0.33  # USD, same for input and output

def cost_usd(total_tokens):
    return total_tokens / 1_000_000 * RATE_PER_MILLION

print(cost_usd(800_000 + 200_000))  # 0.33
```

## Chats add up

If you send the full history each turn, and every turn adds 500 tokens of question and 500 of answer:

| Turn | Input sent | Output | Tokens this turn |
|---|---|---|---|
| 1 | 500 | 500 | 1,000 |
| 2 | 1,500 | 500 | 2,000 |
| 3 | 2,500 | 500 | 3,000 |
| 10 | 9,500 | 500 | 10,000 |

Ten turns total 55,000 tokens, about $0.018, although only 10,000 tokens of new text were written. Trimming or summarizing old turns keeps this down; see [Conversation History](/docs/c/conversation-history).

Your balance is in the [dashboard](https://dash.luv13.ai) (confirmed by the luv13 operator team on 2026-09-17). <!-- TODO: confirm with the operator whether recent usage is shown in the dashboard. --> See [Usage and Billing](/docs/u/usage-and-billing).
