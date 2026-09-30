---
title: Environment Variables
definition: An environment variable is a named value set outside your code, such as an API key, that your program reads when it runs.
description: How to set and read environment variables so your luv13 API key stays out of source code, on macOS, Linux and Windows.
category: general
author: Quill
status: draft
last_checked: 2026-09-30
---

## Key takeaways

- Keep secrets like your luv13 key in an environment variable, not in source code.
- These docs use `LUV13_API_KEY` as the name for your luv13 key.
- Set it in your shell, a `.env` file that git ignores, or your host's secret settings.
- Read it in code with `os.environ` (Python) or `process.env` (Node.js).
- Many OpenAI-compatible tools also read `OPENAI_API_KEY` and a base URL variable. Check each tool's docs for the exact names.

## Setting one

In a macOS or Linux shell, for the current session:

```bash
export LUV13_API_KEY="paste-your-key-here"
```

To keep it across sessions, add that line to your shell profile, such as `~/.zshrc` or `~/.bashrc`.

On Windows PowerShell, for the current session:

```powershell
$env:LUV13_API_KEY = "paste-your-key-here"
```

## Using a .env file

Many projects keep local settings in a `.env` file and load it with a library such as `python-dotenv` or Node's `--env-file` flag. If you do:

- Add `.env` to `.gitignore` **before** you create it.
- Commit a `.env.example` with the variable names and no real values.

## Reading it in code

```python
import os
key = os.environ["LUV13_API_KEY"]   # fails loudly if it's missing
```

```js
const key = process.env.LUV13_API_KEY;
if (!key) throw new Error("LUV13_API_KEY is not set");
```

## Example

Once the variable is set, the shell fills it in for you:

```bash
curl https://api.luv13.ai/v1/chat/completions \
  -H "Authorization: Bearer $LUV13_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"model": "luv13/glm-5.3-flash", "messages": [{"role": "user", "content": "Hi"}]}'
```

For more on keeping keys safe, see [API Key Best Practices](/docs/a/api-key-best-practices).
