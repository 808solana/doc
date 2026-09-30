---
title: Tool Calling on luv13
definition: Tool calling on luv13 means describing your functions in a chat completion request so the model can reply with a structured request to call one, instead of plain text.
category: luv13
author: Ink
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- You describe functions in the `tools` field of a `POST https://api.luv13.ai/v1/chat/completions` request, in the OpenAI format.
- The model doesn't run anything. It returns `tool_calls` with a function name and JSON arguments, and your code runs the function.
- You send the result back as a message with `role: "tool"`, and the model uses it in its next reply.
- The tool definitions are sent as input tokens on every request, so they cost $0.33 per 1M like the rest of the prompt.
- luv13 hasn't confirmed which models support tool calling. Test before you depend on it.

<!-- TODO: confirm with the operator which of the seven models support tools and tool_choice, and verify the curl below with a real key. -->

## Example request

```bash
curl https://api.luv13.ai/v1/chat/completions \
  -H "Authorization: Bearer $LUV13_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "luv13/glm-5.3-flash",
    "messages": [{"role": "user", "content": "What is the weather in Phoenix?"}],
    "tools": [{
      "type": "function",
      "function": {
        "name": "get_weather",
        "description": "Get the current weather for a city",
        "parameters": {
          "type": "object",
          "properties": {"city": {"type": "string"}},
          "required": ["city"]
        }
      }
    }]
  }'
```

## The round trip

In the OpenAI format:

1. The model replies with `choices[0].message.tool_calls`, a list where each item has an `id`, a `function.name` and `function.arguments` (a JSON string). `finish_reason` is `tool_calls`.
2. Your code parses the arguments and runs the function.
3. You send a new request with the whole conversation: the original messages, the assistant message that holds `tool_calls`, and one `{"role": "tool", "tool_call_id": "...", "content": "..."}` message per call.
4. The model answers in plain text using the result.

Always validate the arguments before running anything. The model can produce arguments that don't match your schema.

## Why coding tools need it

Agents such as Cline and Claude Code rely on tool calling to read files and run commands. If one of them stalls or replies in plain text where it should act, try another luv13 model. See [Compatible Tools](/docs/c/compatible-tools).
