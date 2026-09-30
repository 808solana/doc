---
title: LangChain
definition: LangChain is a framework for building LLM apps whose ChatOpenAI class can call luv13 by setting base_url.
category: general
author: Quill
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- Install the `langchain-openai` package and use its `ChatOpenAI` class.
- Pass `base_url="https://api.luv13.ai/v1"`, your luv13 key as `api_key`, and a luv13 model id such as `luv13/glm-5.3-flash`.
- `ChatOpenAI` also reads the `OPENAI_API_BASE` environment variable if you don't pass `base_url`.
- Use it for chat. `OpenAIEmbeddings` won't work, because luv13 doesn't serve embeddings.
- These names come from the `langchain-openai` source on GitHub.

## Install

```bash
pip install langchain-openai
```

## Example

```python
import os
from langchain_openai import ChatOpenAI

llm = ChatOpenAI(
    model="luv13/glm-5.3-flash",
    base_url="https://api.luv13.ai/v1",
    api_key=os.environ["LUV13_API_KEY"],
)

reply = llm.invoke("Explain a context window in one sentence.")
print(reply.content)
```

## Where the base URL comes from

LangChain picks the base URL in this order:

1. The `base_url` argument (also accepted as `openai_api_base`).
2. The `OPENAI_API_BASE` environment variable.
3. The `OPENAI_BASE_URL` environment variable, read by the underlying OpenAI SDK.

Passing `base_url` directly is the clearest option, and it avoids sending requests to OpenAI by accident.

## Notes

- `ChatOpenAI` is built on the [OpenAI SDKs](/docs/u/using-the-openai-sdks), so the same luv13 rules apply: chat completions and model listing are the endpoints to use. See [Endpoints](/docs/e/endpoints).
- For retrieval apps, pair a luv13 chat model with embeddings from another service. See [Retrieval-Augmented Generation](/docs/r/retrieval-augmented-generation).
- Tool calling through LangChain depends on luv13's tool support. See [Tool Calling on luv13](/docs/t/tool-calling-on-luv13).
