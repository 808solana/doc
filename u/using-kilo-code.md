---
title: Using Kilo Code
definition: Using Kilo Code with luv13 means adding luv13 as an OpenAI Compatible custom provider in the Kilo Code agent.
description: Add luv13 to Kilo Code as a custom OpenAI Compatible provider in Settings or kilo.jsonc, with notes on tools and token limits.
category: general
author: Quill
status: draft
last_checked: 2026-09-30
---

> **Compatibility: works, with setup caveats.** Kilo Code's **Custom provider** with Provider API **OpenAI Compatible** is built for Chat Completions endpoints like luv13's. Kilo can list luv13's models automatically. Two catches: luv13 doesn't publish context or output limits, and a custom model without limits turns off Kilo's automatic context compaction. Tool calling also isn't confirmed per luv13 model.

## Key takeaways

- Go to **Settings (gear) > Providers > Custom provider**. Set Provider API to **OpenAI Compatible**, Base URL to `https://api.luv13.ai/v1`, add your luv13 key, and pick `luv13/glm-5.3-flash` from the list Kilo fetches.
- Don't pick **OpenAI Responses** or **Anthropic Messages**. luv13 returns 404 for `/v1/responses` and `/v1/messages`.
- For tool use, token limits and other model options, edit `kilo.jsonc`. Keep the key in your global config (`~/.config/kilo/kilo.jsonc`), because Kilo ignores `{env:...}` in a project's config file.
- Set `tool_call: true` if you want Kilo's agent to edit files and run commands. Whether each luv13 model handles tools well isn't confirmed.

## Before you start

You need:

- Kilo Code (the VS Code extension or the Kilo CLI). These steps follow Kilo's "OpenAI Compatible" and "Custom Models" docs, read 2026-09-30. The newest tag in the `Kilo-Org/kilocode` repo that day was `v7.8.1`.
- A luv13 API key. The [Quickstart](/docs/q/quickstart) shows how to get one.
- The model id `luv13/glm-5.3-flash`, from the live list at `https://api.luv13.ai/v1/models`. See [GLM-5.3 Flash](/docs/m/glm-5-3-flash) for details on the model.

Facts about luv13 that affect this setup:

- luv13 serves one generation endpoint, `POST /v1/chat/completions`, plus `GET /v1/models`. `/v1/responses`, `/v1/messages`, `/v1/completions` and `/v1/embeddings` return 404 (checked 2026-09-30). See [Endpoints](/docs/e/endpoints).
- Every model costs a flat $0.33 per 1M tokens, input the same as output. See [Pricing](/docs/p/pricing).
- The model list at `https://api.luv13.ai/v1/models` doesn't report context length, so luv13 publishes no per-model limits. See the [model list](https://luv13.ai/#models).

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

## Option A: add luv13 in Settings

1. Open **Settings** (the gear icon) and go to the **Providers** tab.
2. Scroll to the bottom and click **Custom provider**.
3. Fill in the dialog:

   | Field | What to enter |
   |---|---|
   | **Provider ID** | `luv13`. Kilo's docs allow lowercase letters, numbers, hyphens and underscores. |
   | **Display name** | `luv13` |
   | **Provider API** | **OpenAI Compatible** |
   | **Base URL** | `https://api.luv13.ai/v1` |
   | **API key** | your luv13 key |
   | **Models** | once the base URL is in, Kilo fetches luv13's models from `/v1/models`. Choose `luv13/glm-5.3-flash`, or add it by hand. |
   | **Headers** | leave empty |

4. Click **Submit**. luv13's models then show up in the model picker.
5. Pick `luv13/glm-5.3-flash` and ask something small, like "Reply with the word ready."

To change the provider later, click **Edit provider** next to it in the connected providers section.

## Option B: kilo.jsonc

Kilo's docs say token limits, tool calling and other model options go in `kilo.jsonc`. The global config is `~/.config/kilo/kilo.jsonc`. On Windows it's `C:\Users\<you>\.config\kilo\kilo.jsonc`.

This follows the "OpenAI-compatible provider with a custom endpoint" example in Kilo's docs. It uses the documented `id` field so the model key doesn't need a slash, while the id sent to luv13 stays `luv13/glm-5.3-flash`:

```jsonc
{
  "$schema": "https://app.kilo.ai/config.json",
  "model": "openai-compatible/glm-5.3-flash",
  "provider": {
    "openai-compatible": {
      "options": {
        "apiKey": "{env:LUV13_API_KEY}",
        "baseURL": "https://api.luv13.ai/v1"
      },
      "models": {
        "glm-5.3-flash": {
          "id": "luv13/glm-5.3-flash",
          "name": "GLM-5.3 Flash (luv13)",
          "tool_call": true
        }
      }
    }
  }
}
```

Set `LUV13_API_KEY` in your environment before starting Kilo. Then run `kilo models` to check that the model is listed.

About `{env:LUV13_API_KEY}`: Kilo's docs say `{env:...}` only resolves in trusted config, meaning your global config, `KILO_CONFIG` or `KILO_CONFIG_CONTENT`, or managed config. In a project `kilo.jsonc` committed to a repo, it's ignored, so the provider won't authenticate. Kilo does this so a repo can't send your keys to a `baseURL` of its choosing.

