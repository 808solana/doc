---
title: Agents
definition: An AI agent is a program that lets a model work toward a goal over several steps, choosing and using tools and checking the results as it goes.
category: general
author: Quill
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- An agent runs a loop: the model decides the next step, the program runs it, and the result goes back to the model.
- Tools are what let an agent act, such as reading files, running tests or searching. See [Tool Calling](/docs/t/tool-calling).
- Coding agents like [Aider](/docs/a/aider), [Roo Code](/docs/r/roo-code) and [OpenCode](/docs/o/opencode) can use luv13 as their model.
- Agents use many requests and a lot of tokens per task, because the growing history is sent each step.
- Give agents clear limits: which tools they have, how many steps they can take, and what needs your approval.

## The loop

1. **Goal.** You give the task: "Fix the failing test in `utils.py`."
2. **Think and choose.** The model reads the context and picks an action, often a tool call.
3. **Act.** The program runs the tool and captures the output.
4. **Observe.** The output is added to the conversation.
5. **Repeat** until the model says it's done or a step limit is hit.

## What makes agents work well

- **A model with solid tool calling.** Some agents need it to work at all.
- **Good tools.** Small, well-described tools with clear errors.
- **Feedback.** Tests, linters and type checkers give the agent a way to check its own work.
- **Guardrails.** Ask before risky actions, like deleting files, running shell commands or spending money.

## Cost and context

Each step re-sends the conversation, so input tokens grow fast over a long task. On luv13 input and output cost the same per token (see [Pricing](/docs/pricing)), so the total token count is what drives cost. Watch `usage` and cap the number of steps. See [Conversation History](/docs/c/conversation-history).

## Example

This is one step of an agent loop: the model gets a tool and decides whether to call it.

```bash
curl https://api.luv13.ai/v1/chat/completions \
  -H "Authorization: Bearer $LUV13_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "luv13/glm-5.3-flash",
    "messages": [{"role": "user", "content": "How many files are in the src folder?"}],
    "tools": [{
      "type": "function",
      "function": {
        "name": "list_files",
        "description": "List files in a folder.",
        "parameters": {"type": "object", "properties": {"path": {"type": "string"}}, "required": ["path"]}
      }
    }]
  }'
```

Your program would run `list_files`, send the result back, and call the API again. See [Tool Calling on luv13](/docs/t/tool-calling-on-luv13) for what luv13 supports.
