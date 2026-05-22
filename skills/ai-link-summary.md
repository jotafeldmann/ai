---
name: ai-link-summary
description: Generate friendly, human-reviewed summaries for internet links to help technical and non-technical people learn AI
---

# AI Link Summary

Author: Jota Feldmann

## Intro

The `ai-link-summary` skill turns an internet link into a short, shareable knowledge summary for people learning about AI.

The goal is to make useful AI tools, projects, papers, demos, libraries, and articles easier to understand for both technical and non-technical folks.

The output should be clear, practical, and safe to share in chats, newsletters, GitHub issues, Discord, Slack, LinkedIn, or community learning spaces.

## When to use this skill

Use this skill when the user gives you a link and asks for a short summary, learning note, tool explanation, or shareable description.

The summary should help readers quickly understand:

- what the link is
- why it matters
- who it is useful for
- how it works at a high level
- what to be careful about
- whether it is relevant for learning AI

## Phase 1 — Fetch and inspect the link

Use `WebFetch` to open the user-provided URL.

Read the page carefully and identify:

- the project, article, tool, library, demo, paper, or service name
- its stated purpose
- the main idea
- who it is for
- what it works with
- how it works
- whether it stores data, uses external services, requires accounts, or runs locally
- practical use cases
- notable extra features
- any security, privacy, accuracy, or supply-chain concerns

If `WebFetch` fails, use `WebSearch` with the URL, project name, or page title to find reliable supporting information.

If you cannot access enough reliable information, say so clearly and avoid guessing.

## Phase 2 — Decide the audience level

Write for people learning AI, including non-technical readers.

Use simple language.

Avoid unnecessary jargon.

When technical terms are needed, briefly explain them in plain English.

Prefer practical explanations over marketing language.

Do not overhype the project.

Do not invent features.

Do not claim the tool is safe, private, open-source, free, local, or production-ready unless the source clearly supports it.

## Phase 3 — Generate the summary

The output must follow this format exactly:

[URL]

- **Purpose:** [one short sentence explaining what the link is for.]
- **Main idea:** [one short sentence explaining the core concept.]
- **Works with:** [platforms, tools, models, frameworks, or audiences it works with.]
- **How it works:** [plain-English explanation of the workflow or mechanism.]
- **Storage/search:** [how data is stored, searched, processed, or whether this is not applicable.]
- **Useful for:** [practical use cases, especially for people learning AI.]
- **Extra features:** [notable features, integrations, demos, APIs, benchmarks, viewers, docs, or community resources.]
- ⚠️ **Security warning:** [specific safety, privacy, supply-chain, accuracy, or operational caution based on the link.]

_AI usage notice: This summary was generated with OpenAI ChatGPT and reviewed by a human._

## Writing rules

Keep the summary short and useful.

Use bullets.

Keep emojis where helpful.

Keep the link at the top.

Add one blank line after the link.

Use bold labels for each bullet.

Use the ⚠️ emoji for the security warning.

Keep the AI usage notice at the bottom.

Make the security warning specific to the type of link:

- for open-source tools: mention reviewing code, pinning trusted versions, verifying install sources, and supply-chain risk
- for cloud services: mention data privacy, account permissions, pricing, and terms of service
- for local AI tools: mention local permissions, shell access, model downloads, and sensitive files
- for benchmarks: mention that benchmarks can be incomplete, outdated, or hardware-dependent
- for research papers: mention that results may be experimental and should be validated before production use
- for calculators or estimators: mention that results are estimates and should be checked against real-world benchmarks
- for browser extensions or agents: mention permissions, browsing/session access, and sensitive data exposure

## Examples

[https://llmcalc.teske.live](https://llmcalc.teske.live)

- **Purpose:** no-login VRAM and performance calculator for local LLM setups.
- **Main** idea: helps users estimate whether a given model + hardware setup can run locally, including VRAM needs and expected performance.
-** Works with**: browser users planning local LLM inference or fine-tuning setups, especially people comparing models, GPUs, and configurations.
- **How it works**: users choose or enter model and hardware details, then the calculator estimates hardware fit, VRAM requirements, throughput/decode speed, and related constraints.
- **Storage/search**: front-end web tool; no account required.
- **Useful for**: checking whether a PC or GPU can run a chosen LLM, comparing upgrade options, avoiding bad hardware purchases, and planning local/self-hosted AI workloads.
- **Extra features**: no-login demo, open-source codebase, community contribution path for model/GPU data, and calculator-style estimates for LLM hardware planning.
- **⚠️ Security warning**: because this is an open-source tool related to local AI deployment planning, users should treat its outputs as estimates rather than guarantees. Review the code and methodology, verify install/source links, avoid entering sensitive deployment details unnecessarily, and validate results against real benchmarks before using it for expensive hardware decisions or sensitive workloads.

_AI usage notice: This summary was generated with OpenAI ChatGPT and reviewed by a human._

## Quality checklist

Before returning the summary, check that:

- the URL is at the top
- there is a blank line after the URL
- every bullet is useful to a beginner
- the explanation is understandable by non-technical readers
- claims are grounded in the fetched page or reliable supporting sources
- the warning is not generic if a more specific warning is possible
- the AI usage notice is included exactly
- the output is ready to copy and paste
