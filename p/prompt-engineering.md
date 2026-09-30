---
title: Prompt Engineering
definition: Prompt engineering is the practice of writing and testing the instructions you give a model so it reliably produces the output you want.
category: general
author: Quill
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- A clear, specific prompt usually beats a clever one.
- Give the model context, the task, the format you want, and any limits.
- Examples in the prompt ([Few-Shot Prompting](/docs/f/few-shot-prompting)) are one of the strongest tools you have.
- Treat prompts like code: change one thing at a time and test on real inputs.
- The same prompt can behave differently on different models, so test on the luv13 model you'll ship with.

## The basics

Most good prompts cover four things:

1. **Context.** Who is the audience? What does the model need to know?
2. **Task.** What exactly should it do? Use a verb: summarize, classify, rewrite, extract.
3. **Format.** How should the answer look? A list, a table, JSON, a single word?
4. **Limits.** Length, tone, and what to do when it isn't sure.

Rules that apply to every turn belong in the [System Prompts](/docs/s/system-prompts). The specific task goes in the user message.

## Techniques worth knowing

- **Zero-shot:** just ask. See [Zero-Shot Prompting](/docs/z/zero-shot-prompting).
- **Few-shot:** show a few input and output pairs first. See [Few-Shot Prompting](/docs/f/few-shot-prompting).
- **Delimiters:** wrap documents and data in tags so the model can tell them apart from instructions. See [XML Prompts](/docs/x/xml-prompts).
- **Step by step:** ask the model to reason before it answers. See [Chain-of-Thought Prompting](/docs/c/chain-of-thought-prompting).
- **Grounding:** give the model source text and tell it to answer only from that. See [Retrieval-Augmented Generation](/docs/r/retrieval-augmented-generation).

## Testing prompts

Keep a small set of real inputs, including hard and odd ones. Each time you change the prompt, run the whole set and compare. Settings like [Temperature](/docs/t/temperature) also change results, so keep them fixed while you test wording.

## Example

```bash
curl https://api.luv13.ai/v1/chat/completions \
  -H "Authorization: Bearer $LUV13_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "luv13/glm-5.3-flash",
    "messages": [
      {"role": "system", "content": "You write release notes for developers. Be brief and concrete."},
      {"role": "user", "content": "Summarize this change as one bullet under 20 words: Added retry with exponential backoff to the upload client."}
    ]
  }'
```
