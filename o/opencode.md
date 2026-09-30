---
title: OpenCode
definition: OpenCode is an open-source AI coding agent for the terminal that can use luv13 as a custom OpenAI-compatible provider.
description: Add luv13 as a custom provider in the OpenCode terminal agent, with a sample opencode.json and tips for fixing setup problems.
category: general
author: Quill
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- Add luv13 as a custom provider in your `opencode.json` config.
- Use the `@ai-sdk/openai-compatible` package, and set `options.baseURL` to `https://api.luv13.ai/v1`.
- List the luv13 model ids you want under `models`, such as `luv13/glm-5.3-flash`.
- Store the key with OpenCode's `/connect` command (choose **Other**), or point `options.apiKey` at an environment variable.
- These names come from OpenCode's official providers docs.

## Setup

1. In OpenCode, run `/connect`, scroll to **Other**, and enter a provider id such as `luv13`. Then paste your luv13 key.
2. Create or edit `opencode.json` in your project and add a provider with the **same id**:

```json
{
  "$schema": "https://opencode.ai/config.json",
  "provider": {
    "luv13": {
      "npm": "@ai-sdk/openai-compatible",
      "name": "luv13",
      "options": {
        "baseURL": "https://api.luv13.ai/v1"
      },
      "models": {
        "luv13/glm-5.3-flash": {
          "name": "GLM 5.3 Flash (luv13)"
        }
      }
    }
  }
}
```

3. Run `/models` and pick the luv13 model.

## Using an environment variable instead

OpenCode's config supports `{env:NAME}` values, so you can skip `/connect` and read the key from your environment:

```json
"options": {
  "baseURL": "https://api.luv13.ai/v1",
  "apiKey": "{env:LUV13_API_KEY}"
}
```

## Notes

- The keys under `models` must match luv13's ids exactly. Get them from `https://api.luv13.ai/v1/models`. See [Model IDs](/docs/m/model-ids).
- OpenCode lets you set `limit.context` and `limit.output` per model. luv13's model list doesn't publish context lengths, so leave these out unless the operator gives you numbers. See [Context Window](/docs/c/context-window).
- OpenCode is an agent that relies on tool calling. See [Tool Calling on luv13](/docs/t/tool-calling-on-luv13).
- If it doesn't work, run `opencode auth list` to check the credential, and make sure the provider id in `/connect` matches the one in `opencode.json`.

## Check your settings first

```bash
curl https://api.luv13.ai/v1/chat/completions \
  -H "Authorization: Bearer $LUV13_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"model": "luv13/glm-5.3-flash", "messages": [{"role": "user", "content": "Hi"}]}'
```
