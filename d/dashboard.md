---
title: Dashboard
definition: The luv13 dashboard at dash.luv13.ai is where you sign in, create API keys, top up prepaid credit and see your balance.
description: "What you can do in the luv13 dashboard: sign in with Google or email, create a key, top up from $5 and check your balance."
category: luv13
author: Ink
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- The address is `https://dash.luv13.ai`.
- Sign in with Google or with email.
- Create API keys there. The full key starts with `sk-luv13-` and is shown once, at creation.
- Top up prepaid credit there, any amount from $5 (confirmed by the luv13 operator team, 2026-10-02).
- Your balance is shown there (confirmed by the luv13 operator team on 2026-09-17). <!-- TODO: confirm with the operator whether recent usage is shown in the dashboard. -->

Checked on 2026-09-30: the $5 minimum top-up was confirmed by the luv13 operator team on 2026-10-02, the key format from the [Quickstart](/docs/quickstart), and sign-in from luv13's original docs page. The balance view was confirmed by the luv13 operator team on 2026-09-17.

<!-- TODO: confirm with the operator the sign-in options, the payment method, and whether there's any subscription. -->

## What you do there

| Task | Notes |
|---|---|
| Sign in | Google or email |
| Create a key | Copy it right away; it's shown only once. Store it as `LUV13_API_KEY`. See [API Key Best Practices](/docs/a/api-key-best-practices). |
| Top up | Any amount from $5, in USD (confirmed by the operator team, 2026-10-02) |
| Check balance | Usage draws the balance down at $0.33 per 1M tokens |
| Check recent usage | Compare with the `usage` your code logs. See [Usage and Billing](/docs/u/usage-and-billing). |

If you can't use the dashboard, the Quickstart says to email hi@luv13.ai.

<!-- TODO: confirm with the operator whether the dashboard lets you name, list and revoke keys, and whether usage can be broken down by key or model. -->

## After you have a key

Test it:

```bash
curl https://api.luv13.ai/v1/chat/completions \
  -H "Authorization: Bearer $LUV13_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"model": "luv13/glm-5.3-flash", "messages": [{"role": "user", "content": "ping"}]}'
```

How keys are sent is on [Keys and Accounts](/docs/k/keys-and-accounts) and [Authentication](/docs/a/auth).
