---
title: Using Cline
definition: Cline is an open-source AI coding agent for VS Code and other editors that can use luv13 through its OpenAI Compatible provider.
description: Exact Cline settings for luv13, including model configuration, a CLI note, key and model checks, and fixes for common errors.
category: general
author: Quill
status: draft
last_checked: 2026-09-30
---

> **Compatibility: works.** Cline's **OpenAI Compatible** provider talks to any endpoint that serves OpenAI-style chat completions, which is what luv13 serves. How well Cline's agent works still depends on the model's tool calling, which luv13 hasn't confirmed per model.

## Key takeaways

- In Cline's settings, set **API Provider** to **OpenAI Compatible**, **Base URL** to `https://api.luv13.ai/v1`, **API Key** to your luv13 key, and **Model** to `luv13/glm-5.3-flash`.
- Leave **Use Azure Identity Authentication** unchecked. It's only for Azure.
- In **Model Configuration**, enter `0.33` as both the input and output price if you want Cline's cost display to match luv13. luv13 publishes no context window, so leave that value at Cline's default.
- Cline is an agent that reads files and runs commands through tool calls. Start with a small task to check the model handles them.
- Run the two curl checks below first. If they pass and Cline fails, the problem is in Cline's settings.

## Before you start

You need:

- Cline installed in your editor. Cline's docs list VS Code, Cursor, Windsurf, VSCodium, Antigravity and JetBrains. In VS Code, open the Extensions view, search for Cline and click **Install**.
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

## Set up Cline in your editor

These steps follow Cline's "OpenAI Compatible" provider page, read on 2026-09-30. The latest Cline release on GitHub that day was `desktop-v0.0.39`, published 2026-09-30.

1. Open Cline from the editor's sidebar.
2. Click the settings (gear) icon in the Cline panel.
3. Set **API Provider** to **OpenAI Compatible**.
4. Fill in the fields exactly like this:

   | Field | What to enter |
   |---|---|
   | **Base URL** | `https://api.luv13.ai/v1` |
   | **API Key** | your luv13 key |
   | **Model** | `luv13/glm-5.3-flash` |
   | **Use Azure Identity Authentication** | leave unchecked |

5. Open **Model Configuration** and set:

   | Field | What to enter |
   |---|---|
   | **Input Price** | `0.33` (per million tokens) |
   | **Output Price** | `0.33` (per million tokens) |
   | **Context Window** | leave Cline's default. luv13 publishes no context length. |
   | **Max Output Tokens** | leave the default, or lower it to cap reply length |
   | **Image Support** | leave off unless you've confirmed image input on luv13. See [Image Input](/docs/i/image-input). |
   | **Computer Use** | leave off unless you've tested it with this model |

6. If your version shows a **Verify** button, click it. Cline's docs use it to confirm the connection.
7. Close settings and start a small task, such as "List the files in this folder and describe the project in two sentences."

## Using the Cline CLI

Cline's docs say its settings live under `~/.cline/`, and API keys and provider settings go in `~/.cline/data/settings/providers.json`, shared by the IDE extension, the CLI and the SDK. To set up a provider from the terminal, run:

```bash
cline auth
```

and choose the OpenAI-compatible option. The CLI reference also lists `-P, --provider <id>`, `-k, --key <api-key>` and `-m, --model <model-id>` flags for a single run. The docs don't give the provider id to use with `-P` for OpenAI Compatible, so use the interactive `cline auth` flow instead of guessing it. Don't edit `providers.json` by hand or commit it, because it holds your key.

## Common errors

| Symptom | Likely cause | Fix |
|---|---|---|
| `401` with `{"error":{"code":401,"message":"unauthorized","type":"invalid_auth"}}` | The key is missing, has extra spaces, or isn't a luv13 key. | Paste your luv13 key again. Run test 2 above to confirm it. |
| `404` with an HTML page titled "404 Not Found" | The URL is wrong. Common versions: `https://api.luv13.ai` with no `/v1`, a doubled path such as `https://api.luv13.ai/v1/v1/...`, a pasted full endpoint like `.../v1/chat/completions` in the base URL field, or a trailing slash. | Set the base URL to exactly `https://api.luv13.ai/v1`. The tool adds `/chat/completions` itself. |
| `404` even though the base URL is right | The tool is calling an endpoint luv13 doesn't serve, such as `/v1/responses`, `/v1/messages`, `/v1/completions` or `/v1/embeddings`. | Use the tool's OpenAI chat-completions mode, and turn off features that need other endpoints. See [Endpoints](/docs/e/endpoints). |
| Model not found, or the model doesn't appear | The model id is misspelled or missing the `luv13/` prefix. | Use the exact id `luv13/glm-5.3-flash` from `https://api.luv13.ai/v1/models`. |
| Cline reports "Invalid API Key" | Cline's docs list this for a mistyped key or a key from another provider. | Paste the luv13 key again. Make sure you didn't paste an OpenAI or Anthropic key. |
| Cline reports "Model Not Found" | Cline's docs list this for an id the base URL doesn't serve. | Enter `luv13/glm-5.3-flash` exactly. |
| Connection errors | Cline's docs point to a wrong base URL, a blocked network or a firewall. | Check the base URL, then run test 2 from the same machine. Behind a proxy, the VS Code extension uses VS Code's own proxy settings, and the CLI uses the `https_proxy` and `http_proxy` environment variables. |
| The task stalls, loops or says a tool call failed | The model didn't return a tool call Cline could use. Tool support on luv13 isn't confirmed per model. | Try a smaller task, or another id from the live list. See [Tool Calling on luv13](/docs/t/tool-calling-on-luv13). |

General tip: Cline's cost display uses the prices you enter. It doesn't read luv13's billing. For your real spend, see [Usage and Billing](/docs/u/usage-and-billing).

## Sources

- Cline docs, "OpenAI Compatible", https://docs.cline.bot/provider-config/openai-compatible (read 2026-09-30)
- Cline docs, "Config", https://docs.cline.bot/getting-started/config (read 2026-09-30)
- Cline docs, "CLI Reference", https://docs.cline.bot/cli/cli-reference (read 2026-09-30)
- Cline docs, "Networking and Proxies", https://docs.cline.bot/troubleshooting/networking-and-proxies (read 2026-09-30)
- Cline releases on GitHub, https://github.com/cline/cline/releases (latest: desktop-v0.0.39, 2026-09-30)
- luv13 endpoints and errors checked live with curl on 2026-09-30.

Related: [Using Cursor](/docs/u/using-cursor), [Base URL](/docs/b/base-url), [Agents](/docs/a/agents).
