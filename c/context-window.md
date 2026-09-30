---
title: Context Window
definition: A context window is the most tokens a model can handle in one request, counting both what you send and what it writes back.
category: general
author: Quill
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- The context window is a model's working memory for one request, measured in tokens.
- It covers everything at once: the system prompt, the whole chat history, any tool definitions, and the reply.
- Different models have different context windows.
- If a request is too long, the API returns an error or the reply gets cut short.
- The model has no memory between requests. To continue a chat, you send the earlier messages again, and they count toward the window.

## What counts toward it

Every token in the request counts, including:

- The system message and every user and assistant message you include
- Tool or function definitions and tool results
- The tokens the model writes in its reply

So if a model's window is 100,000 tokens and your prompt uses 95,000, only about 5,000 are left for the answer. (These numbers are only an example. Check your model's real limit.)

## Why it matters

A bigger window lets you send longer documents, more code or a longer conversation in one go. But more tokens in means a higher cost per request, since you pay for every input token. See [What Is a Token](/docs/w/what-is-a-token).

## Staying inside the limit

- **Trim old messages.** Drop or summarize the earliest turns of a long chat.
- **Send only what's needed.** Paste the relevant section of a file, not the whole thing.
- **Cap the reply.** Set `max_tokens` so the reply can't use more room than you've left for it.

## On luv13

Context windows depend on the model. Pick a model id from the live list at `https://api.luv13.ai/v1/models` and check its limit on [Listing Models](/docs/l/listing-models). <!-- TODO: confirm whether /v1/models reports a context length per model once the API is back -->
