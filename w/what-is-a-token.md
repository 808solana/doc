---
title: What Is a Token
definition: A token is a small chunk of text, often a word or part of a word, that a language model reads and writes one at a time.
category: general
author: Quill
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- Models don't see letters or whole sentences. They see text split into tokens.
- In English, one token is often a short word or a piece of a longer word. Spaces and punctuation count too.
- Both the text you send (input) and the text the model writes back (output) are measured in tokens.
- APIs like luv13 charge by the number of tokens, usually quoted per 1 million tokens.
- A model can only handle so many tokens in one request. That limit is its context window.

## How text becomes tokens

Before a model reads your prompt, a tokenizer splits it into tokens and turns each one into a number. A common word like "the" is usually a single token. A rarer or longer word, such as "tokenization", may be split into several pieces. Numbers, code and non-English text often take more tokens than you'd expect for their length.

Each model family has its own tokenizer, so the same sentence can come out as a different number of tokens on different models.

## Why tokens matter

Tokens decide two practical things:

1. **Cost.** You pay for the tokens you send and the tokens you get back. On luv13 the price is a flat $0.33 per 1 million tokens on every model, with input and output at the same rate. See [Pricing](/docs/pricing).
2. **Length.** Every model has a maximum number of tokens it can handle in one request, counting both your prompt and its reply. See [Context Window](/docs/c/context-window).

## Seeing your token count

An OpenAI-compatible API reports tokens in the `usage` field of each response:

```json
"usage": {
  "prompt_tokens": 12,
  "completion_tokens": 30,
  "total_tokens": 42
}
```

The numbers above are an example. `prompt_tokens` is what you sent, `completion_tokens` is what the model wrote, and `total_tokens` is the two added together. See [Input vs. Output Tokens](/docs/i/input-vs-output-tokens) for more.
