---
title: Model IDs
definition: A model ID is the exact string, such as luv13/kimi-k3, that you put in the model field of a luv13 request to choose which model answers.
description: All seven luv13 model ids beside their display names, plus the prefix, dot and hyphen mistakes that break requests.
category: luv13
author: Ink
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- Every luv13 model id starts with `luv13/`, followed by a lowercase name with hyphens and dots.
- The id isn't the display name. "GLM-5.3 Flash" is `luv13/glm-5.3-flash`, and "DeepSeek V4-Pro" is `luv13/deepseek-v4-pro`.
- Copy ids from `GET /v1/models`. Don't retype them.
- There are seven ids as of 2026-09-30.

The main page for luv13 models is [Models](/docs/models); this page covers the format of model ids and the mistakes that break them.

## The seven ids

From live `GET /v1/models` on 2026-09-30, with the display names from luv13.ai/models:

| Id | Display name |
|---|---|
| `luv13/deepseek-v4-pro` | [DeepSeek V4-Pro](/docs/m/deepseek-v4-pro) |
| `luv13/deepseek-v4.1-flash` | [DeepSeek V4.1 Flash](/docs/m/deepseek-v4-1-flash) |
| `luv13/glm-5.3` | [GLM 5.3](/docs/m/glm-5-3) |
| `luv13/glm-5.3-flash` | [GLM-5.3 Flash](/docs/m/glm-5-3-flash) |
| `luv13/kimi-k3` | [Kimi K3](/docs/m/kimi-k3) |
| `luv13/kimi-k3-fast` | [Kimi K3 Fast](/docs/m/kimi-k3-fast) |
| `luv13/qwen-3.8-27b` | [Qwen 3.8 27B](/docs/m/qwen-3-8-27b) |

## Easy mistakes

- **Dropping the prefix.** Use `luv13/kimi-k3`, not `kimi-k3`.
- **Swapping dots and hyphens.** It's `deepseek-v4.1-flash` (dot in the version) but `deepseek-v4-pro` (no dot).
- **Capital letters.** Every id is lowercase.
- **Using an id from another provider.** Only the seven `luv13/` ids are listed; use one of them.

<!-- TODO: confirm with the operator what status and body luv13 returns for an unknown model id, and whether ids are matched case-sensitively. -->

## Get the current list

```bash
curl -s https://api.luv13.ai/v1/models | jq -r '.data[].id'
```

Some tools want the model name in a settings box. Paste the full id, including `luv13/`.

Each display name links to that model's page. The model list is also at [Models](/docs/models). Price is on [Pricing](/docs/p/pricing).
