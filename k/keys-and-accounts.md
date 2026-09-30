---
title: Keys and Accounts
definition: A luv13 account is where you create API keys, which start with sk-luv13-, and hold the prepaid credit that your requests spend.
description: "How a luv13 account and key fit together: Google or email sign-in, sk-luv13- keys shown once, prepaid top-ups and the Bearer header."
category: luv13
author: Ink
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- You sign in to the [dashboard](https://luv13.ai/dashboard) with Google or email and create a key there.
- A luv13 key starts with `sk-luv13-`. The full key is shown once, when you create it, so copy it right away.
- The account is prepaid: you top up credit in USD by card, from $5, and usage draws down the balance. There's no subscription.
- Send the key in the `Authorization: Bearer` header on every chat completion request.
- If you can't use the dashboard, email hi@luv13.ai.

All of the above is from luv13's original docs page and luv13.ai/pricing, checked on 2026-09-30.

## Where the key goes

Keep it in an environment variable, as the [Quickstart](/docs/quickstart) does:

```bash
export LUV13_API_KEY=sk-luv13-...
```

Then send it as a Bearer token:

```bash
curl https://api.luv13.ai/v1/chat/completions \
  -H "Authorization: Bearer $LUV13_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"model": "luv13/glm-5.3-flash", "messages": [{"role": "user", "content": "ping"}]}'
```

In a tool or SDK, paste it into the API key setting and set the base URL to `https://api.luv13.ai/v1`.

A missing or wrong key returns HTTP 401 with `"type": "invalid_auth"`. See [Errors and Status Codes](/docs/e/errors-and-status-codes).

## The account

| What | Detail |
|---|---|
| Sign-in | Google or email, at luv13.ai/dashboard |
| Billing | Prepaid credit in USD; top up any amount from $5 by card, through Stripe |
| Price | $0.33 per 1M tokens on every model, input the same as output |
| Charges | Usage draws down the balance; failed or empty calls aren't charged |
| Balance and usage | Shown in the dashboard |

Keep the key out of client-side code; anyone who reads it can spend your balance. See [API Key Best Practices](/docs/a/api-key-best-practices). How to authenticate is on [Authentication](/docs/a/auth), and prices on [Pricing](/docs/p/pricing).
