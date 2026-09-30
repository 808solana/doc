---
title: Few-Shot Prompting
definition: Few-shot prompting means showing a model a few worked examples in the prompt so it copies the pattern for a new input.
category: general
author: Quill
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- You put two to five example inputs and outputs before the real input.
- The model picks up the format, style and labels from the examples.
- It needs no training. The examples live only in that request.
- Every example adds input tokens, so keep them short.
- The idea was made widely known by the 2020 GPT-3 paper "Language Models are Few-Shot Learners" (Brown and others).

## How to do it

In a chat API, the cleanest way is to write each example as a pair of messages: a `user` message with the example input, then an `assistant` message with the ideal output. Then add the real input as the last `user` message.

Tips:

- **Make examples match the real task.** Same length, same kind of input.
- **Cover the edge cases.** If some inputs should get "unknown", include one.
- **Vary them.** If every example has the same answer, the model may just repeat it.
- **Keep the format exact.** The model copies small details, like punctuation and capital letters.

If the model does well with no examples at all, you may not need them. See [Zero-Shot Prompting](/docs/z/zero-shot-prompting).

## Example

This teaches a simple sentiment label with two examples.

```bash
curl https://api.luv13.ai/v1/chat/completions \
  -H "Authorization: Bearer $LUV13_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "luv13/glm-5.3-flash",
    "messages": [
      {"role": "system", "content": "Label each review as positive, negative or mixed. Reply with the label only."},
      {"role": "user", "content": "Fast shipping and it works great."},
      {"role": "assistant", "content": "positive"},
      {"role": "user", "content": "Nice screen, but the battery dies by noon."},
      {"role": "assistant", "content": "mixed"},
      {"role": "user", "content": "It broke on day two."}
    ]
  }'
```
