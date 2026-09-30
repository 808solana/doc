---
title: Roo Code
definition: Roo Code is an AI coding agent for VS Code that can use luv13 through its OpenAI Compatible provider.
category: general
author: Quill
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- In Roo Code's settings, set **API Provider** to **OpenAI Compatible**.
- Set **Base URL** to `https://api.luv13.ai/v1`, paste your luv13 key into **API Key**, and pick a model id such as `luv13/glm-5.3-flash`.
- Roo Code uses native tool calling only. A model that can't do OpenAI-style tool calls won't work with it.
- Settings names on this page come from Roo Code's official docs.

## Setup

1. Install the Roo Code extension in VS Code.
2. Open Roo Code's settings panel.
3. Set **API Provider** to **OpenAI Compatible**.
4. Fill in:
   - **Base URL:** `https://api.luv13.ai/v1`
   - **API Key:** your luv13 key
   - **Model:** `luv13/glm-5.3-flash`, or another id from the live list
5. Save and start a task.

## Tool calling matters here

Roo Code sends its tools using OpenAI's `tools` format and expects the model to answer with tool calls. If a model or provider doesn't fully support that, Roo Code shows tool-calling errors. Check [Tool Calling on luv13](/docs/t/tool-calling-on-luv13) for which luv13 models are confirmed, and test a short task first. For the idea in general, see [Tool Calling](/docs/t/tool-calling).

## Model Configuration

Roo Code lets you set max output tokens, context window, image support and prices.

- **Prices:** luv13 is a flat $0.33 per 1M tokens on every model, input the same as output. See [Pricing](/docs/p/pricing).
- **Context window:** luv13 doesn't publish context lengths in its model list, so there's no official number to enter.

## Check your settings first

This checks the key, base URL and model with a tool attached, which is close to what Roo Code sends:

```bash
curl https://api.luv13.ai/v1/chat/completions \
  -H "Authorization: Bearer $LUV13_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "luv13/glm-5.3-flash",
    "messages": [{"role": "user", "content": "Read the file README.md."}],
    "tools": [{
      "type": "function",
      "function": {
        "name": "read_file",
        "description": "Read a file from the workspace.",
        "parameters": {"type": "object", "properties": {"path": {"type": "string"}}, "required": ["path"]}
      }
    }]
  }'
```

If the reply contains `tool_calls`, the model is using the tool as Roo Code expects.
