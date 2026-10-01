# Sources

Maker and product URLs cited by public luv13 docs. Gateway catalogs and supplier pages are not listed here.

Checked against published pages on **2026-10-01**.

## Top-level

| Page | Sources |
| --- | --- |
| [INDEX.md](INDEX.md) | Local A–Z index |
| [SOURCES.md](SOURCES.md) | This table |
| [Compatible Tools](c/compatible-tools.md) | Tool maker docs linked from each [Using …](u/) guide |

## Models

| Page | Model id | Maker sources |
| --- | --- | --- |
| [Kimi K3](m/kimi-k3.md) | `luv13/kimi-k3` | [HF model card](https://huggingface.co/moonshotai/Kimi-K3), [Kimi tech blog](https://www.kimi.com/blog/kimi-k3), [Kimi API models](https://platform.kimi.ai/docs/models) |
| [Kimi K3 Fast](m/kimi-k3-fast.md) | `luv13/kimi-k3-fast` | [Kimi API models](https://platform.kimi.ai/docs/models) (no Fast entry as of 2026-10-01); see gaps on the page |
| [GLM 5.3](m/glm-5-3.md) | `luv13/glm-5.3` | [HF model card](https://huggingface.co/zai-org/GLM-5.3), [Z.ai GLM-5.3 guide](https://docs.z.ai/guides/llm/glm-5.3), [Z.ai release notes](https://docs.z.ai/release-notes/new-released) |
| [GLM-5.3 Flash](m/glm-5-3-flash.md) | `luv13/glm-5.3-flash` | [HF model card](https://huggingface.co/zai-org/GLM-5.3-Flash), [Z.ai GLM-5.3-Flash guide](https://docs.z.ai/guides/llm/glm-5.3-flash), [Z.ai release notes](https://docs.z.ai/release-notes/new-released) |
| [DeepSeek V4.1 Flash](m/deepseek-v4-1-flash.md) | `luv13/deepseek-v4.1-flash` | [HF model card](https://huggingface.co/deepseek-ai/DeepSeek-V4.1-Flash), [DeepSeek release news](https://api-docs.deepseek.com/news/news260910), [DeepSeek API updates](https://api-docs.deepseek.com/updates) |
| [DeepSeek V4-Pro](m/deepseek-v4-pro.md) | `luv13/deepseek-v4-pro` | [HF model card](https://huggingface.co/deepseek-ai/DeepSeek-V4-Pro), [DeepSeek API updates](https://api-docs.deepseek.com/updates), [DeepSeek pricing](https://api-docs.deepseek.com/quick_start/pricing) |
| [Qwen 3.8 27B](m/qwen-3-8-27b.md) | `luv13/qwen-3.8-27b` | [HF model card](https://huggingface.co/Qwen/Qwen3.8-27B), [Qwen3.8 GitHub](https://github.com/QwenLM/Qwen3.8) |

luv13 model ids come from live `GET https://api.luv13.ai/v1/models`. Flat rate: see [Pricing](p/pricing.md).

## Harnesses (Use with)

| Page | Anchor on Compatible Tools | Primary tool sources (from each guide) |
| --- | --- | --- |
| [Using Cursor](u/using-cursor.md) | [#cursor](c/compatible-tools.md#cursor) | [Cursor BYOK help](https://cursor.com/help/models-and-usage/api-keys) |
| [Using VS Code](u/using-vs-code.md) | [#vs-code](c/compatible-tools.md#vs-code) | VS Code Copilot / custom endpoint docs cited on the page |
| [Using Cline](u/using-cline.md) | [#cline](c/compatible-tools.md#cline) | [Cline docs](https://docs.cline.bot/) (OpenAI Compatible provider) |
| [Using Claude Code](u/using-claude-code.md) | [#claude-code](c/compatible-tools.md#claude-code) | Anthropic Claude Code docs cited on the page |
| [Using Open WebUI](u/using-open-webui.md) | [#open-webui](c/compatible-tools.md#open-webui) | Open WebUI connection docs cited on the page |
| [Using Codex](u/using-codex.md) | [#codex](c/compatible-tools.md#codex) | OpenAI Codex CLI docs cited on the page |
| [Using Hermes](u/using-hermes.md) | [#hermes](c/compatible-tools.md#hermes) | Hermes Agent provider docs cited on the page |
| [Using Kilo Code](u/using-kilo-code.md) | [#kilo-code](c/compatible-tools.md#kilo-code) | Kilo Code settings docs cited on the page |

## Gaps worth knowing

- **Kimi K3 Fast:** no separate maker Fast page or SKU; page documents uncertainty only.
- **Claude Code / Codex:** guides exist; neither works until luv13 serves `/v1/messages` or `/v1/responses`.
- **Hermes:** documented as a custom provider; not end-to-end tested on luv13 as of the guide's last check.
