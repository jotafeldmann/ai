---
name: english-language-evaluation
description: Evaluate English language evidence with CEFR-aware precautions and distinguish provisional screening from validated CEFR determination
---

# English Language Evaluation

Author: Jota Feldmann

## Intro

The `english-language-evaluation` skill helps an agent evaluate English language evidence in a cautious, CEFR-aware way.

When you invoke `/english-language-evaluation`, the agent follows a multi-phase evaluation workflow: it identifies the evidence type, determines which language modalities are covered, analyzes CEFR-related indicators, checks whether the available evidence is sufficient, and produces a cautious evaluation that clearly distinguishes provisional screening from validated CEFR determination.

This skill is intended for transcripts, interview excerpts, writing samples, communication samples, or other English-language evidence.

This skill must not be used to certify a CEFR level.

A valid CEFR determination requires CEFR-aligned tasks, appropriate evidence across relevant skills, and a qualified evaluator or validated assessment process.

## Phase 1 — Identify evidence type

You are the evaluator for an English language assessment workflow.

Start by identifying what kind of evidence the user provided.

Classify the evidence as one or more of:

* Original audio
* Literal transcript
* Cleaned transcript
* AI-generated transcript
* Summarized transcript
* Translated transcript
* Writing sample
* Interview notes
* Self-reported level
* Assessment score
* Unknown evidence type

If the input does not clearly state whether a transcript is literal, cleaned, summarized, or AI-generated, classify it as:

> Evidence type: Transcript of unknown fidelity

Then assess evidence quality:

* Strong: original audio, literal transcript, CEFR-aligned writing task, or validated assessment evidence
* Moderate: transcript with clear task context but no audio
* Weak: cleaned transcript, summarized transcript, AI-rewritten transcript, translated transcript, interview notes, self-reported level, or short excerpt
* Unknown: insufficient information to classify the evidence

Store the evidence type and evidence quality. You will use them in later phases.

## Phase 2 — Identify assessment scope

Determine which language skills or modalities can be evaluated from the available evidence.

Possible scopes include:

* Spoken production
* Spoken interaction
* Listening comprehension
* Pronunciation
* Prosody
* Real-time fluency
* Writing
* Reading
* Grammar
* Vocabulary
* Coherence
* Workplace communication
* Interview communication
* Mediation

Only include a scope if the evidence actually supports it.

Examples:

If the user provides only a cleaned interview transcript, you may assess:

* Grammar as represented in text
* Vocabulary as represented in text
* Coherence as represented in text
* Ability to explain professional experience
* Interview communication as represented in text

But you must not assess:

* Pronunciation
* Prosody
* Real-time fluency
* Listening comprehension
* Full spoken interaction quality
* Reading
* Writing, unless a writing sample is present

Store two lists:

1. Scope assessed
2. Scope not assessed

## Phase 3 — Check CEFR validity conditions

Before assigning any CEFR-related estimate, check whether the available evidence supports a validated CEFR determination.

A validated CEFR determination usually requires:

* CEFR-aligned tasks
* Clear task instructions
* Appropriate evidence across relevant skills
* Original audio for speaking assessment
* Writing sample for writing assessment
* Listening task for listening assessment
* Reading task for reading assessment
* CEFR-specific rubric
* Calibrated examples or scoring rules
* Qualified evaluator or validated automated assessment process

If these conditions are not met, do not claim that the person “is A1”, “is B2”, “is C1”, or similar.

Instead, use cautious wording such as:

> The available evidence shows CEFR-related indicators compatible with B1/B2 spoken production.

or:

> This is a provisional screening hypothesis, not a validated CEFR determination.

## Phase 4 — Analyze CEFR-related indicators

Analyze the evidence only within the supported scope.

Look for indicators such as:

* Communicative function
* Task completion
* Grammar range
* Grammar accuracy
* Vocabulary range
* Vocabulary precision
* Coherence
* Organization
* Ability to narrate events
* Ability to describe experience
* Ability to explain decisions
* Ability to justify opinions
* Ability to compare alternatives
* Ability to handle abstract topics
* Use of connectors and discourse markers
* Recurring error patterns
* Sentence complexity
* Fluency evidence, only if preserved
* Interaction evidence, only if preserved

Do not invent evidence.

Do not infer pronunciation from text.

Do not infer listening comprehension from an answer unless the task required observable listening comprehension.

Do not treat a polished, cleaned, or AI-rewritten transcript as direct evidence of the learner's original language performance.

If the transcript appears cleaned or normalized, explicitly downgrade confidence.

## Phase 5 — Produce provisional estimate

Based on the evidence and limitations, produce one of the following:

* No CEFR estimate possible
* CEFR-related indicators only
* Provisional CEFR range
* Validated CEFR level, only if the evidence and process support it

Prefer ranges when evidence is partial.

Examples:

> Possible range: A2/B1

> Possible range: B1/B2

> Possible range: B1+/B2 for spoken production only

Use confidence levels:

* Low
* Medium
* High

Use High only when evidence is strong, task context is clear, and the relevant modalities are covered.

If the evidence is weak, use Low confidence.

If the evidence is moderate but incomplete, use Low to Medium confidence.

## Phase 6 — Apply internal proficiency boundary

If the user also asks for an internal workplace proficiency label, keep it separate from CEFR.

Internal labels such as:

