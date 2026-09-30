---
title: Using VS Code
definition: Using VS Code with luv13 means adding luv13 to VS Code's chat as a Custom Endpoint model that uses the Chat Completions API.
description: Add luv13 to VS Code chat with the built-in Custom Endpoint provider, the exact chatLanguageModels.json, limits and common fixes.
category: general
author: Quill
status: draft
last_checked: 2026-09-30
---

> **Compatibility: works with caveats.** VS Code's chat has a built-in **Custom Endpoint** model provider, part of its "bring your own key" (BYOK) support. It can call any endpoint that speaks the Chat Completions API, which luv13 does. Set the API type to **Chat Completions**. The other two types, Responses and Messages, call endpoints luv13 doesn't serve. BYOK covers chat and utility tasks only. Inline suggestions, semantic search and embedding-based features still need GitHub Copilot.

## Key takeaways

- Use **Chat: Manage Language Models** > **Add Models** > **Custom Endpoint**, pick the **Chat Completions** API type, and enter your luv13 key.
- In the `chatLanguageModels.json` file VS Code opens, set `"vendor": "customendpoint"`, `"apiType": "chat-completions"`, the model `id` `luv13/glm-5.3-flash` and the `url` `https://api.luv13.ai/v1/chat/completions`.
- Keep the key out of the file. VS Code's docs recommend an input variable such as `"apiKey": "${input:luv13ApiKey}"`.
- A model shows up for agents only if `toolCalling` is `true`. luv13 hasn't confirmed tool calling per model, so test agent mode before you rely on it.
- The Custom Endpoint provider replaces the deprecated **OpenAI Compatible** provider and the `github.copilot.chat.customOAIModels` setting. Don't follow older guides that use them.

## Before you start

You need:

- VS Code. These steps follow the VS Code docs page "AI language models in VS Code", dated 9/30/2026. The latest stable VS Code release that day was 1.140.0.
- A luv13 API key. See [Authentication](/docs/a/auth).
- The model id `luv13/glm-5.3-flash`, from the live list at `https://api.luv13.ai/v1/models`.
- Optional: a GitHub account. VS Code's docs say BYOK models work without a GitHub account or Copilot plan, but some features then need extra setup (see "Utility tasks" below).
- On Copilot Business or Enterprise: your admin must enable the **Bring Your Own Language Model Key in VS Code** policy on GitHub.com.

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

## Set up the Custom Endpoint provider

1. Open the Chat view. In the model picker, select **Manage Language Models** (gear icon), or run **Chat: Manage Language Models** from the Command Palette.
2. Select **Add Models**, then choose **Custom Endpoint** from the list.
3. Enter a group name, such as `luv13`. This label groups the models in the model picker.
4. Enter a display name and your luv13 API key.
5. When asked for the API type, choose **Chat Completions**.
6. VS Code opens `chatLanguageModels.json`. Make the luv13 entry look like this, then save:

```json
[
  {
    "name": "luv13",
    "vendor": "customendpoint",
    "apiKey": "${input:luv13ApiKey}",
    "apiType": "chat-completions",
    "models": [
      {
        "id": "luv13/glm-5.3-flash",
        "name": "GLM 5.3 Flash (luv13)",
        "url": "https://api.luv13.ai/v1/chat/completions",
        "toolCalling": true,
        "vision": false,
        "streaming": true
      }
    ]
  }
]
```

7. Pick **GLM 5.3 Flash (luv13)** in the chat model picker. If it doesn't appear, restart VS Code, as the docs suggest.

### What each field does

| Field | Value for luv13 | Why |
|---|---|---|
| `vendor` | `customendpoint` | Selects the Custom Endpoint provider. |
| `apiKey` | `${input:luv13ApiKey}` | VS Code's docs recommend an input variable so the raw key never sits in the file. If VS Code already filled this in from step 4, keep what it wrote, as long as it isn't your raw key. |
| `apiType` | `chat-completions` | The only API type luv13 serves. `responses` and `messages` would call `/v1/responses` or `/v1/messages`, which return 404. |
| `id` | `luv13/glm-5.3-flash` | Sent to luv13 as the `model` field, so it must match the live list exactly. |
| `url` | `https://api.luv13.ai/v1/chat/completions` | The full endpoint. VS Code uses a URL that already contains `/chat/completions` as-is. |
| `toolCalling` | `true` | Needed for the model to show up for agents. Set it to `false` if tool calls fail and you only want plain chat. |
| `vision` | `false` | luv13 hasn't confirmed image input. See [Image Input](/docs/i/image-input). |
| `streaming` | `true` | VS Code's default. See [Streaming on luv13](/docs/s/streaming-on-luv13). |

### About token limits

VS Code's reference lists `maxInputTokens`, `maxOutputTokens` and `contextWindow` to describe a model's context window. luv13 doesn't publish context lengths, so this page leaves them out rather than invent numbers. If your VS Code version refuses to save the model without them, enter conservative values you've tested, and treat them as your own estimate. See [Context Window](/docs/c/context-window).

