---
name: models
description: Track current LLM context-window sizes across vendors and map them to recognizable nerd books that fit
---

# Models

Author: Jota Feldmann

## Intro

  The `models` skill tracks current LLM context-window sizes across major vendors and compares them to recognizable books and fictional works that can fit inside those windows.

  > Context size is measured in tokens, not parameters. When you invoke `/models`, the agent should verify current model limits from official vendor sources, normalize them into context-size buckets, estimate rough English word capacity, and update the comparison table below.

## LLM context windows vs. nerd books that fit

Last updated: 2026-06-23

Rule of thumb: `1 token ≈ 0.75 English words`.  
So: `rough_word_budget = context_tokens × 0.75`.

This is an estimate. Real fit depends on tokenizer, edition, formatting, system prompts, user instructions, citations, and desired output length.

| Context size | Current models/vendors, examples as of 2026-06-23 | Rough English word budget | Nerd book/work that fits | Fit notes |
|---:|---|---:|---|---|
| **32K–64K** | **OpenAI ChatGPT GPT-5.5 Instant**: Free 16K, Plus/Business 32K, Pro/Enterprise 128K. Source: [OpenAI Help Center](https://help.openai.com/en/articles/11909943) | ~24k–48k words | **The Time Machine** by H. G. Wells | More recognizable replacement for *Flatland*. Around **35,483 words**, so it fits a 64K-class context, but not safely in 32K. Source: [How Long to Read](https://howlongtoread.com/books/32452/The-Time-Machine) |
| **128K** | **OpenAI ChatGPT GPT-5.5 Instant Pro/Enterprise**. Source: [OpenAI Help Center](https://help.openai.com/en/articles/11909943) | ~96k words | **Do Androids Dream of Electric Sheep?** by Philip K. Dick | The book behind *Blade Runner*. Around **67,131 words**, so it fits comfortably in 128K. Source: [How Long to Read](https://howlongtoread.com/books/15172/Do-Androids-Dream-of-Electric-Sheep) |
| **200K** | **Anthropic Claude Haiku 4.5**: 200K context; **Claude Opus 4.8 on Microsoft Foundry** is also listed at 200K. Source: [Anthropic model overview](https://platform.claude.com/docs/en/about-claude/models/overview) | ~150k words | **The Martian** by Andy Weir | Very good hard-science example. Around **104,588 words**, so it fits well. Source: [Accelerated Reader Bookfinder](https://www.arbookfind.com/bookdetail.aspx?client=WKAR&l=EN&q=174189) |
| **256K** | **OpenAI ChatGPT GPT-5.5 Thinking paid tiers**: 256K; **Mistral Large 3**: 256K; **Cohere Command A**: 256K; **xAI Grok Build 0.1**: 256K. Sources: [OpenAI Help Center](https://help.openai.com/en/articles/11909943), [Mistral Large 3](https://docs.mistral.ai/models/model-cards/mistral-large-3-25-12), [Cohere Command A](https://docs.cohere.com/docs/command-a), [xAI models](https://docs.x.ai/developers/models) | ~192k words | **Ready Player One** by Ernest Cline | Replacement for *Snow Crash*. Around **136,048 words**, so it fits comfortably in 256K. Source: [Wordcounters](https://wordcounters.com/word-count/book/ready-player-one) |
| **400K** | **OpenAI ChatGPT GPT-5.5 Thinking Pro**: 400K; **OpenAI GPT-5 / GPT-5 mini API**: 400K; **GPT-5-Codex**: 400K. Sources: [OpenAI Help Center](https://help.openai.com/en/articles/11909943), [OpenAI GPT-5 model docs](https://developers.openai.com/api/docs/models/gpt-5), [OpenAI GPT-5-Codex docs](https://developers.openai.com/api/docs/models/gpt-5-codex) | ~300k words | **Dune** by Frank Herbert, or **The Ultimate Hitchhiker’s Guide to the Galaxy** by Douglas Adams | *Dune* is safer here than in 256K because estimates vary by edition. *The Ultimate Hitchhiker’s Guide* is around **266,290 words**, so it fits but leaves less room. Source: [Reading Length](https://www.readinglength.com/work/Wrz9hez) |
| **1M** | **OpenAI GPT-5.5 / GPT-5.5 Pro API**: ~1.05M; **Anthropic Claude Opus 4.8 / Sonnet 4.6**: 1M; **Google Gemini 3.1 Pro Preview**: 1,048,576 input tokens; **xAI Grok 4.3**: 1M; **Amazon Nova Premier**: 1M; **DeepSeek V4**: 1M. Sources: [OpenAI model comparison](https://developers.openai.com/api/docs/models/compare), [Anthropic model overview](https://platform.claude.com/docs/en/about-claude/models/overview), [Gemini 3.1 Pro Preview](https://ai.google.dev/gemini-api/docs/models/gemini-3.1-pro-preview), [xAI models](https://docs.x.ai/developers/models), [Amazon Nova long context](https://docs.aws.amazon.com/nova/latest/userguide/prompting-long-context.html), [DeepSeek V4 Preview](https://api-docs.deepseek.com/news/news260424) | ~750k words | **The Hobbit + The Lord of the Rings trilogy** | A good “big nerd canon” fit. LOTR is roughly ~481k words, and *The Hobbit* is roughly ~95k words, so together they are around ~575k words / ~767k tokens. Sources: [LotRProject](https://lotrproject.com/statistics/books/wordscount), [Bitlather word counts](https://bitlather.com/blog/article/16/how-many-words-are-in-a-fantasy-novel) |
| **10M-class** | **Meta Llama 4 Scout**: 10M; **Alibaba Qwen-Long**: 10M via file upload/reference mechanism. Sources: [Meta Llama 4 announcement](https://ai.meta.com/blog/llama-4-multimodal-intelligence/), [Alibaba Cloud Qwen-Long docs](https://www.alibabacloud.com/help/en/model-studio/long-context-qwen-long) | ~7.5M words | **Harry Potter + LOTR + A Song of Ice and Fire published books**, together | This tier can fit multiple huge series together on paper. *Harry Potter* is around **1,084,170 words**; published *A Song of Ice and Fire* is around **1,736,054 words**. Sources: [WordCounter Harry Potter](https://wordcounter.net/blog/2015/11/23/10922_how-many-words-harry-potter.html), [A Song of Ice and Fire word count](https://en.wikipedia.org/wiki/A_Song_of_Ice_and_Fire) |

## Agent auto-update instructions

1. Set the update date at the top of the file using ISO format: `Last updated: YYYY-MM-DD`.

2. Use official vendor/model pages first. Check at least:
   - OpenAI API model docs and ChatGPT Help Center
   - Anthropic model overview and context-window docs
   - Google Gemini model cards
   - xAI model docs
   - AWS/Amazon Nova docs
   - Mistral model cards
   - Cohere model docs
   - DeepSeek API docs/news pages
   - Meta AI/Llama official announcements
   - Alibaba Cloud/Qwen official docs

3. Extract these fields per model:
   - vendor
   - model name
   - model ID, if available
   - maximum context window
   - maximum input tokens
   - maximum output tokens
   - API/app/product surface
   - tier restrictions
   - preview/deprecation status
   - source URL
   - date checked

4. Normalize models into context buckets:
   - `32K–64K`
   - `128K`
   - `200K`
   - `256K`
   - `400K`
   - `1M`
   - `2M–4M`, only if currently supported by official docs
   - `10M-class`

5. Separate API limits from app limits. Example: an OpenAI API model may expose a larger context window than the ChatGPT app tier.

6. Label caveats clearly:
   - preview
   - invite-only
   - region-limited
   - file-reference mode
   - hosted cap differs from open-weight theoretical cap
   - product-tier-specific
   - deprecated or retiring soon

7. Recalculate rough word budget with:
   - `word_budget = context_tokens × 0.75`

8. Also calculate a safer practical budget by reserving 10–20% of the context for:
   - system prompts
   - developer instructions
   - user prompt
   - citations
   - reasoning overhead
   - output

9. Validate book/work examples. Prefer exact or verified public word-count sources when possible. If sources disagree, use a range and say “depending on edition/source.”

10. Preserve this table schema:
   - `Context size`
   - `Current models/vendors, examples as of YYYY-MM-DD`
   - `Rough English word budget`
   - `Nerd book/work that fits`
   - `Fit notes`

11. Do not carry over old model limits without re-checking. Context-window limits change often.

12. Add a note that context size is not the same as reliable recall. A model may accept 1M tokens but still perform worse on retrieval, reasoning, or “needle in the middle” tasks at long range.

13. When updating book examples, prefer recognizable nerd-culture works:
   - 64K: *The Time Machine*
   - 128K: *Do Androids Dream of Electric Sheep?*
   - 200K: *The Martian*
   - 256K: *Ready Player One*
   - 400K: *Dune* or *The Ultimate Hitchhiker’s Guide to the Galaxy*
   - 1M: *The Hobbit + The Lord of the Rings*
   - 10M-class: multiple major series together, such as *Harry Potter + LOTR + A Song of Ice and Fire*

14. If you are an agent, update this data.
