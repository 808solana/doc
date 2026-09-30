---
title: Streaming on luv13
definition: Streaming on luv13 means setting stream to true on a chat completion so the reply arrives in small pieces as the model writes it, instead of all at once.
category: luv13
author: Ink
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- Add `"stream": true` to a `POST https://api.luv13.ai/v1/chat/completions` request.
- In the OpenAI format, the reply comes back as server-sent events: lines that start with `data: `, each holding a JSON chunk, ending with `data: [DONE]`.
- Each chunk carries a small piece of text in `choices[0].delta.content`. Join them in order to get the full reply.
- Streaming doesn't change the price. You pay the same $0.33 per 1M tokens either way.
- luv13 hasn't confirmed which models support streaming. Test before you depend on it.

<!-- TODO: confirm with the operator that streaming works on all seven models, and run the curl below with a real key to verify the chunk format. -->

## Example

`-N` stops curl from buffering, so you see chunks as they arrive:

```bash
curl -N https://api.luv13.ai/v1/chat/completions \
  -H "Authorization: Bearer $LUV13_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "luv13/glm-5.3-flash",
    "messages": [{"role": "user", "content": "Count from 1 to 5."}],
    "stream": true
  }'
```

With the OpenAI Python SDK (`pip install openai`):

```python
import os
from openai import OpenAI

client = OpenAI(base_url="https://api.luv13.ai/v1", api_key=os.environ["LUV13_API_KEY"])

stream = client.chat.completions.create(
    model="luv13/glm-5.3-flash",
    messages=[{"role": "user", "content": "Count from 1 to 5."}],
    stream=True,
)
for chunk in stream:
    if chunk.choices and chunk.choices[0].delta.content:
        print(chunk.choices[0].delta.content, end="", flush=True)
```

## Why stream

- The first words show up sooner, which matters in chat interfaces and coding tools.
- A long reply is less likely to hit a client or proxy timeout while you wait.

## Token counts

In the OpenAI format, a streamed response only includes `usage` if you ask for it with `"stream_options": {"include_usage": true}`; it then arrives in the last chunk.

<!-- TODO: confirm luv13 honors stream_options.include_usage, and whether a stream the client closes early is charged for the tokens produced so far. -->

## If it fails

Errors such as a 401 come back before any stream starts, as a normal JSON body. See [Errors and Status Codes](/docs/e/errors-and-status-codes).
