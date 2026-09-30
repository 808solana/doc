---
title: Throughput
definition: Throughput is how much work a model or API gets done over time, usually measured in output tokens per second.
description: How output speed in tokens per second differs from latency, how to measure it, and ways to raise overall app throughput.
category: general
author: Quill
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- For one reply, throughput is how fast tokens stream out, in tokens per second.
- For a whole app, it's how many requests or tokens you can process per minute.
- Higher throughput means long answers finish sooner.
- Model size, service load and hardware all affect it.
- Running requests in parallel raises total throughput, up to your [rate limits](/docs/r/rate-limiting).

## Throughput vs. latency

[Latency](/docs/l/latency) is how long you wait. Throughput is how fast work gets done once it's moving. A model can start quickly (low time to first token) but write slowly, or the other way around. For short replies, latency matters most. For long ones, throughput does.

As an example, at 50 output tokens per second, a 500-token answer takes about 10 seconds to write, plus the time to the first token.

## Measuring it

1. Send a request and note the time.
2. Note when the first token arrives and when the last one does.
3. Divide `usage.completion_tokens` by the time between first and last token.

Run it a few times and at different times of day. Numbers vary with load.

## Raising app throughput

- Run several requests at once instead of one after another.
- Keep prompts short so each request uses fewer resources.
- Use a faster model for bulk or simple work.
- Batch small tasks into one prompt when the answers are short and independent.

## Example

This streams a longer reply so you can watch the pace:

```bash
curl -N https://api.luv13.ai/v1/chat/completions \
  -H "Authorization: Bearer $LUV13_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "luv13/glm-5.3-flash",
    "messages": [{"role": "user", "content": "Write 200 words about rivers."}],
    "stream": true
  }'
```
