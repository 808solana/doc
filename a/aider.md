---
title: Aider
definition: Aider is an open-source AI pair-programming tool for the terminal that can use luv13 as an OpenAI-compatible endpoint.
category: general
author: Quill
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- Aider reads the endpoint from `OPENAI_API_BASE` and the key from `OPENAI_API_KEY`.
- Set `OPENAI_API_BASE` to `https://api.luv13.ai/v1` and `OPENAI_API_KEY` to your luv13 key.
- Put `openai/` in front of the model id, so luv13's `luv13/glm-5.3-flash` becomes `openai/luv13/glm-5.3-flash`.
- Aider may warn that it doesn't know the model's settings. That's expected for models outside its built-in list.
- These names come from Aider's official docs on OpenAI-compatible APIs.

## Setup

Install Aider using the method in its docs, for example:

```bash
python -m pip install aider-install
aider-install
```

Set the endpoint and key. On macOS or Linux:

```bash
export OPENAI_API_BASE=https://api.luv13.ai/v1
export OPENAI_API_KEY=$LUV13_API_KEY
```

On Windows, use `setx OPENAI_API_BASE https://api.luv13.ai/v1` and `setx OPENAI_API_KEY <your key>`, then open a new terminal.

Then start Aider inside your project:

```bash
cd /path/to/your/project
aider --model openai/luv13/glm-5.3-flash
```

## Using a config file

Aider can also read its settings from a `.aider.conf.yml` file:

```yaml
openai-api-base: https://api.luv13.ai/v1
model: openai/luv13/glm-5.3-flash
```

Keep the key in the environment instead of this file. See [YAML Config](/docs/y/yaml-config).

## About the model warning

Aider keeps details, such as context size, for models it knows. For other models it shows a warning and uses defaults. luv13's model list doesn't publish context lengths, so there's no official value to add. See [Context Window](/docs/c/context-window).

## Check your settings first

```bash
curl https://api.luv13.ai/v1/chat/completions \
  -H "Authorization: Bearer $LUV13_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"model": "luv13/glm-5.3-flash", "messages": [{"role": "user", "content": "Hi"}]}'
```
