---
title: Using Hermes Agent
definition: Using Hermes Agent with luv13 means pointing Nous Research's Hermes Agent at luv13 through its custom OpenAI-compatible provider.
description: Set up Nous Research's Hermes Agent with luv13 using the wizard or config.yaml, with notes on context length and transports.
category: general
author: Quill
status: draft
last_checked: 2026-09-30
---

> **Compatibility: should work, not tested end to end.** Hermes Agent, Nous Research's open-source agent, has a first-class `custom` provider for any OpenAI-compatible endpoint. Its docs say it works with any server that implements `/v1/chat/completions`, which luv13 does. Two things to check: Hermes needs at least 64,000 tokens of context for agent work, and luv13 doesn't publish context lengths. Tool calling also isn't confirmed per luv13 model.

## Key takeaways

- Run `hermes model`, choose **Custom endpoint**, and enter base URL `https://api.luv13.ai/v1`, your luv13 key, and model `luv13/glm-5.3-flash`. Or put the same values in `~/.hermes/config.yaml`.
- Use the Chat Completions API mode (`transport: chat_completions`). Don't pick `codex_responses` or `anthropic_messages`, because luv13 serves neither `/v1/responses` nor `/v1/messages`.
- By default, Hermes's auxiliary tasks (vision, web summarization, compression and titles) also go to your main model, so they run on luv13 and are billed like any other request.
- Don't use `OPENAI_BASE_URL` for luv13. Hermes's docs say it only applies to the `openai-api` provider.

## Before you start

You need:

- Hermes Agent installed. The latest GitHub release on 2026-09-30 was `v2026.9.24`, published 2026-09-24. These steps follow the "AI Providers" doc on the repo's `main` branch, read 2026-09-30.
- A luv13 API key. The [Quickstart](/docs/q/quickstart) shows how to get one.
- The model id `luv13/glm-5.3-flash`, from the live list at `https://api.luv13.ai/v1/models`. See [GLM-5.3 Flash](/docs/m/glm-5-3-flash) for details on the model.

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

## Option A: the setup wizard

Hermes's docs recommend the interactive setup:

1. From your terminal (not inside a chat session), run:

   ```bash
   hermes model
   ```

