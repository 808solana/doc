---
title: Contact and Support
definition: Contact and support covers how to reach the luv13 team, by email at hi@luv13.ai or the form on luv13.ai, and what to include so a problem can be traced.
category: luv13
author: Ink
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- Email hi@luv13.ai. It's the address luv13.ai lists for questions and for anyone who can't use the dashboard.
- luv13.ai also has a contact form (email and message) on the home page.
- Balance and recent usage are self-serve in the [dashboard](https://luv13.ai/dashboard).
- Never send your full API key in an email or form.

## What to include

A report with these details is much quicker to act on:

- The time of the request, with your time zone
- The endpoint and the model id, for example `POST /v1/chat/completions` with `luv13/glm-5.3-flash`
- The HTTP status code and the error body
- The first few characters of your key at most, such as `sk-luv13-ab`, if the team needs to find your account

Get the status and body in one go:

```bash
curl -s -D - https://api.luv13.ai/v1/chat/completions \
  -H "Authorization: Bearer $LUV13_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"model": "luv13/glm-5.3-flash", "messages": [{"role": "user", "content": "ping"}]}' \
  | grep -iE "^HTTP|error"
```

## Check first

- Is the API up? See [Health Checks](/docs/h/health-checks).
- Is it a known error? See [Errors and Status Codes](/docs/e/errors-and-status-codes) and [Troubleshooting](/docs/t/troubleshooting).

luv13 also has an Instagram account, instagram.com/luv13ai, linked from the site footer.

<!-- TODO: ask the operator for expected response times and whether there's a status page or community channel to list here. -->
