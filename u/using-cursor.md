---
title: Using Cursor
definition: Using Cursor with luv13 means pointing Cursor's OpenAI API key and base URL override at luv13 so local Chat and Agent run on a luv13 model.
description: Step-by-step Cursor setup for luv13 with the base URL override, what features it covers, and fixes for the errors people hit.
category: general
author: Quill
status: draft
last_checked: 2026-09-30
---

> **Compatibility: works with caveats.** Cursor can send its local Chat and Agent requests to luv13 through the OpenAI API key setting with **Override OpenAI Base URL** turned on. Tab, Auto, Cloud or Background Agents, Automations, the Cursor CLI and Cursor's API and SDK can't use your own key. The override applies to every OpenAI-family request while it's on. Cursor staff have also said Cursor sometimes sends Responses API requests, which luv13 doesn't serve.

## Key takeaways

- Set it up in **Cursor Settings > Models**: paste your luv13 key as the OpenAI API key, turn on **Override OpenAI Base URL**, enter `https://api.luv13.ai/v1`, and add `luv13/glm-5.3-flash` as a custom model.
- Only local Chat and Agent can use luv13. Tab completion and the other features listed above keep using Cursor's own models.
- Requests go from Cursor's servers to luv13, not straight from your computer, so your key travels through Cursor's backend with each request.
- Turn the override off when you want Cursor's built-in models again. While it's on, those requests go to luv13 too, and they fail because luv13 doesn't have them.
- Run the two curl checks below first. If they pass and Cursor still fails, the problem is on the Cursor side.

## Before you start

You need:

- The Cursor desktop app.
- A luv13 API key. The [Quickstart](/docs/q/quickstart) shows how to get one.
- The model id `luv13/glm-5.3-flash`, from the live list at `https://api.luv13.ai/v1/models`. See [GLM-5.3 Flash](/docs/m/glm-5-3-flash) for details on the model.

Facts about luv13 that affect this setup:

- luv13 serves one generation endpoint, `POST /v1/chat/completions`, plus `GET /v1/models`. `/v1/responses`, `/v1/messages`, `/v1/completions` and `/v1/embeddings` return 404 (checked 2026-09-30). See [Endpoints](/docs/e/endpoints).
- Every model costs a flat $0.33 per 1M tokens, input the same as output. See [Pricing](/docs/p/pricing).
- The model list at `https://api.luv13.ai/v1/models` doesn't report context length, so luv13 publishes no per-model limits. See the [model list](/docs/models).

## Test your key and model first

Run these two checks in a terminal before you touch the tool. They take a few seconds and rule out key and model problems.

**1. The model id exists.** This call needs no key:

```bash
curl -s https://api.luv13.ai/v1/models
```

The list should include `"id":"luv13/glm-5.3-flash"`.

**2. Your key works for chat.** Set the key in your shell first (`export LUV13_API_KEY="your luv13 key"`), then run:

```bash
curl -s https://api.luv13.ai/v1/chat/completions \
  -H "Authorization: Bearer $LUV13_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "luv13/glm-5.3-flash",
    "messages": [{"role": "user", "content": "Reply with the word ready."}],
    "max_tokens": 20
  }'
```

With a valid key you should get back a JSON chat completion whose `choices[0].message.content` holds the reply, plus a `usage` block. If you see `{"error":{"code":401,"message":"unauthorized","type":"invalid_auth"}}` instead, the key is missing or wrong, and no tool setting will fix that.

## Set up Cursor

These steps follow Cursor's "Bring your own API key" help page, OpenAI's help article on using OpenAI models in Cursor, and replies from Cursor staff on the Cursor forum, all read on 2026-09-30. Cursor's own docs don't document the base URL override step by step, so the labels in steps 4 and 5 may look a little different in your version.

1. Open **Cursor Settings** (the gear icon, or the command palette) and select **Models**.
2. Find the **OpenAI API Key** field.
3. Paste your luv13 key into it. Don't paste it anywhere else, such as a rules file or chat.
4. Turn on **Override OpenAI Base URL** and enter exactly:

   ```
   https://api.luv13.ai/v1
   ```

   Use no trailing slash and no `/chat/completions`. Cursor adds the path itself.
5. Save or verify the key.
6. Add a custom model with the name:

   ```
   luv13/glm-5.3-flash
   ```

   The name must match luv13's id exactly, including the `luv13/` prefix.
7. Make sure the new model is enabled, then pick it in the model picker of a local **Chat** or **Agent** session.
8. Send a short prompt such as "Reply with the word ready."

## What works and what doesn't

