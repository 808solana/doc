---
title: Dashboard
definition: The luv13 dashboard at luv13.ai/dashboard is where you sign in, create API keys, top up prepaid credit and see your balance and recent usage.
category: luv13
author: Ink
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- The address is `https://luv13.ai/dashboard`.
- Sign in with Google or with email.
- Create API keys there. The full key starts with `sk-luv13-` and is shown once, at creation.
- Top up credit by card, through Stripe, from $5. There's no subscription.
- Your balance and recent usage are shown there.

All of this is from luv13.ai/docs and luv13.ai/pricing on 2026-09-30.

## What you do there

| Task | Notes |
|---|---|
| Sign in | Google or email |
| Create a key | Copy it right away; it's shown only once. Store it as `LUV13_API_KEY`. See [API Key Best Practices](/docs/a/api-key-best-practices). |
| Top up | Card payment through Stripe, any amount from $5, in USD |
| Check balance | Usage draws the balance down at $0.33 per 1M tokens |
| Check recent usage | Compare with the `usage` your code logs. See [Usage and Billing](/docs/u/usage-and-billing). |

If you can't use the dashboard, luv13.ai/docs says to email hi@luv13.ai.

<!-- TODO: confirm with the operator whether the dashboard lets you name, list and revoke keys, and whether usage can be broken down by key or model. -->

## After you have a key

Test it:

```bash
curl https://api.luv13.ai/v1/chat/completions \
  -H "Authorization: Bearer $LUV13_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"model": "luv13/glm-5.3-flash", "messages": [{"role": "user", "content": "ping"}]}'
```

The step-by-step first request is on [Quickstart](/docs/quickstart), and how keys are sent is on [Authentication](/docs/auth).