* Novice
* Proficient
* Advanced
* Expert

must not be mapped directly to CEFR levels.

Good example:

```text
English for client communication: Proficient

CEFR note:
This is an internal workplace communication assessment, not a CEFR determination.
```

Bad example:

```text
English: Advanced, equivalent to C1.
```

If using an internal proficiency guide, state that it evaluates workplace communication capability, not certified language proficiency.

## Phase 7 — Print evaluation

Print the evaluation in exactly this format:

```text
## English Language Evaluation

Evidence type:
[Original audio | Literal transcript | Cleaned transcript | AI-generated transcript | Summarized transcript | Translated transcript | Writing sample | Interview notes | Self-reported level | Assessment score | Transcript of unknown fidelity | Unknown]

Evidence quality:
[Strong | Moderate | Weak | Unknown]

Scope assessed:
- [Skill or modality]
- [Skill or modality]

Scope not assessed:
- [Missing skill or modality]
- [Missing skill or modality]

CEFR-related indicators:
- [Indicator supported by the evidence]
- [Indicator supported by the evidence]
- [Indicator supported by the evidence]

Provisional estimate:
[No CEFR estimate possible | CEFR-related indicators only | Possible range: A2/B1 | Possible range: B1/B2 | Possible range: B1+/B2 for spoken production only | Validated CEFR level: only if supported]

Confidence:
[Low | Medium | High]

Missing evidence:
- [Missing evidence]
- [Missing evidence]

Precaution:
This is not a validated CEFR determination unless the evaluation used CEFR-aligned tasks, appropriate evidence across relevant skills, and a qualified evaluator or validated assessment process.
```

If the user provided insufficient language evidence, print:

```text
## English Language Evaluation

Evidence type:
Insufficient evidence

Evidence quality:
Weak

Scope assessed:
- None

Scope not assessed:
- Spoken production
- Spoken interaction
- Listening comprehension
- Pronunciation
- Real-time fluency
- Writing
- Reading

CEFR-related indicators:
- Not enough evidence to identify CEFR-related indicators.

Provisional estimate:
No CEFR estimate possible.

Confidence:
Low

Missing evidence:
- Actual language sample
- Task context
- CEFR-aligned rubric
- Relevant modality evidence

Precaution:
This is not a validated CEFR determination. No CEFR level should be inferred from the available information.
```

## Output rules

Follow these rules every time:

1. Do not claim a validated CEFR level from transcript-only evidence.
2. Do not infer pronunciation, prosody, or real-time fluency from cleaned text.
3. Do not infer listening comprehension unless the task directly tests it.
4. Do not map internal proficiency labels directly to CEFR.
5. Prefer conservative ranges over exact CEFR levels.
6. State missing evidence explicitly.
7. Always include a precaution when CEFR is mentioned.
8. Separate workplace communication assessment from CEFR assessment.
9. Downgrade confidence when transcripts are cleaned, summarized, translated, or AI-generated.
10. Do not treat AI-polished text as original learner performance.

## Common mistakes to avoid

Avoid these mistakes:

* Mistaking transcript readability for language proficiency
* Mistaking AI-cleaned language for learner language
* Mistaking confidence for fluency
* Mistaking topic complexity for CEFR level
* Mistaking internal labels such as Advanced or Expert for CEFR C1 or C2
* Assigning a precise CEFR level from a short transcript
* Ignoring missing modalities
* Ignoring whether the transcript is literal or cleaned
* Presenting AI screening as certification
* Treating a generic proficiency rubric as a CEFR rubric

## Reference sources

Use these references as background when explaining why the evaluation must be cautious.

Official CEFR sources:

* Council of Europe — Common European Framework of Reference for Languages
  https://www.coe.int/en/web/common-european-framework-reference-languages

* Council of Europe — CEFR Companion Volume
  https://www.coe.int/en/web/common-european-framework-reference-languages/cefr-companion-volume-and-its-language-versions

* Council of Europe — CEFR Descriptors
  https://www.coe.int/en/web/common-european-framework-reference-languages/cefr-descriptors

Research on automated spoken proficiency assessment and transcript limitations:

* Bannò, S., & Matassoni, M. (2022). Proficiency assessment of L2 spoken English using wav2vec 2.0.
  https://arxiv.org/abs/2210.13168

* Qiao, Y., Zhou, W., Kerz, E., & Schlüter, R. (2021). The Impact of ASR on the Automatic Analysis of Linguistic Complexity and Sophistication in Spontaneous L2 Speech.
  https://arxiv.org/abs/2104.08529

* Nebhi, K., & Szaszák, G. (2023). Automatic Assessment Of Spoken English Proficiency Based on Multimodal and Multitask Transformers.
  https://aclanthology.org/2023.ranlp-1.83/

* Lo, T.-H., Chao, F.-A., Wu, T.-I., Sung, Y.-T., & Chen, B. (2024). An Effective Automated Speaking Assessment Approach to Mitigating Data Scarcity and Imbalanced Distribution.
  https://aclanthology.org/2024.findings-naacl.86/

* Scaria, N., Kennedy, S. J. J., Latinovich, T., & Subramani, D. (2024). EvalYaks: Instruction Tuning Datasets and LoRA Fine-tuned Models for Automated Scoring of CEFR B2 Speaking Assessment Transcripts.
  https://arxiv.org/abs/2408.12226
