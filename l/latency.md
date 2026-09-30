---
title: Latency
definition: Latency is how long you wait for a model's response, often measured as the time to the first token and the time to the full reply.
category: general
author: Quill
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- Time to first token (TTFT) is how long before any text arrives. It's what users notice most.
- Total time depends mostly on how many output tokens the model writes.
- Long prompts add time before the first token, because the model has to read them first.
- Smaller or "fast" models are usually quicker. [Reasoning Models](/docs/r/reasoning-models) are usually slower.
- [Streaming](/docs/s/streaming) doesn't make the reply finish sooner, but it shows text much earlier.

## Where the time goes

1. **Network:** your request reaching the API and the reply coming back.
2. **Queueing:** waiting for capacity if the service is busy.
3. **Reading the prompt:** the model processes every input token. Longer prompts take longer.
4. **Writing the reply:** tokens come out one after another, so a 1,000-token answer takes much longer than a 50-token one.

The last step is often the biggest. That's why output length matters so much. See [Throughput](/docs/t/throughput) for the speed of that step.

## Ways to cut it

- Stream the reply so users see progress.
- Ask for shorter answers, and set `max_tokens`.
- Trim the prompt: drop old chat turns and unneeded context.
- Pick a faster model for simple tasks. luv13's list includes ids like `luv13/glm-5.3-flash` and `luv13/kimi-k3-fast`, but test the speed yourself. Names are a hint, not a promise.
- Send independent requests in parallel instead of one after another, within your [rate limits](/docs/r/rate-limiting).

## Measuring it

curl can report timing for a request:

```bash
curl -s -o /dev/null \
  -w "first byte: %{time_starttransfer}s, total: %{time_total}s\n" \
  https://api.luv13.ai/v1/chat/completions \
  -H "Authorization: Bearer $LUV13_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"model": "luv13/glm-5.3-flash", "messages": [{"role": "user", "content": "Hi"}], "max_tokens": 20}'
```

For a non-streamed request, "first byte" is close to the full time. Add `"stream": true` and `-N` to measure time to first token instead.
