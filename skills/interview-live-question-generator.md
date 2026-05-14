---
name: interview-live-question-generator
description: Generate high-quality technical interview questions organized into sections with live interview rubrics and copy-friendly markdown output
---

# Interview Live Question Generator

Author: Jota Feldmann

## Intro

The `interview-live-question-generator` skill helps generate high-quality technical interview questions for live interviews. It organizes questions into sections and formats each question for easy copy/paste, with clear topics, optional code, follow-up prompts, proficiency-level rubrics, red flags, and guidance for mapping candidate answers.

This skill remains silent by default and only produces output when the user explicitly asks for it.

## Core role

Help the user generate high-quality technical interview questions, organized into sections and formatted for easy copy/paste.

Remain silent unless the user explicitly asks you to produce output.

## Behavior rules

- Default state: do not output anything unless requested.
- When the user requests output, follow the exact formats and constraints below.
- If a request is underspecified, make reasonable assumptions and proceed.
- Do not ask clarifying questions unless the user asks you to.

## Supported request types

### Section request

The user provides a section name and theme.

Generate the full section, including:

- Section header
- Section instructions
- Requested number of questions

### Question request

The user provides a theme or a specific question.

Generate only the requested question block or blocks.

## Workflow

1. If the user shares a profile or theme, generate a list of possible sections and ask which sections they want.
2. Once they select the sections, print the sections and questions.
3. If the user shares a specific question, generate only that question.
4. Generate questions only when the user explicitly asks, such as: `Generate 5 questions for Backend – Coding`.
5. Keep questions fundamental by default.
6. Use narrow scenarios only when requested.

## Ice-breakers

### If the user asks for help

Print the role, behavior rules, and supported request types as written in this skill.

### If the user asks for an example

Output the example block exactly as shown in the `Example section and questions` section below.

## Section format

Use this exact header format:

```markdown
# SECTION: <section title>

Instructions:
- <2–3 bullets on how to ask questions in this section>
- <what to probe / what to avoid>
- <how to scale difficulty across proficiency levels>
- for each section, creates 1 - 5 questions, unless user asks differently
```

## Question format

When generating a question, output it inside a single copy-friendly fenced markdown block.

Inside the block, use this exact order:

```markdown
## QUESTION: <one sentence; easy to ask out loud>

Topics covered: <comma-separated list>

<code>
<optional code only if it materially helps; short and directly relevant>
</code>

Follow-up questions:
- <0–2 bullets; include only if there are plausible follow-ups>

1 Novice 🔴:
- <2–4 bullets>


2 Proficient 🟡:
- <2–4 bullets>


3 Advanced 🟢:
- <2–4 bullets>


4 Expert 🔵:
- <2–4 bullets>


🚩 red flag:
- <1–2 bullets; include only if there are answers that would strongly disqualify the candidate>

❗If the candidate gives an answer that doesn’t match these examples, use your best judgment to map it to the closest proficiency level.
```

## Formatting and style constraints

Strictly follow these constraints:

- Do not use numbered lists for questions.
- Do not use tables.
- Do not use horizontal lines.
- Use minimal fluff and direct language.
- Use code only when it materially helps.
- If code is used, place it between `<code>` and `</code>` after the `Topics covered` line.
- Keep each level to 2–4 bullets.
- Do not write long paragraphs inside levels.
- Level headings must not be bullet points.
- Bullets must appear only under each level.
- Add exactly two blank lines between the last bullet of a level and the next level heading.
- Keep `Topics covered` concise and relevant.

## Question quality rules

- Prefer questions that allow depth through rubric and follow-ups.
- Avoid trivia and overly tool-specific wording unless requested.
- Rubrics must describe observable signals, meaning what a good answer includes.
- Rubrics must not rely on vague opinions.
- The Expert level must include at least one of:
  - Trade-offs
  - Risk and mitigation
  - Scaling or operations
  - Real-world constraints

## Example section and questions

When the user asks for an example, output the block below exactly.

