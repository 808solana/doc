---
title: Tool Calling
definition: Tool calling lets a model ask your code to run a function you described, then use the result in its reply.
description: How the tool calling loop works step by step, with tips for safe tools and a sample request to luv13.
category: general
author: Quill
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- You describe tools (name, purpose, JSON parameters) in the request's `tools` list.
- The model doesn't run anything. It replies with a `tool_calls` entry naming the tool and its arguments.
- Your code runs the tool and sends the result back in a message with the `tool` role.
- The model then writes its answer using that result.
- For what luv13 supports, see [Tool Calling on luv13](/docs/t/tool-calling-on-luv13).

## The loop

1. **You send** the conversation plus a list of tools, each with a JSON Schema for its arguments.
2. **The model decides.** If a tool would help, it returns an assistant message with `tool_calls` instead of plain text. Each call has an `id`, the function `name` and `arguments` as a JSON string.
3. **You run it.** Parse the arguments, call your real function, and capture the output.
4. **You reply** with a message of role `tool`, the matching `tool_call_id`, and the output as `content`.
5. **The model answers**, or asks for another tool. Repeat until it replies with text.

## Tips

- Write clear tool descriptions. The model picks tools based on them.
- Always validate the arguments. The model can produce missing or wrong values.
- Never let a tool do something risky, like deleting data or spending money, without a check in your own code.
- Coding tools such as [Roo Code](/docs/r/roo-code) depend on tool calling, so a model's tool support matters when you choose one.

## Example

This request offers one tool. Check [Tool Calling on luv13](/docs/t/tool-calling-on-luv13) to confirm which models and options luv13 supports.

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
        "description": "Get the current weather for a city.",
        "parameters": {
          "type": "object",
          "properties": {"city": {"type": "string"}},
          "required": ["city"]
        }
      }
    }]
  }'
```

If the model wants the tool, the reply's `choices[0].message.tool_calls` holds the call. `get_weather` here is only an example. You write the real function.
