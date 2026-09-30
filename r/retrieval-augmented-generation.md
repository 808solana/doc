---
title: Retrieval-Augmented Generation
definition: Retrieval-augmented generation (RAG) means looking up relevant text first and adding it to the prompt so the model answers from that source.
category: general
author: Quill
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- RAG has two steps: retrieve the passages that match a question, then generate an answer using them.
- It lets a model answer about your own documents without retraining it.
- It cuts down on [Hallucinations](/docs/h/hallucinations), because the model has the facts in front of it.
- Retrieval is often done with embeddings and a vector search, but keyword search works too.
- The name comes from a 2020 paper by Lewis and others, "Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks."

## How it works

1. **Prepare.** Split your documents into chunks, such as a few paragraphs each, and index them.
2. **Retrieve.** When a question comes in, search the index and take the top few matching chunks.
3. **Augment.** Put those chunks into the prompt, clearly marked, with the question.
4. **Generate.** Ask the model to answer only from the provided text, and to say so when the answer isn't there.

For step 2 you can use keyword search, a vector search over [Embeddings](/docs/e/embeddings), or both.

## Tips

- **Chunk size matters.** Too small and chunks lose meaning. Too large and you waste the [Context Window](/docs/c/context-window).
- **Label sources.** Give each chunk an id so the model can cite it and you can check it.
- **Retrieval is the weak point.** If the right chunk isn't found, the model can't use it. Test search quality on its own.

## On luv13

luv13 handles the generate step through [Chat Completions](/docs/c/chat-completions). It didn't serve an embeddings endpoint when this page was written (checked 2026-09-30), so the retrieve step needs your own search or another service.

## Example

This is the generate step, with two retrieved chunks already pasted in.

```bash
curl https://api.luv13.ai/v1/chat/completions \
  -H "Authorization: Bearer $LUV13_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "luv13/glm-5.3-flash",
    "messages": [
      {"role": "system", "content": "Answer only from the sources. Cite the source id. If the answer is missing, say so."},
      {"role": "user", "content": "<source id=\"1\">Refunds are issued within 14 days.</source>\n<source id=\"2\">Shipping is free over $50.</source>\n\nQuestion: How long do refunds take?"}
    ]
  }'
```
