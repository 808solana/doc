---
title: Compatible Tools
definition: Compatible tools are apps and coding agents that accept an OpenAI-style base URL and key, and so can use luv13 models once pointed at https://api.luv13.ai/v1.
description: The tools luv13.ai names, the three settings they need, and which API paths they may call that luv13 doesn't serve.
category: luv13
author: Ink
status: draft
last_checked: 2026-10-01
---

## Key takeaways

- luv13.ai lists these tools under "Use with": Cursor, VS Code, Cline, Claude Code, Open WebUI, Codex, Hermes and Kilo Code. Claude Code and Codex don't work today.
- Most need the same three settings: base URL `https://api.luv13.ai/v1`, your `sk-luv13-` key, and a model id such as `luv13/glm-5.3-flash`.
- luv13 serves only `GET /v1/models` and `POST /v1/chat/completions`. A tool feature that calls another endpoint won't work.
- Agent-style tools depend on tool calling. luv13 hasn't published per-model tool-calling support, so try more than one model.

## The three settings

| Setting | Value |
|---|---|
| Base URL (may be called API base, endpoint or OpenAI base URL) | `https://api.luv13.ai/v1` |
| API key | Your key from the [dashboard](https://dash.luv13.ai) |
| Model | An id from `GET /v1/models`, entered in full including `luv13/` |

Enter the base URL exactly. Don't add `/chat/completions`; the tool adds it. See [Troubleshooting](/docs/t/troubleshooting).

## Guides

Status as tested on 2026-09-30. Each guide has the details. The [Use with](#use-with) sections below match the anchors on luv13.ai (`#cursor`, `#vs-code`, and the rest).

| Tool | Works with luv13? | Guide |
|---|---|---|
| Cursor | Yes, with caveats: only local Chat and Agent | [Using Cursor](/docs/u/using-cursor) |
| VS Code | Yes, with caveats: inline suggestions and embeddings still need Copilot | [Using VS Code](/docs/u/using-vs-code) |
| Cline | Yes | [Using Cline](/docs/u/using-cline) |
| Kilo Code | Yes, with setup caveats | [Using Kilo Code](/docs/u/using-kilo-code) |
| Open WebUI | Chat only | [Using Open WebUI](/docs/u/using-open-webui) |
| Hermes Agent | Should work as a custom provider; not yet tested end to end | [Using Hermes](/docs/u/using-hermes) |
| Claude Code | Not today: it needs `/v1/messages` | [Using Claude Code](/docs/u/using-claude-code) |
| Codex | Not today: it needs `/v1/responses` | [Using Codex](/docs/u/using-codex) |
| OpenAI SDK apps | Yes | [Using the OpenAI SDKs](/docs/u/using-the-openai-sdks) |

## Use with

Short landing sections for the tools named on luv13.ai. Full setup steps live in each guide.

<a id="cursor"></a>

## Cursor

Point Cursor's OpenAI API key and **Override OpenAI Base URL** at `https://api.luv13.ai/v1`. Local Chat and Agent can use luv13; Tab, Auto, Cloud Agents, and the Cursor CLI cannot. See [Using Cursor](/docs/u/using-cursor).

<a id="vs-code"></a>

## VS Code

Configure VS Code's custom OpenAI-compatible chat endpoint with base URL `https://api.luv13.ai/v1`, your luv13 key, and a `luv13/` model id. Inline suggestions and embeddings still need Copilot or another provider. See [Using VS Code](/docs/u/using-vs-code).

<a id="cline"></a>

## Cline

In Cline, choose the **OpenAI Compatible** provider, set base URL `https://api.luv13.ai/v1`, paste your luv13 key, and enter a model such as `luv13/glm-5.3-flash`. See [Using Cline](/docs/u/using-cline).

<a id="claude-code"></a>

## Claude Code

Claude Code does not work with luv13 today. It needs Anthropic-style `POST /v1/messages`, which luv13 does not serve. See [Using Claude Code](/docs/u/using-claude-code).

<a id="open-webui"></a>

## Open WebUI

Add luv13 as an OpenAI-compatible connection: base URL `https://api.luv13.ai/v1`, your luv13 key, and a `luv13/` model id. Chat works; features that call embeddings or other paths will not. See [Using Open WebUI](/docs/u/using-open-webui).

<a id="codex"></a>

## Codex

OpenAI's Codex CLI does not work with luv13 today. It needs `POST /v1/responses`, which luv13 does not serve. See [Using Codex](/docs/u/using-codex).

<a id="hermes"></a>

## Hermes

Configure Hermes Agent with a custom OpenAI-compatible provider pointing at `https://api.luv13.ai/v1` and a `luv13/` model id. Expected to work; not yet tested end to end on luv13. See [Using Hermes](/docs/u/using-hermes).

<a id="kilo-code"></a>

## Kilo Code

Point Kilo Code's OpenAI-compatible settings at `https://api.luv13.ai/v1` with your luv13 key and a `luv13/` model id. See [Using Kilo Code](/docs/u/using-kilo-code).

## Endpoints tools may call that luv13 doesn't serve

These returned 404 on 2026-09-30. If a tool needs one, that part of the tool won't work with luv13:

| Path | Usually used for |
|---|---|
| `/v1/responses` | OpenAI's newer Responses API |
| `/v1/messages` | Anthropic-style Messages API |
| `/v1/embeddings` | Codebase indexing and search |
| `/v1/completions` | Older text completion, sometimes used for autocomplete |

<!-- TODO: update the Claude Code and Codex rows if the operator adds /v1/messages or /v1/responses. -->

## Test before you configure

If the tool can't list models, check that the API answers from your machine:

```bash
curl -s https://api.luv13.ai/v1/models | jq -r '.data[].id'
```
