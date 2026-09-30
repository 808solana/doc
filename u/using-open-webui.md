---
title: Using Open WebUI
definition: Using Open WebUI with luv13 means adding luv13 as an OpenAI API connection so Open WebUI's chat can use luv13 models.
description: Connect Open WebUI to luv13 in the admin panel or with environment variables, see which features work, and fix common problems.
category: general
author: Quill
status: draft
last_checked: 2026-09-30
---

> **Compatibility: works.** Open WebUI is built around the OpenAI Chat Completions protocol, and it can find models through `GET /v1/models`. luv13 serves both. Features that need other endpoints, such as OpenAI-based embeddings for RAG, image generation and audio, won't work through luv13.

## Key takeaways

- As an admin, go to **Settings > Admin > Connections**, click **Add Connection** under **Manage OpenAI API Connections**, and enter URL `https://api.luv13.ai/v1` and your luv13 key.
- Open WebUI reads luv13's model list, so the seven luv13 models appear on their own. You can limit it to `luv13/glm-5.3-flash` with the **Model IDs** allowlist.
- **Verify Connection** only checks `/models`, and luv13 answers that without a key. A green check doesn't prove your key is right, so send a real chat message to confirm.
- For a Docker or server install, you can set the same connection with the `OPENAI_API_BASE_URL` and `OPENAI_API_KEY` environment variables.
- Leave RAG embeddings, image generation and speech on other providers. luv13 serves none of those endpoints.

## Before you start

You need:

- A running Open WebUI and an admin account. These steps follow Open WebUI's "OpenAI-Compatible" guide, read on 2026-09-30. The `open-webui` repo's `package.json` on `main` showed version `0.11.4` that day.
- A luv13 API key. The [Quickstart](/docs/q/quickstart) shows how to get one.
- The model id `luv13/glm-5.3-flash`, from the live list at `https://api.luv13.ai/v1/models`. See [GLM-5.3 Flash](/docs/m/glm-5-3-flash) for details on the model.

Facts about luv13 that affect this setup:

- luv13 serves one generation endpoint, `POST /v1/chat/completions`, plus `GET /v1/models`. `/v1/responses`, `/v1/messages`, `/v1/completions` and `/v1/embeddings` return 404 (checked 2026-09-30). See [Endpoints](/docs/e/endpoints).
- Every model costs a flat $0.33 per 1M tokens, input the same as output. See [Pricing](/docs/p/pricing).
- The model list at `https://api.luv13.ai/v1/models` doesn't report context length, so luv13 publishes no per-model limits. See the [model list](https://luv13.ai/#models).

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

## Set up the connection in the admin panel

1. Open Open WebUI in your browser and sign in as an admin.
2. Go to **Settings > Admin > Connections** and find the **Manage OpenAI API Connections** list.
3. Click **Add Connection** (the ➕ button).
4. Fill in:

   | Field | What to enter |
   |---|---|
   | **URL** | `https://api.luv13.ai/v1` (no trailing slash) |
   | **API Key** | your luv13 key |
   | **Model IDs** | leave empty to show all luv13 models, or add `luv13/glm-5.3-flash` and click **+** to show only that one |
   | **Advanced > Provider** | leave at **Default**. None of the other options (Azure OpenAI, llama.cpp, LM Studio, LiteLLM) describe luv13. |
   | **Advanced > Forward cookies** | leave off. Open WebUI's docs say a third-party endpoint should never have this on. |

5. Click **Save**. Make sure the connection's toggle switch is on.
6. Start a new chat, pick `luv13/glm-5.3-flash` in the model selector, and send "Reply with the word ready."

Open WebUI's docs note that saving a connection doesn't test it. Step 6 is the real test.

## Or set it with environment variables

Open WebUI's docs say it's configured through environment variables, and that the same values can be set in the admin panel. The core connection uses:

```bash
OPENAI_API_BASE_URL=https://api.luv13.ai/v1
OPENAI_API_KEY=your-luv13-key
```

With Docker, Open WebUI's quick start runs the container with `docker run`. Add the two variables as `-e` flags, and read the key from your shell instead of typing it into the command:

```bash
docker run -d -p 3000:8080 \
  --add-host=host.docker.internal:host-gateway \
  -v open-webui:/app/backend/data \
  -e WEBUI_SECRET_KEY=your-secret-key \
  -e OPENAI_API_BASE_URL=https://api.luv13.ai/v1 \
  -e OPENAI_API_KEY="$LUV13_API_KEY" \
  --name open-webui --restart always \
  ghcr.io/open-webui/open-webui:main
```

Replace `your-secret-key` with your own random value, as in Open WebUI's quick start. Open WebUI's docs also list `TASK_MODEL_EXTERNAL` for the model used by background tasks such as title generation. You can set it to `luv13/glm-5.3-flash` so those tasks use luv13 too, and they're billed like any other request.

## Which Open WebUI features use luv13

Open WebUI's docs list the endpoints a provider should serve:

