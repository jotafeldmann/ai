---
name: hackerrank-question-mcp
version: 1.0.0
description: Generate HR-style multiple-choice questions and export them as clean, structured CSV
---

# HR MCQ CSV Generator

Author: Jota Feldmann

## Intro

The `hr-mcq-csv` skill generates HR-style multiple-choice questions (MCQs) and exports them in a structured, comma-separated CSV format. It is designed for assessment, training, recruiting, onboarding, certification, and knowledge-check use cases.

The skill creates clear, logical, non-repetitive questions based on user-provided topics, documents, descriptions, job roles, competencies, policies, or technical/conceptual learning objectives.

## Output contract

The final output must be export-ready CSV using a comma as the separator.

Do not wrap the CSV in a Markdown code fence unless the user explicitly requests it.

The CSV must include the following columns in this exact order:

```csv
Name,Tags,Recommended_time,Description,Notes,Answers,Score,A,B,C
```

Answer columns are dynamic and must be added as needed:

```csv
Name,Tags,Recommended_time,Description,Notes,Answers,Score,A,B,C,D,E,F
```

Every row must follow this schema:

| Column | Required behavior |
|---|---|
| Name | Short identifier for the question, such as `Q001`, `HR001`, or a topic-based ID |
| Tags | A relevant tag or tag group for the question |
| Recommended_time | Always `1` |
| Description | The question text |
| Notes | Always empty unless the user explicitly requests notes |
| Answers | Correct answer letter or comma-separated letters for multiple correct answers, such as `A` or `"A,B"` |
| Score | Always `1` |
| A, B, C, etc. | Possible answer choices |

## CSV formatting rules

1. Use a comma as the separator.
2. Include the header row.
3. Quote any field that contains a comma, double quote, or line break.
4. Escape double quotes inside quoted fields by doubling them.
5. Keep `Notes` empty by leaving the field blank between commas.
6. Because multiple correct answers contain commas, quote them in the `Answers` column, for example `"A,B"`.
7. Ensure every row has the same number of columns as the header.
8. Do not add explanatory text before or after the CSV unless the user asks for it.

## Question-generation rules

Generate questions that are:

- Clear and concise
- HR-style and assessment-ready
- Logical and answerable from the provided material or requested topic
- Non-repetitive across the set
- Balanced across conceptual, practical, and technical topics when applicable
- Free of ambiguity, unless the user explicitly wants scenario-based judgment questions
- Appropriate for the requested audience, seniority level, job role, or training context

Avoid:

- Trick questions
- Duplicated questions with minor wording changes
- Overlapping answer choices
- Multiple correct answers unless the question clearly signals that more than one answer may be correct
- Answers such as “all of the above” or “none of the above” unless explicitly useful
- Unsupported facts not present in the provided source material when the user asks for source-based questions

## Workflow

### Phase 1 — Understand the request

Use the user’s input to determine:

1. The topic, source material, or competency area
2. The number of questions requested
3. The intended audience or difficulty level, if provided
4. Whether questions should be single-answer or may include multiple correct answers
5. The appropriate tags
6. The number of answer choices per question

If the user does not specify a number of questions, generate 10 questions.

If the user does not specify the number of answer choices, use 4 choices: A, B, C, and D.

If the user does not specify tags, create concise tags from the topic.

### Phase 2 — Draft questions

For each question:

1. Create a short identifier in the `Name` column.
2. Write one clear question in the `Description` column.
3. Create plausible answer choices.
4. Mark the correct answer letter in the `Answers` column.
5. Set `Recommended_time` to `1`.
6. Leave `Notes` empty.
7. Set `Score` to `1`.

For multiple-answer questions:

1. Make it clear in the question text that more than one answer may be selected.
2. Put the correct letters in the `Answers` field as comma-separated values.
3. Quote the `Answers` field, for example `"A,C"`.

### Phase 3 — Validate the CSV

Before returning the final CSV, check that:

1. The header columns are in the required order.
2. Every row has the same number of fields as the header.
3. Each question has exactly one `Name`.
4. `Recommended_time` is always `1`.
5. `Notes` is empty unless requested otherwise.
6. `Score` is always `1`.
7. The `Answers` value matches valid answer columns.
8. Multiple correct answers are quoted.
9. Answer choices are not repeated within a question.
10. Questions are not repeated across the set.

## Default CSV header

Use this header for four-option questions:

```csv
Name,Tags,Recommended_time,Description,Notes,Answers,Score,A,B,C,D
```

Use this header for five-option questions:

```csv
Name,Tags,Recommended_time,Description,Notes,Answers,Score,A,B,C,D,E
```

## Example output

```csv
Name,Tags,Recommended_time,Description,Notes,Answers,Score,A,B,C,D
HR001,Recruitment,1,What is the main purpose of a structured interview?,,B,1,To make the interview shorter,To evaluate candidates consistently,To avoid asking role-related questions,To let candidates choose the questions
HR002,Workplace Safety,1,"Which actions help maintain a safe workplace? Select all that apply.",,"A,C",1,Reporting hazards promptly,Ignoring minor incidents,Following safety procedures,Disabling safety equipment for speed
```

## Final response behavior

When the user asks for questions, return only the CSV unless they request explanation, review, or commentary.

When the user asks for a downloadable file, create a `.csv` file or `SKILL.md` file as requested and provide the download link.

When the user provides raw source content, base the questions on that content and avoid adding unsupported details.
