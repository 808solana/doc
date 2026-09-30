---
title: Troubleshooting
definition: Troubleshooting is a symptom-to-fix list for the most common problems when calling luv13, based on the responses the API actually returns.
description: A symptom-to-fix table for luv13 errors, CORS failures and base URL mistakes, with what each tool setting ends up calling.
category: luv13
author: Ink
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- First check the API is up: `curl -s -o /dev/null -w "%{http_code}\n" https://api.luv13.ai/v1/models` should print `200`.
- 401 is almost always the key or the header format.
- 404 is almost always the URL: the base URL must be exactly `https://api.luv13.ai/v1`, with no trailing slash on paths.
- A tool that shows no models or can't connect usually has the wrong base URL.
- If a model is unavailable, switch to another id.

## Symptom and fix

| Symptom | Likely cause | Fix |
|---|---|---|
| `401` with `invalid_auth` | Key missing, wrong, or not sent as `Authorization: Bearer <key>` | Re-copy the key from where you stored it; check for a missing `Bearer ` or a trailing newline |
| `404` with an HTML page | Wrong path: missing `/v1`, doubled `/v1/v1`, a trailing slash, or an endpoint luv13 doesn't serve | Use `https://api.luv13.ai/v1` as the base URL; see [Endpoints](/docs/e/endpoints) |
| `405` with an HTML page | `GET` sent to `/v1/chat/completions` | Use `POST` |
| `522` | luv13's servers unreachable behind Cloudflare | Wait and retry; see [Health Checks](/docs/h/health-checks) |
| `402` with `insufficient_funds_error` | Not enough credit, per the [Quickstart](/docs/quickstart) | "Top up at [https://luv13.ai/top-up](https://luv13.ai/top-up)." |
| CORS error in the browser console | luv13 rejects cross-origin browser calls | Call luv13 from your server; see [Browser Requests](/docs/b/browser-requests) |
| JSON parse error in your code | The error body was HTML (404 or 405) | Check the status code before parsing |
| Tool feature fails, chat works | The feature uses an endpoint luv13 doesn't serve, such as embeddings | Turn that feature off, or use another service for it |

<!-- TODO: add rows for an unknown model id, an unavailable model, and slow or failing requests under load once the operator confirms their status codes and bodies. -->

## Base URL mistakes

Many tools add `/chat/completions` to whatever you enter. Enter the base URL, not the full endpoint:

| You enter | Tool calls | Result |
|---|---|---|
| `https://api.luv13.ai/v1` | `https://api.luv13.ai/v1/chat/completions` | Works |
| `https://api.luv13.ai` | `https://api.luv13.ai/chat/completions` | 404 |
| `https://api.luv13.ai/v1/chat/completions` | `.../v1/chat/completions/chat/completions` | 404 |

## Still stuck

Email hi@luv13.ai with the time, endpoint, model id, status code and error body. See [Contact and Support](/docs/c/contact-and-support).
