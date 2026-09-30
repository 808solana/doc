---
title: Tool Calling on luv13
definition: Tool calling lets a model ask your code to run a function; on luv13 it would be requested in the OpenAI format, and per-model support isn't published yet.
description: What is and isn't known about tool calling on luv13, with links to the general guide and the tools that depend on it.
category: luv13
author: Ink
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- In the OpenAI chat completions format, you describe functions in the `tools` field of `POST https://api.luv13.ai/v1/chat/completions`.
- luv13 hasn't published which of its seven models support tool calling.
- Coding agents such as Cline rely on tool calling. If one misbehaves on luv13, try another model id.
- The price is the same flat $0.33 per 1M tokens on every model.

<!-- TODO: fill in from the operator's answer on tool-calling support per model, and verify a tool-call round trip with a real key before adding an example. -->

## How tool calling works in general

See [Tool Calling](/docs/t/tool-calling) for the OpenAI format and the request, call and result round trip, and [Agents](/docs/a/agents) for how coding tools use it.

## Related

- [Compatible Tools](/docs/c/compatible-tools)
- [Model IDs](/docs/m/model-ids)
