---
name: daily-news
description: Fetch today's top news from multiple sources in parallel and synthesize a top-5 digest
---

# Daily News

Author: Jota Feldmann

## Phase 1 — Discover sources

You are the orchestrator for a multi-agent news digest.

Start by using the `WebSearch` tool to find 3-5 currently active, reputable English-language news websites. Use the query:

> "top news websites headlines today"

From the search results, extract 3-5 direct URLs to news homepages (e.g. https://www.bbc.com/news, https://reuters.com, etc.). Prefer sources that are likely to have machine-readable HTML (avoid paywalled or JavaScript-heavy SPAs where possible).

Store these URLs as a list — you will use them in Phase 2.

## Phase 2 — Fetch in parallel

For each URL discovered in Phase 1, spawn one worker agent **in parallel**.

IMPORTANT: All worker agents MUST be spawned in a single message using multiple `Agent` tool calls at once. Do NOT spawn them one at a time sequentially.

Each worker agent receives this prompt (substitute `{{URL}}` with the actual URL):

---
You are a news extraction worker. Your only job is to fetch headlines from one news website and return them as structured text.

1. Use `WebFetch` to load this URL: {{URL}}
2. Extract the top 5 headlines visible on the page
3. For each headline, write one sentence summarizing the story if the page provides enough context; otherwise just return the headline
4. Return your results in exactly this format:

SOURCE: {{URL}}
1. [Headline] — [One-sentence summary or "No summary available"]
2. [Headline] — [One-sentence summary or "No summary available"]
3. [Headline] — [One-sentence summary or "No summary available"]
4. [Headline] — [One-sentence summary or "No summary available"]
5. [Headline] — [One-sentence summary or "No summary available"]

If the page fails to load or returns no readable content, return:
SOURCE: {{URL}}
ERROR: Could not fetch content
---

Wait for all worker agents to return before proceeding to Phase 3.
If fewer than 2 workers return valid results, print:
"Could not gather enough news data. Please try again."
and stop.

## Phase 3 — Synthesize digest

You now have headline lists from each worker agent. Synthesize a top-5 digest:

1. **Identify consensus stories** — if the same event appears in headlines from 2 or more sources, it is higher priority. Normalize for different wordings of the same event (e.g. "Ukraine ceasefire talks" and "Peace negotiations in Ukraine" are the same story).

2. **Rank stories** — order by:
   - Number of sources covering the story (more = higher)
   - Recency signals in the headline (e.g. "today", "breaking", a date)
   - General significance (geopolitical > business > entertainment, when unclear)

3. **Pick the top 5** — if fewer than 5 unique stories exist across all sources, include all of them.

4. **Print the digest** in this exact format:

    ## Today's Top News — [today's date]

    1. **[Headline]** — [Source name(s) that covered it]
       [One-sentence summary]

    2. **[Headline]** — [Source name(s)]
       [One-sentence summary]

    3. **[Headline]** — [Source name(s)]
       [One-sentence summary]

    4. **[Headline]** — [Source name(s)]
       [One-sentence summary]

    5. **[Headline]** — [Source name(s)]
       [One-sentence summary]

    ---
    Sources checked: [list the URLs fetched]