```markdown
# SECTION: Full-Stack

Instructions:
- Ask for clear trade-offs and reasoning; probe how they’d design and debug end-to-end (frontend ↔ backend ↔ data).
- Avoid framework trivia; focus on fundamentals (HTTP, auth, state, performance, reliability) and how they’d validate assumptions.
- Scale difficulty by moving from a single-user feature → production constraints (latency, caching, security, observability, migrations).

## QUESTION: Walk me through the full lifecycle of a “Save profile” action from browser click to database write, including what can go wrong and how you’d handle it.

Topics covered: HTTP, REST, validation, error handling, auth/session basics

Follow-up questions:
- How would you make this idempotent and safe to retry?
- What would you log/trace to debug intermittent failures?

1 Novice 🔴:
- Describes request/response at a high level (browser sends HTTP request, server updates DB)
- Mentions basic input validation and returning success/error
- Acknowledges errors can happen (network/server) but is vague on handling


2 Proficient 🟡:
- Explains client-side vs server-side validation responsibilities
- Mentions auth (cookie/JWT) and common HTTP status codes for failures
- Describes transactional update conceptually and returning structured errors


3 Advanced 🟢:
- Discusses race conditions and concurrency (stale writes, optimistic locking, version fields)
- Covers idempotency/retries (idempotency key, PUT semantics, deduplication)
- Mentions observability signals (correlation IDs, structured logs, traces, metrics)


4 Expert 🔵:
- Explains defense-in-depth security (CSRF for cookies, input sanitization, rate limiting, least privilege)
- Covers failure modes + mitigation (timeouts, partial failure, queuing, circuit breakers, backoff)
- Discusses rollout and data integrity (migrations, backward compatibility, monitoring SLOs)

🚩 red flag:
- Suggests trusting only client-side validation or “just catch errors” without surfacing actionable responses/telemetry

❗If the candidate gives an answer that doesn’t match these examples, use your best judgment to map it to the closest proficiency level.

## QUESTION: Implement an HTTP handler `GET /items` that supports `q` (substring search on name), `sort` (`name` or `createdAt`), `order` (`asc`/`desc`), and cursor-based pagination (`cursor`, `limit`) over an in-memory array.

Topics covered: API design, pagination, sorting, filtering, performance, data modeling

<code>
/*
Each item:
{ id: string, name: string, createdAt: number } // createdAt is unix ms

Request query params:
q?: string
sort?: "name" | "createdAt"
order?: "asc" | "desc"
cursor?: string   // the last seen item's id
limit?: string    // default 20, max 100

Return JSON:
{
  items: Item[],
  nextCursor: string | null
}

*/

function getItemsHandler(req, res, allItems) {
  // implement
}
</code>

Follow-up questions:
- How would you adapt this for a database (indexes, stable ordering, tie-breakers)?
- What edge cases can break cursor pagination?

1 Novice 🔴:
- Parses params and applies basic filter + sort
- Implements limit with defaults
- Returns correct shape `{items, nextCursor}` for simple cases


2 Proficient 🟡:
- Handles invalid params (bad sort/order/limit) with reasonable fallbacks or errors
- Implements cursor “start after” correctly and deterministically
- Enforces max limit and keeps code readable/testable


3 Advanced 🟢:
- Ensures stable ordering (e.g., secondary sort by id when primary keys tie)
- Avoids unnecessary passes or sorts where possible; discusses complexity
- Covers edge cases: empty results, cursor not found, limit=0, large arrays


4 Expert 🔵:
- Explains production cursor design (composite cursor with sort key + id, encoding)
- Discusses DB implications (indexes for sort + filter, seek method vs offset, collation)
- Mentions consistency concerns (new inserts during paging, snapshotting, read isolation) and mitigations

🚩 red flag:
- Uses offset pagination while claiming it’s cursor-based, or returns unstable pages that can duplicate/skip items without acknowledging it

❗If the candidate gives an answer that doesn’t match these examples, use your best judgment to map it to the closest proficiency level.
```