## What works and what doesn't

| VS Code feature | Uses luv13? |
|---|---|
| Chat (ask and edit) | Yes |
| Agent mode | Only if `toolCalling` is `true`, and only as well as the model handles tool calls |
| Inline chat | Yes, if you choose the luv13 model, or set it with `inlineChat.defaultModel` |
| Utility tasks (titles, commit messages and so on) | Optional, through `chat.utilityModel` and `chat.utilitySmallModel` |
| Inline suggestions (code completions) | No. VS Code's docs say these can't use BYOK models. |
| Semantic search and embedding-based features | No. These need GitHub Copilot, and luv13 serves no embeddings. |

### Utility tasks

VS Code also uses small background models for titles, commit messages and similar tasks. With a Copilot account these default to GitHub's models. If you use luv13 without signing in to GitHub, VS Code shows a notice asking you to configure utility models. You can set `chat.utilityModel` and `chat.utilitySmallModel` to the luv13 model, or set `chat.byokUtilityModelDefault` to **Main Agent Model**. Either way, those background requests are billed by luv13 like any other tokens.

## Another option: an extension

If you'd rather not use VS Code's built-in chat, the Continue and Cline extensions both run in VS Code and support OpenAI-compatible endpoints. See [Using Continue](/docs/u/using-continue) and [Using Cline](/docs/u/using-cline).

## Common errors

| Symptom | Likely cause | Fix |
|---|---|---|
| `401` with `{"error":{"code":401,"message":"unauthorized","type":"invalid_auth"}}` | The key is missing, has extra spaces, or isn't a luv13 key. | Paste your luv13 key again. Run test 2 above to confirm it. |
| `404` with an HTML page titled "404 Not Found" | The URL is wrong. Common versions: `https://api.luv13.ai` with no `/v1`, a doubled path such as `https://api.luv13.ai/v1/v1/...`, a pasted full endpoint like `.../v1/chat/completions` in the base URL field, or a trailing slash. | Set the base URL to exactly `https://api.luv13.ai/v1`. The tool adds `/chat/completions` itself. |
| `404` even though the base URL is right | The tool is calling an endpoint luv13 doesn't serve, such as `/v1/responses`, `/v1/messages`, `/v1/completions` or `/v1/embeddings`. | Use the tool's OpenAI chat-completions mode, and turn off features that need other endpoints. See [Endpoints](/docs/e/endpoints). |
| Model not found, or the model doesn't appear | The model id is misspelled or missing the `luv13/` prefix. | Use the exact id `luv13/glm-5.3-flash` from `https://api.luv13.ai/v1/models`. |
| `404` right after adding the model | `apiType` is `responses` or `messages`, so VS Code called `/v1/responses` or `/v1/messages`. | Set `"apiType": "chat-completions"` at both the provider and model level, or remove the model-level value. |
| `404` with a doubled path | The `url` has an extra version segment, such as `https://api.luv13.ai/v1/v1/chat/completions`. VS Code only adds `/v1` when the URL doesn't already end in a version segment. | Use exactly `https://api.luv13.ai/v1/chat/completions`. |
| The luv13 model is missing in agent mode | VS Code's docs say models without tool calling are hidden for agents. | Set `"toolCalling": true`, save and restart VS Code. |
| The model doesn't appear anywhere | VS Code hasn't reloaded the file, or the model is hidden. | Restart VS Code. In the Language Models editor, check the eye icon so it's visible. |
| **Add Models** or Custom Endpoint is missing | On Copilot Business or Enterprise, the BYOK policy is off. | Ask your GitHub admin to enable **Bring Your Own Language Model Key in VS Code**. |
| The picker only shows **Auto** | The workspace is untrusted (Restricted Mode). | Trust the workspace. |
| A notice about utility models | You're using BYOK without a GitHub sign-in. | Set `chat.utilityModel` and `chat.utilitySmallModel`, or `chat.byokUtilityModelDefault`, as described above. |

General tip: if agent requests fail but plain chat works, set `"toolCalling": false` to confirm that tool calls are the problem, then see [Tool Calling on luv13](/docs/t/tool-calling-on-luv13).

## Sources

- VS Code docs, "AI language models in VS Code", https://code.visualstudio.com/docs/copilot/customization/language-models (page dated 9/30/2026, read 2026-09-30). Covers BYOK, the Custom Endpoint provider, its configuration reference, URL resolution and utility models.
- VS Code release list, https://update.code.visualstudio.com/api/releases/stable (latest stable 1.140.0 on 2026-09-30)
- luv13 endpoints and errors checked live with curl on 2026-09-30.

Related: [Using Cursor](/docs/u/using-cursor), [Base URL](/docs/b/base-url), [OpenAI-Compatible APIs](/docs/o/openai-compatible-apis).
