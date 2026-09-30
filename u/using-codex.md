---
title: Using Codex
definition: Using Codex with luv13 would mean setting luv13 as a custom model provider in OpenAI's Codex coding agent.
description: Why OpenAI Codex can't use luv13 today, which Codex setting blocks it, and which tools to use instead.
category: general
author: Quill
status: draft
last_checked: 2026-09-30
---

> **Compatibility: doesn't work today.** Current Codex (CLI release `0.159.2`, published 2026-09-29) talks to custom providers only through the OpenAI Responses API at `/v1/responses`. Codex no longer accepts `wire_api = "chat"`. luv13 serves only `/v1/chat/completions`, and `https://api.luv13.ai/v1/responses` returns 404. So Codex can't use luv13 directly.

## Key takeaways

- Codex's config reference says `responses` is the only supported value for `model_providers.<id>.wire_api`, and it's the default.
- In Codex's source at release `rust-v0.159.2`, `wire_api = "chat"` fails with the error "`wire_api = "chat"` is no longer supported."
- luv13 has no `/v1/responses` endpoint (checked live 2026-09-30), so a luv13 provider block in `config.toml` can't work.
- If you want a terminal coding agent on luv13 today, use one that speaks Chat Completions, such as [Cline](/docs/u/using-cline).
- This page will be updated if luv13 adds `/v1/responses` or Codex brings back Chat Completions.

## Before you start

- A luv13 API key comes from the [Quickstart](/docs/q/quickstart). The model used in luv13 examples is [GLM-5.3 Flash](/docs/m/glm-5-3-flash) (`luv13/glm-5.3-flash`).
- You don't need either for Codex yet, but the tests below show your key works with luv13 itself.

Facts about luv13 that affect this setup:

- luv13 serves one generation endpoint, `POST /v1/chat/completions`, plus `GET /v1/models`. `/v1/responses`, `/v1/messages`, `/v1/completions` and `/v1/embeddings` return 404 (checked 2026-09-30). See [Endpoints](/docs/e/endpoints).
- Every model costs a flat $0.33 per 1M tokens, input the same as output. See [Pricing](/docs/p/pricing).
- The model list at `https://api.luv13.ai/v1/models` doesn't report context length, so luv13 publishes no per-model limits. See the [model list](/docs/models).

## Test your key and model first

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

To see why Codex fails, send a request to the Responses path Codex would use:

```bash
curl -s -o /dev/null -w "%{http_code}\n" -X POST https://api.luv13.ai/v1/responses \
  -H "Authorization: Bearer $LUV13_API_KEY" -H "Content-Type: application/json" \
  -d '{"model":"luv13/glm-5.3-flash","input":"hi"}'
```

This prints `404` (checked 2026-09-30).

## What Codex's docs say

Codex's advanced config docs (read 2026-09-30) define custom providers in `~/.codex/config.toml` under `[model_providers.<id>]`. Every example uses `wire_api = "responses"`, for example:

```toml
[model_providers.proxy]
name = "OpenAI using LLM proxy"
base_url = "https://proxy.example.com/v1"
wire_api = "responses"
```

The config reference entry for `model_providers.<id>.wire_api` says: "Protocol used by the provider. `responses` is the only supported value, and it is the default when omitted."

## What happens if you try luv13 anyway

This is the config you'd write. **It won't work.** It's shown only so you can recognize the errors.

```toml
# ~/.codex/config.toml. Does NOT work with luv13 today.
model_provider = "luv13"
model = "luv13/glm-5.3-flash"

[model_providers.luv13]
name = "luv13"
base_url = "https://api.luv13.ai/v1"
env_key = "LUV13_API_KEY"
wire_api = "responses"
```

- With `wire_api = "responses"` (or the line left out), Codex sends requests to `https://api.luv13.ai/v1/responses`, which returns 404.
- With `wire_api = "chat"`, Codex refuses the config. Its source at `rust-v0.159.2` (`codex-rs/model-provider-info/src/lib.rs`) defines this error:

  ```text
  `wire_api = "chat"` is no longer supported.
  How to fix: set `wire_api = "responses"` in your provider config.
  More info: https://github.com/openai/codex/discussions/7782
  ```

## What works and what doesn't

| Codex surface | Works with luv13? |
|---|---|
| Codex CLI with a custom `model_providers` entry | No. Needs `/v1/responses`. |
| Codex IDE extension and app with the same `config.toml` | No, for the same reason. They read the same config (not tested separately). |
| `openai_base_url` override for the built-in OpenAI provider | No. It still uses the Responses API. |
| `--oss` local mode | Not relevant. It's for local Ollama and LM Studio. |

## Common errors

| Symptom | Likely cause | Fix |
|---|---|---|
| `401` with `{"error":{"code":401,"message":"unauthorized","type":"invalid_auth"}}` | The key is missing, has extra spaces, or isn't a luv13 key. | Paste your luv13 key again. Run test 2 above to confirm it. |
| `404` with an HTML page titled "404 Not Found" | The URL is wrong. Common versions: `https://api.luv13.ai` with no `/v1`, a doubled path such as `https://api.luv13.ai/v1/v1/...`, a pasted full endpoint like `.../v1/chat/completions` in the base URL field, or a trailing slash. | Set the base URL to exactly `https://api.luv13.ai/v1`. The tool adds `/chat/completions` itself. |
| `404` even though the base URL is right | The tool is calling an endpoint luv13 doesn't serve, such as `/v1/responses`, `/v1/messages`, `/v1/completions` or `/v1/embeddings`. | Use the tool's OpenAI chat-completions mode, and turn off features that need other endpoints. See [Endpoints](/docs/e/endpoints). |
| Model not found, or the model doesn't appear | The model id is misspelled or missing the `luv13/` prefix. | Use the exact id `luv13/glm-5.3-flash` from `https://api.luv13.ai/v1/models`. |
| "`wire_api = "chat"` is no longer supported." | Codex dropped Chat Completions for custom providers. The message is in Codex's source at `rust-v0.159.2`. | There's no fix on the Codex side. Use a tool that supports Chat Completions. |
| An unknown variant error for `wire_api` | Codex only accepts `responses`. | Same as above. |
| 404 on every request with `wire_api = "responses"` | luv13 has no `/v1/responses` (checked 2026-09-30). | Same as above. |

General tip: a proxy that converts Responses API requests to Chat Completions could in theory sit between Codex and luv13. That hasn't been tested with luv13, and it isn't covered by luv13's docs.

## Sources

- [Codex docs, "Advanced configuration"](https://developers.openai.com/codex/config-advanced) (read 2026-09-30)
- [Codex docs, "Configuration reference"](https://developers.openai.com/codex/config-reference), `model_providers.<id>.wire_api` entry (read 2026-09-30)
- [Codex source at tag rust-v0.159.2, codex-rs/model-provider-info/src/lib.rs](https://github.com/openai/codex/blob/rust-v0.159.2/codex-rs/model-provider-info/src/lib.rs) (read 2026-09-30)
- [Codex releases on GitHub](https://github.com/openai/codex/releases) (latest `rust-v0.159.2`, published 2026-09-29)
- luv13 `/v1/responses` returning 404, checked live with curl on 2026-09-30

Related: [Using Claude Code](/docs/u/using-claude-code), [Using Cline](/docs/u/using-cline), [Endpoints](/docs/e/endpoints), [Compatible Tools](/docs/c/compatible-tools).
