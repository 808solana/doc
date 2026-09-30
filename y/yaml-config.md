---
title: YAML Config
definition: A YAML config is a settings file written in YAML, a plain-text format that many AI tools use to store provider, model and key settings.
description: YAML basics for AI tool settings, with luv13 examples for Continue's config.yaml and Aider's .aider.conf.yml.
category: general
author: Quill
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- YAML uses indentation and `key: value` pairs. It's easy to read and edit by hand.
- AI tools such as Continue (`config.yaml`) and Aider (`.aider.conf.yml`) store model settings this way.
- To use luv13, the file usually needs a base URL, a model id and a way to get your key.
- Indentation matters. Use spaces, never tabs.
- Keep real keys out of any YAML file you commit to git.

## The basics

```yaml
name: example
count: 3
enabled: true
models:
  - name: first
    tags: [fast, small]
```

- `key: value` sets a value.
- Indented lines belong to the key above them.
- A leading `- ` starts a list item.
- `#` starts a comment.
- Quote strings that contain `:` or `#`, or that start with special characters.

## luv13 in two real tools

**Continue** (`config.yaml`). The field names come from Continue's config reference.

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

See [Using Continue](/docs/u/using-continue) for details.

**Aider** (`.aider.conf.yml`). Aider's config keys match its command-line options.

```yaml
openai-api-base: https://api.luv13.ai/v1
model: openai/luv13/glm-5.3-flash
```

Aider reads the key from the `OPENAI_API_KEY` environment variable. See [Aider](/docs/a/aider).

## Common mistakes

- Tabs instead of spaces, or uneven indentation.
- A missing space after the colon (`model:x` instead of `model: x`).
- Committing a file with a real key. Use your tool's secret or environment variable support instead. See [Environment Variables](/docs/e/environment-variables).
