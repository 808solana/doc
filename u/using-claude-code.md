---
title: Using Claude Code
definition: Using Claude Code with luv13 isn't possible today, because Claude Code needs an Anthropic-format API and luv13 serves only OpenAI-style chat completions.
description: Why Claude Code can't connect to luv13 today, what the request failure looks like, and which luv13-compatible coding tools to use.
category: general
author: Quill
status: draft
last_checked: 2026-09-30
---

> **Compatibility: doesn't work with luv13 today.** Claude Code only talks to endpoints in Anthropic's formats: the Anthropic Messages API (`/v1/messages`), Amazon Bedrock InvokeModel, or Google Cloud's Agent Platform rawPredict. luv13 serves only the OpenAI-style `POST /v1/chat/completions`, and `POST /v1/messages` returns 404. Claude Code's docs also say Anthropic doesn't support routing Claude Code to non-Claude models through any gateway, and every luv13 model is a non-Claude model. This page explains why, what you'd see if you tried, and which tools to use instead.

## Key takeaways

- Claude Code has no setting for an OpenAI chat-completions endpoint. `ANTHROPIC_BASE_URL` expects a server that speaks the Anthropic Messages API.
- Pointing `ANTHROPIC_BASE_URL` at luv13 fails. Claude Code posts to `/v1/messages`, and luv13 answers `404 Not Found` (checked 2026-09-30).
- A translation proxy between the two formats isn't a route Anthropic documents or supports for non-Claude models, so these docs don't recommend one.
- For a terminal or editor coding agent on luv13, use a tool with an OpenAI-compatible provider, such as [Cline](/docs/u/using-cline), [OpenCode](/docs/o/opencode), [Aider](/docs/a/aider) or [Kilo Code](/docs/u/using-kilo-code).
- If luv13 ever adds an Anthropic-format endpoint, this page will change. Check [Endpoints](/docs/e/endpoints) for what's served now.

## Why it doesn't work

Claude Code's gateway docs (the "Gateway compatibility guide", read 2026-09-30, when Claude Code's changelog listed v2.1.285 as the latest) say a gateway must expose at least one of these formats:

| Format | How Claude Code selects it | Endpoints Claude Code calls |
|---|---|---|
| Anthropic Messages | `ANTHROPIC_BASE_URL` | `/v1/messages`, plus optional `/v1/messages/count_tokens` |
| Amazon Bedrock InvokeModel | `ANTHROPIC_BEDROCK_BASE_URL` with `CLAUDE_CODE_USE_BEDROCK=1` | `/model/{model}/invoke` and related paths |
| Google Cloud's Agent Platform rawPredict | `ANTHROPIC_VERTEX_BASE_URL` with `CLAUDE_CODE_USE_VERTEX=1` | `:rawPredict`, `:streamRawPredict` |

OpenAI Chat Completions isn't on that list. The request and response shapes are different: Anthropic's format sends `system` separately, uses content blocks and `anthropic-version` headers, and streams different event types. So Claude Code can't use luv13 even though luv13 accepts a bearer token.

The docs also say that Anthropic "doesn't endorse, maintain, or audit third-party gateway products, and doesn't support routing Claude Code to non-Claude models through any gateway."

## What you'd see if you tried

Say you set:

```bash
export ANTHROPIC_BASE_URL=https://api.luv13.ai
export ANTHROPIC_AUTH_TOKEN=$LUV13_API_KEY
```

Claude Code would send its inference requests to `https://api.luv13.ai/v1/messages?beta=true`. You can reproduce what luv13 does with that request using the same check Claude Code's docs recommend:

```bash
curl -sS -w '\n%{http_code}\n' -X POST "https://api.luv13.ai/v1/messages" \
  -H "Authorization: Bearer $LUV13_API_KEY" \
  -H "anthropic-version: 2023-06-01" \
  -H "content-type: application/json" \
  -d '{"model": "luv13/glm-5.3-flash", "max_tokens": 16, "messages": [{"role": "user", "content": "hi"}]}'
```