| Endpoint | Open WebUI uses it for | luv13 |
|---|---|---|
| `GET /v1/models` | Model discovery | Served |
| `POST /v1/chat/completions` | Chat, streaming and parameters | Served |
| `POST /v1/embeddings` | RAG with this provider | 404, not served |
| `POST /v1/audio/speech` | Text-to-speech | Not served |
| `POST /v1/audio/transcriptions` | Speech-to-text | Not served |
| `POST /v1/images/generations` | Image generation | Not served |

So don't point `RAG_OPENAI_API_BASE_URL`, `IMAGES_OPENAI_API_BASE_URL` or the audio base URL settings at luv13.

Open WebUI's docs also say it passes standard parameters such as `temperature`, `top_p`, `max_tokens`, `stop` and `seed`, and tools when the server supports `tools` and `tool_choice`. Which of these luv13 honors isn't confirmed per model. See [Request Parameters](/docs/r/request-parameters) and [Tool Calling on luv13](/docs/t/tool-calling-on-luv13).

## Common errors

| Symptom | Likely cause | Fix |
|---|---|---|
| `401` with `{"error":{"code":401,"message":"unauthorized","type":"invalid_auth"}}` | The key is missing, has extra spaces, or isn't a luv13 key. | Paste your luv13 key again. Run test 2 above to confirm it. |
| `404` with an HTML page titled "404 Not Found" | The URL is wrong. Common versions: `https://api.luv13.ai` with no `/v1`, a doubled path such as `https://api.luv13.ai/v1/v1/...`, a pasted full endpoint like `.../v1/chat/completions` in the base URL field, or a trailing slash. | Set the base URL to exactly `https://api.luv13.ai/v1`. The tool adds `/chat/completions` itself. |
| `404` even though the base URL is right | The tool is calling an endpoint luv13 doesn't serve, such as `/v1/responses`, `/v1/messages`, `/v1/completions` or `/v1/embeddings`. | Use the tool's OpenAI chat-completions mode, and turn off features that need other endpoints. See [Endpoints](/docs/e/endpoints). |
| Model not found, or the model doesn't appear | The model id is misspelled or missing the `luv13/` prefix. | Use the exact id `luv13/glm-5.3-flash` from `https://api.luv13.ai/v1/models`. |
| **Verify Connection** says it's fine, but chats fail with 401 | **Verify Connection** only calls `/models`, and luv13's `/v1/models` answers even without a valid key (checked 2026-09-30). | Re-enter the key and test with a real chat message or test 2 above. |
| The model selector shows no luv13 models | The URL is wrong (a trailing slash or a missing `/v1`), the connection toggle is off, or the model list fetch timed out. | Fix the URL to `https://api.luv13.ai/v1` and turn the toggle on. Open WebUI's docs say the model list fetch times out after 10 seconds by default, and `AIOHTTP_CLIENT_TIMEOUT_MODEL_LIST` raises it. |
| All seven luv13 models appear, but you only want one | Auto-discovery lists everything `/v1/models` returns. | Add `luv13/glm-5.3-flash` to **Model IDs** and save. |
| "Model ID is already added" | Open WebUI's docs say each id goes on the allowlist once. | Nothing to fix. The id is already there. |
| Empty assistant replies, mostly when tools are on | Open WebUI's docs describe this for providers that stream tool calls without the `index` field. It isn't confirmed either way for luv13. | Run the tool-calling curl check from Open WebUI's "Connection Errors" page against `https://api.luv13.ai/v1/chat/completions` and look for `"index"` in `delta.tool_calls`. If it's missing, set **Function Calling** to **Legacy** in the model's **Advanced Params**, or turn off the builtin tools for that model. |
| Garbled or broken streamed text behind nginx | Open WebUI's docs say nginx proxy buffering breaks streaming. | Turn off proxy buffering in your reverse proxy, as Open WebUI's "Connection Errors" page shows. |
| RAG, image or voice features error out | They call endpoints luv13 doesn't serve. | Use another provider for those features. |

General tip: admin connections are made from the Open WebUI server, not your browser. If the server has no internet access, luv13 won't be reachable even though curl works on your laptop.

## Sources

- Open WebUI docs, "OpenAI-Compatible", https://docs.openwebui.com/getting-started/quick-start/connect-a-provider/starting-with-openai-compatible (read 2026-09-30). Covers the connection steps, Verify Connection, Model IDs, the Provider setting, required endpoints and supported parameters.
- Open WebUI docs, "Quick Start" (the `docker run` command), https://docs.openwebui.com/getting-started/quick-start (read 2026-09-30)
- Open WebUI docs, "Connection Errors" (blank tool replies, streaming behind nginx, backend and frontend connections), https://docs.openwebui.com/troubleshooting/connection-error (read 2026-09-30)
- Open WebUI source, `package.json` on `main`, https://github.com/open-webui/open-webui (version 0.11.4 on 2026-09-30)
- luv13 endpoints checked live with curl on 2026-09-30, including `/v1/models` returning 200 with an invalid key.

Related: [Using Cursor](/docs/u/using-cursor), [Retrieval-Augmented Generation](/docs/r/retrieval-augmented-generation), [Streaming](/docs/s/streaming).
