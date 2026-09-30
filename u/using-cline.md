---
title: Using Cline
definition: Cline is an AI coding agent for VS Code that can use luv13 through its OpenAI Compatible provider setting.
description: Point the Cline VS Code agent at luv13 with the OpenAI Compatible provider, plus model settings and troubleshooting tips.
category: general
author: Quill
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- In Cline's settings, set **API Provider** to **OpenAI Compatible**.
- Set **Base URL** to `https://api.luv13.ai/v1`.
- Paste your luv13 key into **API Key**, and enter a model id such as `luv13/glm-5.3-flash` in **Model**.
- Cline acts as an agent and uses tools, so pick a model that handles tool calling well.
- Setting names on this page come from Cline's official docs. Check them if the UI has changed.

## Setup

1. Install the Cline extension in VS Code.
2. Open Cline and click the settings (gear) icon.
3. Set **API Provider** to **OpenAI Compatible**.
4. Fill in:
   - **Base URL:** `https://api.luv13.ai/v1`
   - **API Key:** your luv13 key
   - **Model:** `luv13/glm-5.3-flash`, or another id from luv13's live list
5. Save and start a task.

## Model Configuration

Cline's OpenAI Compatible provider has a **Model Configuration** section for values like max output tokens, context window size, image support, and input and output price.

- **Price:** luv13 charges a flat $0.33 per 1M tokens on every model, input the same as output. Enter that as both the input and output price if you want Cline's cost estimates to match. See [Pricing](/docs/p/pricing).
- **Context window:** luv13's model list doesn't publish context lengths, so there's no official number to enter. Leave Cline's default or ask the operator. See [Context Window](/docs/c/context-window).

## Troubleshooting

- **Invalid API key:** check you pasted your luv13 key, not another provider's.
- **Model not found:** the id must match the live list exactly, including the `luv13/` prefix. See [Model IDs](/docs/m/model-ids).
- **Connection errors:** make sure the base URL ends in `/v1` with no extra path. See [Base URL](/docs/b/base-url).

## Check your settings first

If Cline can't connect, test the same key and model with curl:

```bash
curl https://api.luv13.ai/v1/chat/completions \
  -H "Authorization: Bearer $LUV13_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"model": "luv13/glm-5.3-flash", "messages": [{"role": "user", "content": "Hi"}]}'
```
