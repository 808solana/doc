---
title: Data and Privacy
definition: Data and privacy covers what information luv13 says it handles when you use the API, and where its privacy notice and terms stand.
description: What luv13 has published so far about the data it handles, and which privacy questions are still unanswered.
category: luv13
author: Ink
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- luv13's public privacy notice isn't written yet. The page at luv13.ai/privacy is a placeholder.
- The placeholder says the data luv13 handles today is your account email, session cookies, and the usage needed to run the API.
- The public terms of service aren't written yet either. Until they ship, your use is governed by the agreement you accept at signup.
- luv13 hasn't published whether prompts and replies are logged, how long anything is kept, or whether it's used for training.
- Questions go to hi@luv13.ai.

## What luv13 has published

As of 2026-09-30:

| Topic | What luv13.ai says |
|---|---|
| Privacy notice | Still being written; luv13.ai/privacy is a placeholder |
| Data handled today | Account email, session cookies, and usage needed to run the API |
| Terms | Still being written; the agreement you accept at signup applies |
| Payments | Card top-ups go through Stripe |

## What isn't published yet

<!-- TODO: fill in from the operator's answers on logging and retention. -->

- Whether request and response content (your prompts and the model's replies) is logged
- How long any request data or usage records are kept
- Whether any data is used to train models
- Where requests are processed

Until luv13 publishes these, don't send data you aren't allowed to share with a third-party service, and check the agreement you accepted at signup.

## Protecting your own side

- Keep your API key on the server and out of client-side code. See [API Key Best Practices](/docs/a/api-key-best-practices).
- Strip personal data from prompts when the task doesn't need it. You also pay for every token you send.

For privacy questions, email hi@luv13.ai.
