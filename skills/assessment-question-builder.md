---
name: assessment-question-builder
description: Create or improve scenario-based multiple-choice training questions with plausible distractors, balanced answer keys, and one clear best answer
---

# Assessment Question Builder

Author: Jota Feldmann

## Intro

Use this skill to create, rewrite, or quality-check multiple-choice questions for training modules.

The goal is to turn raw topics, rough questions, or weak draft questions into practical assessment items that test understanding instead of test-taking shortcuts. The final questions should be scenario-based, have four answer options, use plausible distractors, avoid silly wrong answers, and make the unique correct answer clear without being obvious from length or wording.

The user may provide:

- Raw questions with answer options
- A topic list
- A rough learning objective
- A prototype question
- A course section title
- A mix of good and weak questions
- Only a general idea of what they want to test

When the input is incomplete, make a reasonable best effort and state any assumptions briefly. Do not block on clarification unless the missing information makes the task impossible.

## Output standard

Unless the user requests another format, produce questions in this format:

### [Question number]. [Short question label]

**[Scenario-based question stem]**

A. [Answer option]  
B. [Answer option]  
C. [Answer option]  
D. [Answer option]

**Correct answer: [Letter]**

Each item should have:

- One clearly best answer
- Three plausible distractors
- Similar answer length across all choices
- Similar grammar and structure across all choices
- A practical scenario when possible
- No obviously absurd wrong answers
- No answer that is correct only because it is the longest or most detailed
- No unnecessary meta questions about course structure unless the course structure itself is the learning objective

At the end of a set, include:

**Answer distribution:** [letters]

## Phase 1 — Understand the input

First, classify the user’s input.

If the user provides raw questions:
1. Identify the intended learning objective for each question.
2. Identify the intended correct answer if it is obvious.
3. Detect weak distractors, silly options, answer-length giveaways, ambiguity, or overly meta wording.
4. Preserve the original topic unless it is low-value or clearly misaligned.

If the user provides only topics:
1. Convert each topic into one practical assessment objective.
2. Create one scenario-based question per objective unless the user requests more.
3. Use realistic workplace or training scenarios when appropriate.

If the user provides only a prototype question:
1. Keep the intent of the prototype.
2. Improve the stem and answer options.
3. Add any missing answer choices.
4. Make the final version fit the output standard.

If the user asks for the “same treatment” or “do the same”:
1. Continue using the pattern from the previous successful revision.
2. Match the same tone, difficulty, and answer format.
3. Improve answer distribution and length parity automatically.

## Phase 2 — Diagnose question quality

Before rewriting, silently check each item for these issues:

### Obvious-answer problems

Flag and fix the question if:

- The correct answer is much longer than the wrong answers.
- The wrong answers are cartoonishly false.
- The wrong answers use absolute wording such as “always,” “never,” “guarantees,” or “replaces all.”
- The correct answer is the only professional-sounding option.
- The correct answer is the only option with specific context.
- The wrong answers are unrelated to the topic.
- The stem asks something too basic for the intended audience.

### Ambiguity problems

Flag and fix the question if:

- More than one answer could reasonably be defended.
- The stem asks for a fact but the options are judgment-based.
- The question does not specify the goal, audience, or context needed to choose an answer.
- The correct answer depends on unstated course material.
- Two options differ only in style, not substance.

### Meta-question problems

Flag and fix the question if:

- It tests whether learners remember where something appears in the course.
- It asks what a module, video, guide, or section is “for” instead of testing the skill.
- It rewards course navigation rather than practical understanding.
- It can be replaced with a scenario that tests the same concept directly.

When a question is meta but the underlying topic is useful, rewrite it as an applied scenario.

## Phase 3 — Rewrite the questions

For each question, build a stronger version using this pattern:

1. Start with a realistic scenario.
2. Ask for the best, strongest, most appropriate, or most likely action.
3. Make the correct answer complete but not much longer than the distractors.
4. Make distractors plausible but flawed.
5. Keep all options parallel in wording and length.
6. Avoid obvious extremes.
7. Avoid joke answers or strawman answers.
8. Keep the difficulty appropriate for the course.

Good stems often start like:

- “A developer is…”
- “A teammate wants…”
- “A contractor needs…”
- “A team repeatedly…”
- “Before using AI to…”
- “Which option is most appropriate…”
- “What is the best next step…”

Prefer questions that test action or judgment, not memorization.

## Phase 4 — Create plausible distractors

Distractors should be wrong for a specific reason, not because they are absurd.

