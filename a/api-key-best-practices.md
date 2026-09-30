---
title: API Key Best Practices
definition: API key best practices are the habits that keep your luv13 key, which starts with sk-luv13- and spends your prepaid credit, from leaking or being misused.
category: luv13
author: Ink
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- A luv13 key starts with `sk-luv13-` and is shown once, when you create it in the dashboard. Copy it somewhere safe right away.
- Keep it in an environment variable such as `LUV13_API_KEY`, never in source code.
- Never put it in client-side code (a web page or mobile app). Anyone who can load the page can read it and spend your credit.
- Send it only in the `Authorization: Bearer` header, and only to `https://api.luv13.ai`.

How to create a key is on [Authentication](/docs/auth). This page is about keeping it safe.

## Why it matters

luv13 is prepaid. Anyone holding your key can make requests that draw down your balance at $0.33 per 1M tokens, on any of the seven models.

## Do

- **Store it in the environment.** `export LUV13_API_KEY=sk-luv13-...` in your shell, or your host's secret settings in production. See [Environment Variables](/docs/e/environment-variables).
- **Keep `.env` files out of git.** Add `.env` to `.gitignore` before the first commit.
- **Call luv13 from your server.** If a browser app needs model output, have your backend make the request. See [Browser Requests](/docs/b/browser-requests).
- **Use one key per app or machine** if the dashboard lets you create several, so you can replace one without breaking the others.
- **Watch your usage** in the dashboard. A jump you don't recognize can mean a leaked key.

## Don't

- Paste the key into chats, tickets, screenshots or public repos.
- Log full request headers. Mask the key if you log requests at all.
- Put the key in a URL. URLs end up in logs and browser history. Send it in the `Authorization: Bearer` header, the way luv13.ai/docs shows.

## If a key leaks

1. Create a new key in the dashboard and switch your apps to it.
2. Remove or disable the leaked key.
3. Check recent usage in the dashboard for requests you didn't make.
4. If you can't reach the dashboard, email hi@luv13.ai.

<!-- TODO: confirm with the operator that the dashboard supports several keys per account and revoking a key, and that a revoked key returns 401 immediately. -->

## Quick test

```bash
curl -s -o /dev/null -w "%{http_code}\n" https://api.luv13.ai/v1/chat/completions \
  -H "Authorization: Bearer $LUV13_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"model": "luv13/glm-5.3-flash", "messages": [{"role": "user", "content": "ping"}], "max_tokens": 1}'
```

`401` means the key is missing, wrong or not being sent. This request uses a few tokens when the key works.
