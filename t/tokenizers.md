---
title: Tokenizers
definition: A tokenizer is the part of a language model system that splits text into tokens and turns them into numbers the model can read.
category: general
author: Quill
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- Every model uses a tokenizer before it reads your text and after it writes its reply.
- Most modern tokenizers use subword methods, such as byte-pair encoding (BPE), so common words are one token and rare words are split into pieces.
- Each model family has its own tokenizer, so the same text can be a different number of tokens on different models.
- Token counts drive cost and length limits, so the tokenizer affects both.
- On luv13, the `usage` field in each response gives the real count for that model.

## What a tokenizer does

1. **Splits** the text into pieces from a fixed vocabulary, often tens of thousands of entries.
2. **Maps** each piece to a number (its token id).
3. **Reverses** the process on the way out, turning the model's token ids back into text.

Subword tokenizers are built from a large sample of text. Pieces that show up often get their own entry. That's why everyday English is compact, while code, numbers, rare names and some other languages can use more tokens for the same length.

## Why it matters to you

- **Cost.** You pay per token. See [What Is a Token](/docs/w/what-is-a-token).
- **Limits.** The [Context Window](/docs/c/context-window) is counted in tokens, not characters or words.
- **Odd behavior.** Tasks like counting letters in a word can trip up a model, because it sees tokens, not single letters.

## Counting tokens

Local token counters are only accurate for the tokenizer they were built for. The simplest reliable way to count on luv13 is to send the request and read `usage`:

```bash
curl https://api.luv13.ai/v1/chat/completions \
  -H "Authorization: Bearer $LUV13_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "luv13/glm-5.3-flash",
    "messages": [{"role": "user", "content": "Tokenization splits text into pieces."}],
    "max_tokens": 1
  }'
```

`usage.prompt_tokens` in the reply is how many tokens your message took on that model. Setting `max_tokens` to 1 keeps the reply, and its cost, tiny.
