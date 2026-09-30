---
title: Using Continue
definition: Continue is an open-source AI coding assistant for VS Code and JetBrains that can use luv13 through its openai provider with a custom apiBase.
description: Add a luv13 model to Continue's config.yaml for chat and edits, and why autocomplete should use a different model.
category: general
author: Quill
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- Continue's models are set in its `config.yaml` file.
- Use `provider: openai`, set `apiBase` to `https://api.luv13.ai/v1`, and set `model` to a luv13 id.
- Give luv13 models the `chat`, `edit` and `apply` roles.
- Leave luv13 out of Continue's `autocomplete` role. Autocomplete may need endpoints other than chat completions, and chat completions is the only generation endpoint luv13 serves.
- Field names here come from Continue's official config reference.

## Setup

1. Install Continue in VS Code or a JetBrains IDE.
2. Open Continue's config file (`config.yaml`).
3. Add a model entry:

```yaml
name: My Config
version: 1.0.0
schema: v1
models:
  - name: luv13 GLM 5.3 Flash
    provider: openai
    model: luv13/glm-5.3-flash
    apiBase: https://api.luv13.ai/v1
    apiKey: <YOUR_LUV13_API_KEY>
    roles:
      - chat
      - edit
      - apply
```

4. Replace `<YOUR_LUV13_API_KEY>` with your key, save, and pick the model in Continue's model menu.

Don't commit this file with a real key in it. See [API Key Best Practices](/docs/a/api-key-best-practices).

## Useful options

Continue's config reference lists more fields you can add to a model:

- `defaultCompletionOptions` with `temperature`, `maxTokens`, `topP` and `stop`. Whether luv13 honors each one is covered on [Request Parameters](/docs/r/request-parameters).
- `capabilities` with `tool_use` to tell Continue the model can call tools, which agent mode needs.
- `requestOptions` with `timeout` and `headers`.

## Why not autocomplete

Tab autocomplete works differently from chat, and Continue's docs include a setting (`useLegacyCompletionsEndpoint`) for models that should use the older `/completions` endpoint. luv13 serves `POST /v1/chat/completions` and returns 404 for `/v1/completions`. See [Endpoints](/docs/e/endpoints). The safe setup is luv13 for chat and edits, and a separate model for autocomplete.

## Check your settings first

```bash
curl https://api.luv13.ai/v1/chat/completions \
  -H "Authorization: Bearer $LUV13_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"model": "luv13/glm-5.3-flash", "messages": [{"role": "user", "content": "Hi"}]}'
```
