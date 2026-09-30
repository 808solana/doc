---
title: Compatible Tools
definition: Compatible tools are apps and coding agents that accept an OpenAI-style base URL and key, and so can use luv13 models once pointed at https://api.luv13.ai/v1.
description: The tools luv13.ai names, the three settings they need, and which API paths they may call that luv13 doesn't serve.
category: luv13
author: Ink
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- luv13.ai lists these tools under "Use with": Cursor, VS Code, Cline, Claude Code, Open WebUI, Codex, Hermes and Kilo Code.
- Most need the same three settings: base URL `https://api.luv13.ai/v1`, your `sk-luv13-` key, and a model id such as `luv13/glm-5.3-flash`.
- luv13 serves only `GET /v1/models` and `POST /v1/chat/completions`. A tool feature that calls another endpoint won't work.
- Agent-style tools depend on tool calling. luv13 hasn't published per-model tool-calling support, so try more than one model.

## The three settings

| Setting | Value |
|---|---|
| Base URL (may be called API base, endpoint or OpenAI base URL) | `https://api.luv13.ai/v1` |
| API key | Your key from the [dashboard](https://luv13.ai/dashboard) |
| Model | An id from `GET /v1/models`, entered in full including `luv13/` |

Enter the base URL exactly. Don't add `/chat/completions`; the tool adds it. See [Troubleshooting](/docs/t/troubleshooting).

## Guides

- Cline: [Using Cline](/docs/u/using-cline)
- Any tool built on the OpenAI SDKs: [Using the OpenAI SDKs](/docs/u/using-the-openai-sdks)

## Endpoints tools may call that luv13 doesn't serve

These returned 404 on 2026-09-30. If a tool needs one, that part of the tool won't work with luv13:

| Path | Usually used for |
|---|---|
| `/v1/responses` | OpenAI's newer Responses API |
| `/v1/messages` | Anthropic-style Messages API |
| `/v1/embeddings` | Codebase indexing and search |
| `/v1/completions` | Older text completion, sometimes used for autocomplete |

<!-- TODO: confirm with the operator how luv13 expects Claude Code and Codex to connect, since /v1/messages and /v1/responses return 404, and add guides for the listed tools once each setup is verified. -->

## Test before you configure

If the tool can't list models, check that the API answers from your machine:

```bash
curl -s https://api.luv13.ai/v1/models | jq -r '.data[].id'
```