On 2026-09-30 this returned HTTP `404` with an HTML "404 Not Found" page. Setting the base URL to `https://api.luv13.ai/v1` doesn't help, because Claude Code adds `/v1/messages` itself, giving `/v1/v1/messages`, which also returns 404.

## Check that your luv13 key works

Your key and model work fine with tools that speak OpenAI's format. If you don't have a key yet, the [Quickstart](/docs/q/quickstart) shows how to get one. The model used below is [GLM-5.3 Flash](/docs/m/glm-5-3-flash). Confirm both with the standard checks:

Run these two checks in a terminal before you touch the tool. They take a few seconds and rule out key and model problems.

**1. The model id exists.** This call needs no key:

```bash
curl -s https://api.luv13.ai/v1/models
```

The list should include `"id":"luv13/glm-5.3-flash"`.

**2. Your key works for chat.** Set the key in your shell first (`export LUV13_API_KEY="your luv13 key"`), then run:

```bash
curl -s https://api.luv13.ai/v1/chat/completions \
  -H "Authorization: Bearer $LUV13_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "luv13/glm-5.3-flash",
    "messages": [{"role": "user", "content": "Reply with the word ready."}],
    "max_tokens": 20
  }'
```

With a valid key you should get back a JSON chat completion whose `choices[0].message.content` holds the reply, plus a `usage` block. If you see `{"error":{"code":401,"message":"unauthorized","type":"invalid_auth"}}` instead, the key is missing or wrong, and no tool setting will fix that.

## What to use instead

| If you want... | Use | Guide |
|---|---|---|
| A coding agent in the terminal | OpenCode, Aider, or the Cline CLI | [OpenCode](/docs/o/opencode), [Aider](/docs/a/aider), [Using Cline](/docs/u/using-cline) |
| A coding agent inside VS Code | Cline, Kilo Code, Roo Code, or VS Code's own chat | [Using Cline](/docs/u/using-cline), [Using Kilo Code](/docs/u/using-kilo-code), [Roo Code](/docs/r/roo-code), [Using VS Code](/docs/u/using-vs-code) |
| An AI editor | Cursor, with caveats | [Using Cursor](/docs/u/using-cursor) |

## Common errors

| Symptom | Cause | Fix |
|---|---|---|
| `404` from `https://api.luv13.ai/v1/messages` | luv13 doesn't serve the Anthropic Messages API. | None on the Claude Code side. Use one of the tools above. |
| `404` from `https://api.luv13.ai/v1/v1/messages` | `ANTHROPIC_BASE_URL` included `/v1`, and Claude Code added another. | This would still fail without the extra `/v1`, for the reason above. |
| Claude Code keeps using your claude.ai subscription | Claude Code's docs say setting only `ANTHROPIC_BASE_URL`, without `ANTHROPIC_AUTH_TOKEN` or `ANTHROPIC_API_KEY`, leaves the saved claude.ai login as the active credential. | Unset `ANTHROPIC_BASE_URL` and use Claude Code with Anthropic as normal. |
| A startup warning ending in `auth may not work as expected` | Claude Code's docs: a gateway credential variable and a saved login are both active. | Unset the luv13 variables you added. |

General tip: remove `ANTHROPIC_BASE_URL` and `ANTHROPIC_AUTH_TOKEN` from your shell profile and from any `env` block in `~/.claude/settings.json` if you added them while testing, so Claude Code goes back to its normal login.

## Sources

- Claude Code docs, "Other LLM gateways", https://code.claude.com/docs/en/llm-gateway (read 2026-09-30)
- Claude Code docs, "Gateway compatibility guide" (API formats), https://code.claude.com/docs/en/llm-gateway-protocol (read 2026-09-30)
- Claude Code docs, "Connect Claude Code to an LLM gateway" (variables, verification request, troubleshooting table), https://code.claude.com/docs/en/llm-gateway-connect (read 2026-09-30)
- Claude Code changelog, https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md (latest entry 2.1.285 on 2026-09-30)
- luv13's `/v1/messages` checked live with curl on 2026-09-30 (404).

Related: [Endpoints](/docs/e/endpoints), [OpenAI-Compatible APIs](/docs/o/openai-compatible-apis).
