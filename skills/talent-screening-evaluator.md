---
name: talent-screening-evaluator
description: use this skill to help non-technical talent teams evaluate candidates against any job description. trigger when the user provides a job description, screening guidelines, recruiter notes, interview transcript, resume, or candidate answers and asks whether the candidate should proceed. the skill extracts must-have criteria, creates simple non-technical screening questions, scores candidate evidence, identifies red flags, and outputs a clear continue or reject recommendation using a strict minimum score.
---

# Talent Screening Evaluator

## Purpose

Evaluate whether a candidate should continue in a hiring process based on a job description, role-specific guidelines, and candidate evidence such as resume notes, recruiter screening answers, interview transcript, or call notes.

This skill is designed for non-technical recruiters and talent teams. Keep the evaluation simple, direct, and easy to copy into ATS notes or share with hiring stakeholders.

## Required Inputs

Use any available inputs the user provides:

1. Job description or role requirements
2. Hiring manager guidelines or must-have criteria
3. Candidate resume or LinkedIn notes
4. Screening transcript or recruiter notes
5. Candidate answers to screening questions
6. Any custom scoring rule provided by the user

If some inputs are missing, make the best possible evaluation from the available evidence. Do not invent evidence.

## Default Decision Rule

Use a 0 to 10 score unless the user provides another scale.

Decision:
7 to 10: Continue
0 to 6: Reject

Default rule:
If the candidate scores below 7, recommend rejection.

Only recommend Continue when the candidate shows enough evidence against the role’s most important requirements.

## Evaluation Priorities

First identify the role’s must-have criteria.

Separate them into:

1. Core technical or functional requirements
2. Role seniority expectations
3. Domain or industry requirements
4. Communication and collaboration expectations
5. Ownership, autonomy, or leadership expectations
6. Nice-to-have requirements

Must-have criteria carry more weight than nice-to-haves.

A candidate should not continue only because of nice-to-haves if they are weak on must-haves.

## How to Evaluate Evidence

Use only evidence from the provided material.

Classify each major requirement as:

Clear evidence
Some evidence
Weak evidence
No evidence

Raise the bar for senior, staff, principal, lead, architect, or manager roles.

For senior roles, look for:
1. Real ownership, not only participation
2. Production experience, not only theoretical knowledge
3. Clear examples, not generic statements
4. Tradeoff thinking
5. Communication with non-technical stakeholders
6. Ability to explain decisions clearly
7. Practical judgment, not just tool name-dropping

## Handling Technical Topics for Non-Technical Recruiters

Do not require the recruiter to validate deep technical correctness.

Instead, evaluate whether the candidate gives practical signals:

Good signs:
1. Gives real project examples
2. Explains what they personally did
3. Uses relevant role-specific terms naturally
4. Explains tradeoffs in simple language
5. Connects decisions to business, users, reliability, speed, cost, quality, or maintainability
6. Can explain what went wrong and how they fixed it

Red flags:
1. Uses buzzwords without examples
2. Cannot explain their own listed skills
3. Gives generic answers that could apply to any role
4. Does not mention the role’s core tools or concepts when directly asked
5. Cannot explain tradeoffs
6. Avoids ownership
7. Gives answers that are too shallow for the seniority level
8. Needs excessive prompting for must-have areas

## If Asked to Create Screening Questions

Create 2 or 3 questions maximum.

Each question should:
1. Be simple enough for a non-technical recruiter to ask
2. Cover multiple must-have criteria
3. Ask for a real example or design approach
4. Help distinguish practical experience from surface familiarity
5. Include what to listen for
6. Include red flags
7. Include one useful follow-up

Question format:

Question:
[Simple question]

What to listen for:
[Short list of expected signals]

Good follow-up:
[One follow-up question]

Red flags:
[Short list of concerns]

## If Asked to Score a Candidate

Use this structure.

Candidate: [Name]
Score: [X]/10
Decision: Continue or Reject

Reason:
[2 to 4 short sentences explaining the decision]

Positive signals:
[List only the strongest evidence]

Gaps or concerns:
[List only the most important gaps]

Final recruiter note:
[Short copy/paste summary for Talent team]

## If Comparing Multiple Candidates

Avoid tables unless the user asks for them.

Use this structure:

Candidate 1: [Name]
Score: [X]/10
Decision: Continue or Reject
Reason: [Short reason]

Candidate 2: [Name]
Score: [X]/10
Decision: Continue or Reject
Reason: [Short reason]

Final comparison:
[Name] showed the strongest alignment because [reason].
[Name] should not continue because [reason].

## Scoring Guidance

Use this scale:

10: Excellent match. Clear evidence across all core requirements, seniority, ownership, and communication.
9: Very good match. Minor gaps only.
8: Good match. Enough evidence to continue.
7: Minimum acceptable signal. Continue, but flag areas to validate.
6: Not enough signal. Reject unless the user explicitly overrides the rule.
5: Weak fit. Several must-have gaps.
4: Poor fit. Mostly generic or shallow evidence.
3: Very weak fit. Little role-specific evidence.
2: Almost no relevant evidence.
1: Not aligned.
0: No usable evidence or clear mismatch.

Do not inflate scores. If evidence is missing, score lower.

## Output Style

Keep outputs short, clear, and easy to share.

Use plain language.
Avoid jargon unless it appears in the job description.
Avoid long explanations.
Avoid tables unless requested.
Avoid over-positive language.
Be direct about rejection when the score is below the threshold.

Use this decision wording:

Decision: Continue
Decision: Reject

Do not use “borderline” unless the user explicitly asks for a borderline category.

## Copy/Paste Summary Template

Use this when the user needs a short message for the Talent team:

Candidate: [Name]
Score: [X]/10
Decision: [Continue or Reject]

Reason:
[One concise paragraph]

Positive signals:
[Short bullets or short lines]

Main concerns:
[Short bullets or short lines]

Recommendation:
[Clear recommendation in one sentence]

## Special Rule: Missing Evidence

If the transcript, resume, or notes do not contain enough information, say so clearly.

Use:

There is not enough evidence to recommend continuing.

Then explain what is missing.

Do not assume the candidate has a skill just because it appears in the job title or resume keywords.

## Special Rule: User Provides a Minimum Score

If the user defines a minimum score, use it.

Example:
If the user says the minimum is 7, apply:
7 to 10: Continue
0 to 6: Reject

Do not create a borderline category unless requested.

## Special Rule: Role-Specific Screening Guide

If the user provides role-specific screening questions or guidelines, treat them as the source of truth.

Evaluate whether:
1. The question was asked
2. The candidate answered it directly
3. The answer provided role-specific evidence
4. The answer met the expected seniority level
5. The answer supports Continue or Reject

## Final Check Before Answering

Before finalizing, verify:

1. Did I use only provided evidence?
2. Did I identify the role’s must-haves?
3. Did I apply the minimum score correctly?
4. Did I avoid a borderline category unless requested?
5. Is the recommendation clear enough for Talent to use?
6. Is the output short enough to copy and paste?
