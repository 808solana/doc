---
title: Hallucinations
definition: A hallucination is when a model states something false or made up as if it were true.
description: Why models state false things with confidence, and practical ways to reduce made-up answers when building on luv13.
category: general
author: Quill
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- Models predict likely text. Likely-sounding text isn't always true.
- Hallucinations often look confident: fake quotes, wrong dates, made-up links, or functions that don't exist.
- Giving the model the source text and asking it to answer only from that cuts them down a lot.
- Letting the model say "I don't know" helps.
- For anything that matters, check facts in code or with a person.

## Why they happen

A language model learns patterns from text, not a list of checked facts. When it doesn't know something, it can still produce a fluent answer that fits the pattern. It has no built-in sense of which of its statements are true.

Some things make hallucinations more likely:

- Questions about rare, recent or very specific facts.
- Requests for exact numbers, citations or URLs.
- Pushing the model to answer when it should decline.
- High [Temperature](/docs/t/temperature) settings.

## How to reduce them

- **Ground the answer.** Paste the relevant source and say "Answer only from the text below." See [Retrieval-Augmented Generation](/docs/r/retrieval-augmented-generation).
- **Allow uncertainty.** Add "If the answer isn't in the text, say you don't know."
- **Ask for quotes.** Have the model quote the lines it relied on, then check that they're really in the source.
- **Use tools.** Let the model look things up or run code instead of guessing. See [Tool Calling](/docs/t/tool-calling).
- **Check outputs.** Validate code by running it, and check links and numbers before you publish them.

## Example

```bash
curl https://api.luv13.ai/v1/chat/completions \
  -H "Authorization: Bearer $LUV13_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "luv13/glm-5.3-flash",
    "messages": [
      {"role": "system", "content": "Answer only from the provided text. If the answer is not there, reply: I do not know."},
      {"role": "user", "content": "Text: The library opens at 9 a.m. on weekdays.\n\nQuestion: What time does it open on Sunday?"}
    ]
  }'
```

A good reply here is "I do not know", because the text doesn't say.
