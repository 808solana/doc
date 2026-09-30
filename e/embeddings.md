---
title: Embeddings
definition: An embedding is a list of numbers that represents the meaning of a piece of text, so that similar texts get similar numbers.
category: general
author: Quill
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- An embedding model turns text into a vector, often hundreds or thousands of numbers long.
- Texts with similar meaning end up close together, even if they use different words.
- Embeddings power semantic search, clustering, recommendations and the retrieval step of RAG.
- Closeness is usually measured with cosine similarity.
- luv13 doesn't serve an embeddings endpoint right now. `POST /v1/embeddings` returned 404 on 2026-09-30.

## How they're used

1. Run each document chunk through an embedding model and store the vectors.
2. When a query comes in, embed it with the same model.
3. Find the stored vectors closest to the query vector.
4. Use those chunks, for example as context in a chat prompt.

Vectors from different embedding models aren't compatible. Embed your documents and your queries with the same model.

## Cosine similarity

Cosine similarity compares the direction of two vectors. It ranges from -1 to 1, and higher means more alike. In practice you compare scores against each other, not against a fixed cutoff, because the typical range depends on the model.

## Using embeddings with luv13

Because luv13 doesn't offer embeddings, you'd create them with a separate embedding model or service, then send the matching text to a luv13 chat model. See [Retrieval-Augmented Generation](/docs/r/retrieval-augmented-generation) for the full pattern and [Listing Models](/docs/l/listing-models) for what luv13 serves today.

You can check the current status yourself. A 404 means the endpoint isn't there:

```bash
curl -i https://api.luv13.ai/v1/embeddings \
  -H "Authorization: Bearer $LUV13_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"model": "luv13/glm-5.3-flash", "input": "hello"}'
```
