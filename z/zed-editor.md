---
title: Zed Editor
definition: Zed is a code editor with built-in AI features that can use luv13 as an OpenAI-compatible provider.
description: Add luv13 to the Zed editor as an OpenAI-compatible provider through Agent Settings or settings.json, and set up its key.
category: general
author: Quill
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- In Zed, add luv13 as an OpenAI-compatible provider, either from Agent Settings or in `settings.json`.
- The settings live under `language_models.openai_compatible`, with `api_url` set to `https://api.luv13.ai/v1`.
- Each model needs a `name` (the luv13 id) and a `max_tokens` value, which Zed treats as the context window.
- If you name the provider `luv13`, Zed reads the key from the `LUV13_API_KEY` environment variable.
- These names come from Zed's official "Use API Access" docs.

## Setup from the UI

1. Run `agent: open settings` from the command palette.
2. In the LLM Providers section, choose **Add Provider**.
3. Fill in the provider name, API URL (`https://api.luv13.ai/v1`), model id (`luv13/glm-5.3-flash`), and a context window.
4. Enter your luv13 key when asked. Zed stores it in your system keychain, not in `settings.json`.

## Setup in settings.json

```json
{
  "language_models": {
    "openai_compatible": {
      "luv13": {
        "api_url": "https://api.luv13.ai/v1",
        "available_models": [
          {
            "name": "luv13/glm-5.3-flash",
            "display_name": "GLM 5.3 Flash (luv13)",
            "max_tokens": 32000
          }
        ]
      }
    }
  }
}
```

The `32000` above is only an example value. Zed requires a number here, but luv13's model list doesn't publish context lengths, so pick a conservative value or ask the operator. See [Context Window](/docs/c/context-window).

## The API key

Zed builds the environment variable name from the provider id in upper snake case plus `_API_KEY`. A provider id of `luv13` reads `LUV13_API_KEY`, the same name these docs use. A non-empty environment variable takes priority over a key saved in the keychain. Don't put the key in `settings.json`.

## Capabilities

Zed assumes OpenAI-compatible models support tools and not images unless you set `capabilities`. Check [Tool Calling on luv13](/docs/t/tool-calling-on-luv13) and [Image Input](/docs/i/image-input) for what luv13 supports, and adjust if needed.

## Check your settings first

```bash
curl https://api.luv13.ai/v1/chat/completions \
  -H "Authorization: Bearer $LUV13_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"model": "luv13/glm-5.3-flash", "messages": [{"role": "user", "content": "Hi"}]}'
```
