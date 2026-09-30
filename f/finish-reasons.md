---
title: Finish Reasons
definition: A finish reason is the value in choices[0].finish_reason of a luv13 chat completion that tells you why the model stopped writing.
category: luv13
author: Ink
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- Every choice in an OpenAI-format chat completion has a `finish_reason`.
- `stop` means the model finished on its own or hit one of your `stop` strings.
- `length` means the reply was cut off by `max_tokens` or the context limit. The text is incomplete.
- `tool_calls` means the model wants your code to run a function.
- Check it on every response, especially before parsing JSON.

<!-- TODO: verify with a real authenticated luv13 response which finish_reason values each model returns. -->

## The values

These are the OpenAI values. luv13 uses the OpenAI format, but which values each model returns is unconfirmed.

| Value | Meaning | What to do |
|---|---|---|
| `stop` | Natural end, or a `stop` string was hit | Use the reply |
| `length` | Ran out of room | Raise `max_tokens`, shorten the prompt, or ask the model to continue |
| `tool_calls` | The model is requesting a function call | Run it and send the result back; see [Tool Calling on luv13](/docs/t/tool-calling-on-luv13) |
| `content_filter` | Output was withheld by a filter | Rephrase the request |

## Where to find it

Without streaming, it's in `choices[0].finish_reason`. With streaming, it's `null` on every chunk except the last one, which carries the final value. See [Streaming on luv13](/docs/s/streaming-on-luv13).

## Example check

```python
choice = response.choices[0]
if choice.finish_reason == "length":
    print("Reply was cut off; raise max_tokens.")
```

A cut-off reply is still billed for the tokens it used, at $0.33 per 1M. Setting `max_tokens` too low wastes money on replies you can't use.

<!-- TODO: confirm with the operator that luv13 bills replies cut off at max_tokens (expected, since they aren't failed or empty calls). -->