2. Choose **Custom endpoint (self-hosted / VLLM / etc.)**.
3. When asked, enter:

   | Prompt | What to enter |
   |---|---|
   | API base URL | `https://api.luv13.ai/v1` |
   | API key | your luv13 key |
   | Model name | `luv13/glm-5.3-flash` |
   | API mode | Chat Completions (`chat_completions`) |
   | Context length (if asked) | See [Context length](#context-length) below |

Hermes saves the answers to `~/.hermes/config.yaml`. The docs call that file the single source of truth for model, provider and base URL.

## Option B: edit config.yaml

This follows the manual config in Hermes's docs. It uses `key_env` so the key stays out of the file:

```yaml
# ~/.hermes/config.yaml
model:
  default: luv13/glm-5.3-flash
  provider: custom
  base_url: https://api.luv13.ai/v1
  key_env: LUV13_API_KEY
```

Then set `LUV13_API_KEY` in your shell or in `~/.hermes/.env`. Hermes's docs also accept `api_key:` inline, but keep the key out of files you share or commit. See [API Key Best Practices](/docs/a/api-key-best-practices).

If you use more than one endpoint, Hermes's docs describe named providers under `providers:`:

```yaml
providers:
  luv13:
    api: https://api.luv13.ai/v1
    key_env: LUV13_API_KEY
    transport: chat_completions
    default_model: luv13/glm-5.3-flash
```

Switch to it in a session with `/model custom:luv13:luv13/glm-5.3-flash`. Hermes's docs give the pattern as `/model custom:<provider>:<model>`.

## Context length

Hermes's docs say it needs at least **64,000 tokens** of context for agent use with tools. It works out the window in this order:

1. `model.context_length` in `config.yaml`
2. A per-model value on a named provider
3. A cached value
4. The endpoint's `/models` response
5. Several public catalogs
6. A 128K default

luv13's `/v1/models` doesn't include context lengths, and luv13 doesn't publish per-model limits. So Hermes will fall back to a catalog value or its default. Don't pin a `context_length` you haven't confirmed. If Hermes trims history too early or too late, ask luv13 support for the model's real window.

## What works and what doesn't

| Feature | With luv13 |
|---|---|
| Chat through the `custom` provider on `chat_completions` | Expected to work. It's the endpoint luv13 serves. |
| Tool use (Hermes's agent tools) | Depends on the model. Not confirmed per luv13 model. See [Tool Calling on luv13](/docs/t/tool-calling-on-luv13). |
| `codex_responses` or `anthropic_messages` transports | No. luv13 returns 404 for `/v1/responses` and `/v1/messages`. |
| Auxiliary vision tasks on the main model | Only if the model accepts images. See [Image Input](/docs/i/image-input). |
| Embedding-based features that go to the main provider | No. luv13 has no `/v1/embeddings`. |
| `/model custom` auto-detect | Hermes's docs say it only auto-selects when the endpoint lists exactly one model. luv13 lists seven, so give the model id. |

## Common errors

| Symptom | Likely cause | Fix |
|---|---|---|
| `401` with `{"error":{"code":401,"message":"unauthorized","type":"invalid_auth"}}` | The key is missing, has extra spaces, or isn't a luv13 key. | Paste your luv13 key again. Run test 2 above to confirm it. |
| `404` with an HTML page titled "404 Not Found" | The URL is wrong. Common versions: `https://api.luv13.ai` with no `/v1`, a doubled path such as `https://api.luv13.ai/v1/v1/...`, a pasted full endpoint like `.../v1/chat/completions` in the base URL field, or a trailing slash. | Set the base URL to exactly `https://api.luv13.ai/v1`. The tool adds `/chat/completions` itself. |
| `404` even though the base URL is right | The tool is calling an endpoint luv13 doesn't serve, such as `/v1/responses`, `/v1/messages`, `/v1/completions` or `/v1/embeddings`. | Use the tool's OpenAI chat-completions mode, and turn off features that need other endpoints. See [Endpoints](/docs/e/endpoints). |
| Model not found, or the model doesn't appear | The model id is misspelled or missing the `luv13/` prefix. | Use the exact id `luv13/glm-5.3-flash` from `https://api.luv13.ai/v1/models`. |
| Requests go to OpenAI instead of luv13 | You set `OPENAI_BASE_URL`, which Hermes's docs say only applies to the `openai-api` provider. | Use `provider: custom` with `base_url`, as in Option B. |
| 404 from luv13 on every turn | The transport is set to `codex_responses` or `anthropic_messages`. | Set `transport: chat_completions`. |
| Tool calls show up as plain text instead of running | Hermes's docs say this happens when the server or model doesn't support tool calling. | Try another luv13 model, or use the task without tools. |
| The model forgets earlier turns | Hermes's docs describe this when the context window is too small or set wrong. | See [Context length](#context-length). Don't set a value you haven't confirmed. |

General tip: Hermes's docs say that when no reasoning effort is configured, `chat_completions` requests include `reasoning_effort: medium`. It isn't confirmed whether every luv13 model accepts that field. If you get a 400 that points at it, check the error text and report it to luv13 support.

## Sources

- [Hermes Agent docs, "AI Providers"](https://hermes-agent.nousresearch.com/docs/integrations/providers) (`website/docs/integrations/providers.md` on `main`, read 2026-09-30). Covers Custom & Self-Hosted LLM Providers, Named Custom Providers, Context Length Detection and Troubleshooting Local Models.
- [Hermes Agent docs, "FAQ & Troubleshooting"](https://hermes-agent.nousresearch.com/docs/reference/faq) (read 2026-09-30)
- [Hermes Agent releases on GitHub](https://github.com/NousResearch/hermes-agent/releases) (latest `v2026.9.24`, published 2026-09-24)
- luv13 endpoints checked live with curl on 2026-09-30

Related: [Using Cline](/docs/u/using-cline), [Using Codex](/docs/u/using-codex), [Compatible Tools](/docs/c/compatible-tools).
