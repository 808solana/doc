---
title: Vibe Coding
definition: Vibe coding is building software mostly by describing what you want to an AI coding tool and accepting its changes, with little reading of the code yourself.
description: What building software by describing it to an AI tool looks like, where it's risky, and which coding tools work with luv13.
category: general
author: Quill
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- You steer with plain-language requests and judge the result by running it, not by reviewing every line.
- The term was popularized by Andrej Karpathy in early 2025.
- It's fast for prototypes, personal tools and trying out ideas.
- It's risky for code that handles money, private data or security, where unread code can hide real bugs.
- Coding tools that accept an OpenAI-compatible base URL can use luv13 as the model.

## How it usually goes

1. Describe the app or change you want.
2. Let the AI tool write or edit the files.
3. Run it and see what happens.
4. Paste errors back or describe what's wrong, and let the tool fix it.
5. Repeat.

Tools built for this work as [Agents](/docs/a/agents): they read files, make edits and run commands in a loop.

## Doing it more safely

- Use version control and commit often, so you can undo a bad change.
- Keep secrets out of the chat and out of code. See [Environment Variables](/docs/e/environment-variables).
- Ask the tool to write tests, and run them.
- Read the code before it touches real users, real data or real money.
- Watch token use. Long agent sessions re-send a lot of context. See [Usage and Billing](/docs/u/usage-and-billing).

## Tools that work with luv13

Any coding tool with an OpenAI-compatible provider setting can point at `https://api.luv13.ai/v1`. See [Using Cline](/docs/u/using-cline), [Roo Code](/docs/r/roo-code), [Aider](/docs/a/aider), [OpenCode](/docs/o/opencode), [Using Continue](/docs/u/using-continue) and [Zed Editor](/docs/z/zed-editor).

## Example

A quick check that your key and model work before you start a session:

```bash
curl https://api.luv13.ai/v1/chat/completions \
  -H "Authorization: Bearer $LUV13_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "luv13/glm-5.3-flash",
    "messages": [{"role": "user", "content": "Write a Python one-liner that prints the current date."}]
  }'
```