### Token limits

Kilo's docs say that a custom model with no `limit` and no match in its catalog ends up with `context` and `output` set to `0`. When that happens:

- Automatic compaction is off, so conversations grow until the provider rejects them.
- The output cap falls back to 32,000 tokens and is sent as `max_tokens`.
- Context usage isn't tracked.

luv13 doesn't publish per-model context or output limits, and `/v1/models` doesn't include them. So this guide doesn't give numbers. If you want compaction, ask luv13 support for the model's real limits and add them as `"limit": { "context": ..., "output": ... }`. Don't guess.

## What works and what doesn't

| Feature | With luv13 |
|---|---|
| Custom provider with **OpenAI Compatible** | Works. It uses `/v1/chat/completions`. |
| Automatic model detection | Works. luv13 serves `/v1/models`. |
| **OpenAI Responses** or **Anthropic Messages** Provider API | No. Both endpoints return 404 on luv13. |
| Agent tools (file edits, terminal) | Needs `tool_call: true`. Not confirmed per luv13 model. See [Tool Calling on luv13](/docs/t/tool-calling-on-luv13). |
| Automatic context compaction | Only after you set `limit.context`. See [Token limits](#token-limits). |
| Voice transcription via a custom base URL | No. luv13 has no `/audio/transcriptions`. |

## Common errors

| Symptom | Likely cause | Fix |
|---|---|---|
| `401` with `{"error":{"code":401,"message":"unauthorized","type":"invalid_auth"}}` | The key is missing, has extra spaces, or isn't a luv13 key. | Paste your luv13 key again. Run test 2 above to confirm it. |
| `404` with an HTML page titled "404 Not Found" | The URL is wrong. Common versions: `https://api.luv13.ai` with no `/v1`, a doubled path such as `https://api.luv13.ai/v1/v1/...`, a pasted full endpoint like `.../v1/chat/completions` in the base URL field, or a trailing slash. | Set the base URL to exactly `https://api.luv13.ai/v1`. The tool adds `/chat/completions` itself. |
| `404` even though the base URL is right | The tool is calling an endpoint luv13 doesn't serve, such as `/v1/responses`, `/v1/messages`, `/v1/completions` or `/v1/embeddings`. | Use the tool's OpenAI chat-completions mode, and turn off features that need other endpoints. See [Endpoints](/docs/e/endpoints). |
| Model not found, or the model doesn't appear | The model id is misspelled or missing the `luv13/` prefix. | Use the exact id `luv13/glm-5.3-flash` from `https://api.luv13.ai/v1/models`. |
| "Invalid API Key" | Kilo's troubleshooting section lists it. The key is wrong, or `{env:LUV13_API_KEY}` didn't resolve. | Re-enter the key. If you used `{env:...}`, move the provider to your global config and check the variable is set in the shell that starts Kilo. |
| "Model Not Found" | Kilo's docs list it for an invalid model id. | Make sure the id sent is exactly `luv13/glm-5.3-flash` (the `id` field in Option B). |
| The model doesn't appear in the picker | Kilo's docs say to check credentials and that `"model"` matches `provider/model-key`. | In Option B, the value is `openai-compatible/glm-5.3-flash`. Run `kilo models` to see what's active. |
| The conversation keeps growing and never compacts | Kilo's docs say this means `limit.context` is `0` (unset). | Add a `limit` block once you have confirmed limits. See [Token limits](#token-limits). |
| The model writes tool calls as text or never edits files | Kilo's docs say to set `tool_call: true` for tool use. | Set it. If it still fails, try another luv13 model. |

General tip: put the base URL `https://api.luv13.ai/v1` in the Base URL field, not the full `/v1/chat/completions` URL. Kilo's docs say full endpoint URLs are supported, but [Kilo issue #2035](https://github.com/Kilo-Org/kilocode/issues/2035) reported `/chat/completions` being appended twice. The issue was closed in November 2025.

## Sources

- [Kilo Code docs, "Using OpenAI-Compatible Providers with Kilo Code"](https://kilo.ai/docs/ai-providers/openai-compatible) (read 2026-09-30)
- [Kilo Code docs, "Custom Models"](https://kilo.ai/docs/code-with-ai/agents/custom-models) (read 2026-09-30). Covers config fields, token limits, the `id` mapping, provider options and the trusted-config rule for `{env:...}`.
- [Kilo Code docs, "Settings"](https://kilo.ai/docs/getting-started/settings) (config file locations, read 2026-09-30)
- [Kilo-Org/kilocode tags on GitHub](https://github.com/Kilo-Org/kilocode/tags) (newest `v7.8.1` on 2026-09-30)
- [Kilo issue #2035, "Base URL field incorrectly appends /chat/completions"](https://github.com/Kilo-Org/kilocode/issues/2035) (read 2026-09-30)
- luv13 endpoints checked live with curl on 2026-09-30

Related: [Using Cline](/docs/u/using-cline), [Using VS Code](/docs/u/using-vs-code), [Using Claude Code](/docs/u/using-claude-code), [Compatible Tools](/docs/c/compatible-tools).