| Cursor feature | Uses luv13? |
|---|---|
| Local Chat | Yes |
| Local Agent | Yes, with caveats. Agent mode depends on tool calling. See [Tool Calling on luv13](/docs/t/tool-calling-on-luv13). |
| Tab completion | No. Cursor's docs say custom keys work only with chat models, and Tab keeps using Cursor's models. |
| Auto model selection | No |
| Cloud or Background Agents, Automations | No |
| Cursor CLI, Cursor API and SDK | No |

## Things to know

- **Requests go through Cursor's servers.** Cursor's docs say your key isn't stored on its servers, but it's sent to Cursor's backend with every request, because Cursor builds the final prompt there. So luv13 gets the call from Cursor, not from your computer.
- **Privacy.** Cursor's docs say its Zero Data Retention policy doesn't apply when you use your own key. How luv13 handles data is covered on [Data and Privacy](/docs/d/data-and-privacy).
- **Billing.** luv13 bills you for the tokens at its flat rate. On Cursor's Teams and Enterprise plans, Cursor's docs say its own per-token "Cursor Token Rate" still applies to requests made with your key. Check Cursor's pricing for your plan.
- **The override is global.** Cursor staff have confirmed that while **Override OpenAI Base URL** is on, all OpenAI-family requests go to the custom URL, even for models picked from Cursor's built-in list. The only workaround they give is to turn the override off when you use Cursor's models and back on for luv13.

## Common errors

| Symptom | Likely cause | Fix |
|---|---|---|
| `401` with `{"error":{"code":401,"message":"unauthorized","type":"invalid_auth"}}` | The key is missing, has extra spaces, or isn't a luv13 key. | Paste your luv13 key again. Run test 2 above to confirm it. |
| `404` with an HTML page titled "404 Not Found" | The URL is wrong. Common versions: `https://api.luv13.ai` with no `/v1`, a doubled path such as `https://api.luv13.ai/v1/v1/...`, a pasted full endpoint like `.../v1/chat/completions` in the base URL field, or a trailing slash. | Set the base URL to exactly `https://api.luv13.ai/v1`. The tool adds `/chat/completions` itself. |
| `404` even though the base URL is right | The tool is calling an endpoint luv13 doesn't serve, such as `/v1/responses`, `/v1/messages`, `/v1/completions` or `/v1/embeddings`. | Use the tool's OpenAI chat-completions mode, and turn off features that need other endpoints. See [Endpoints](/docs/e/endpoints). |
| Model not found, or the model doesn't appear | The model id is misspelled or missing the `luv13/` prefix. | Use the exact id `luv13/glm-5.3-flash` from `https://api.luv13.ai/v1/models`. |
| "The requested model is not available" after picking one of Cursor's own models | The override is still on, so Cursor sent that model's request to luv13. Cursor staff report this exact message. | Turn off **Override OpenAI Base URL** to use Cursor's models, or pick `luv13/glm-5.3-flash`. |
| Errors in some Agent requests but not in Chat, or a `404` from luv13 | Cursor staff say Cursor sometimes sends Responses API payloads, which only work with endpoints that serve `/v1/responses`. luv13 serves only `/v1/chat/completions`. | Use Chat or a simpler Agent task. If it keeps happening, report the failing feature to the luv13 operator and to Cursor. |
| Network error, or "Client network socket disconnected before secure TLS connection was established" | A connection problem between Cursor and the endpoint. Cursor staff suggest this fix for that message. | In **Cursor Settings > Network**, set **HTTP Compatibility Mode** to HTTP/1.1, then try again. |
| Tab suggestions still come from another model | Expected. Custom keys don't apply to Tab. | Nothing to fix. |

General tip: if a request fails and you're not sure whether the problem is Cursor or luv13, run test 2 above with the same key and model. If curl works, the problem is in the Cursor settings.

## Sources

- [Cursor, "Bring your own API key"](https://cursor.com/help/models-and-usage/api-keys) (read 2026-09-30)
- [OpenAI Help Center, "Using OpenAI models in Cursor"](https://help.openai.com/en/articles/20001506-using-openai-models-in-cursor) (marked "Updated: last month", read 2026-09-30)
- [Cursor forum, staff replies on the base URL override](https://forum.cursor.com/t/cursor-managed-models-are-routed-through-override-openai-base-url/169088) (August and September 2026) and [Cursor forum, "The custom override of the OpenAI base URL is unusable"](https://forum.cursor.com/t/the-custom-override-of-the-openai-base-url-is-unusable/152675) (February 2026)
- luv13 endpoints and errors checked live with curl on 2026-09-30.

Related: [Base URL](/docs/b/base-url), [OpenAI-Compatible APIs](/docs/o/openai-compatible-apis), [Errors and Status Codes](/docs/e/errors-and-status-codes).