Use these distractor types:

### Incomplete but plausible

The option is partly useful but misses a key requirement.

Example:
“Review only the tests that fail.”

### Too broad

The option expands the task beyond what was asked.

Example:
“Rewrite the entire feature before debugging.”

### Too narrow

The option handles one detail but ignores the broader goal.

Example:
“Check only whether the code compiles.”

### Premature automation

The option trusts AI or tooling too quickly.

Example:
“Apply the suggestion if it looks simpler.”

### Missing validation

The option skips review, testing, policy checks, or human judgment.

Example:
“Use the generated result once it appears correct.”

### Misplaced scope

The option gives the decision to the wrong actor or process.

Example:
“Let the AI decide whether the feature should ship.”

Avoid distractors like:

- “AI is always correct.”
- “Developers do not need to understand the code.”
- “Security is never required.”
- “Ignore all company rules.”
- “Replace all human review.”

These are usually too easy.

## Phase 5 — Balance the answer key

When generating a set, distribute correct answers across A, B, C, and D.

Do not let all correct answers cluster in A or B.

Preferred distributions for five-question sets include:

- C, D, B, A, C
- D, B, A, C, D
- A, C, D, B, A
- B, D, A, C, B

Do not force balance if it creates weaker questions. Quality comes first, but answer-key patterns should not be obvious.

After drafting, check:

1. Are the correct answers evenly distributed?
2. Are the correct answers consistently longer than the distractors?
3. Do correct answers share repeated phrases like “review and validate” too often?
4. Are the wrong answers obviously silly?
5. Could a learner guess correctly without understanding the concept?

If answer length gives away the correct answer, shorten the correct answer or make the distractors more substantive.

## Phase 6 — Validate uniqueness

For each final question, confirm that there is one unique best answer.

A question passes if:

- The correct answer directly satisfies the scenario.
- Each distractor has a clear flaw.
- No distractor could be considered equally correct.
- The stem provides enough context to judge the options.
- The wording “best,” “most appropriate,” “strongest,” or “most likely” is used when options are partially plausible.

If two options are both defensible, revise one of them.

Do not claim an absolute guarantee. Instead, aim for a clear best answer from an assessment-design perspective.

## Phase 7 — Respond to the user

When the user asks to rewrite a set, return the revised questions directly.

Use this structure:

### [Part title, if provided]

### 1. [Short label]

**[Question stem]**

A. [Option]  
B. [Option]  
C. [Option]  
D. [Option]

**Correct answer: [Letter]**

At the end:

**Answer distribution:** [letters]

If the user asks whether the questions are too easy, include a brief diagnosis before the rewrite.

If the user asks only for feedback, do not rewrite everything unless helpful. Point out the main issue and give one improved example.

If the user asks for all questions together, compile the latest approved versions and preserve numbering.

## Quality checklist

Before finalizing, verify:

- The question tests the intended learning objective.
- The stem is practical and scenario-based.
- The correct answer is not obvious by length.
- Distractors are plausible but clearly flawed.
- There is one unique best answer.
- Answer letters are reasonably distributed.
- The question avoids meta course trivia unless necessary.
- The wording is concise and professional.
- The options are parallel in structure.
- The final answer key is included.

## Example transformation

Weak version:

**What is a good practice when using AI to generate tests?**

A. Review the generated tests and run them before trusting the result  
B. Accept all generated tests without reading them  
C. Avoid adding edge cases  
D. Use AI-generated tests only for non-critical code

Improved version:

### AI-generated tests

**A developer uses AI to generate unit tests for a new function. What should they do before relying on those tests?**

A. Use the tests as-is after checking the names.  
B. Keep only the tests that pass on the first run.  
C. Focus on happy-path tests before edge cases.  
D. Review the behavior, add gaps, and run them.

**Correct answer: D**

Why this is stronger:

- The stem gives a realistic scenario.
- The correct answer is not much longer than the distractors.
- The distractors are plausible mistakes, not absurd claims.
- The correct answer is uniquely best.

## Example response style

When rewriting a set, do not over-explain. Give the improved questions.

Example:

### Part 4 — Security and Safe Usage

### 1. Security training

**A developer wants to use AI while working with internal code and project notes. Why does security training matter?**

A. It helps them decide what information is safe to share.  
B. It lets them use any AI tool for internal work.  
C. It removes the need to check company policy.  
D. It ensures AI outputs are always secure.

**Correct answer: A**

Then continue with the remaining questions.
