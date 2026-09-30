---
title: Estimating Costs
definition: Estimating costs means turning luv13 token counts into dollars with one multiplication, because every model has the same flat rate and input costs the same as output.
category: luv13
author: Ink
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- Cost in USD = total tokens ÷ 1,000,000 × $0.33. No per-model table, no separate input and output rates.
- 800,000 input + 200,000 output = 1,000,000 tokens = $0.33.
- $5, the smallest top-up, buys about 15.15M tokens.
- In a chat, each turn resends the whole history, so input grows every turn. That's usually the biggest cost.
- Failed or empty calls aren't charged.

The rate is set on [Pricing](/docs/pricing); if it changes, change the constant below.

## Quick numbers

At $0.33 per 1M tokens:

| Tokens | Cost |
|---|---|
| 1,000 | $0.00033 |
| 100,000 | $0.033 |
| 1,000,000 | $0.33 |
| 15,151,515 | about $5.00 |

## From a response

In the OpenAI format, each response reports `usage.total_tokens`:

```python
RATE_PER_MILLION = 0.33  # USD, same for input and output

def cost_usd(usage):
    return usage.total_tokens / 1_000_000 * RATE_PER_MILLION
```

<!-- TODO: confirm with a real authenticated response that luv13 returns usage.total_tokens. -->

## Chats add up

The model doesn't remember earlier turns, so each request carries the whole conversation. If every turn adds 500 tokens of question and 500 of answer:

| Turn | Input sent | Output | Tokens this turn |
|---|---|---|---|
| 1 | 500 | 500 | 1,000 |
| 2 | 1,500 | 500 | 2,000 |
| 3 | 2,500 | 500 | 3,000 |
| 10 | 9,500 | 500 | 10,000 |

Ten turns total 55,000 tokens, about $0.018, even though only 10,000 tokens of new text were written. To keep costs down, trim or summarize old turns, and set `max_tokens` on replies.

## Before you send

You can't know the output length in advance, but you can cap it. The most a request can cost is (input tokens + `max_tokens`) ÷ 1,000,000 × $0.33.

Your balance and actual usage are in the [dashboard](https://luv13.ai/dashboard). See [Usage and Billing](/docs/u/usage-and-billing).
