---
name: hackerrank-question-database
description: create complete hackerrank database questions from shared hiring or technical assessment material. use when the user provides role requirements, a scenario, a sample question, schemas, expected outputs, or interview topics and wants all hacker rank database-question fields generated, including title, candidate-facing html description, postgres table setup sql, expected output, reference answer, interviewer guidelines, postgres validation steps, scoring level, and exact-output tests.
---

# HackerRank Question Database

Author: Jota Feldmann

## Goal

Create complete HackerRank database questions from shared technical material.

The output must help the user fill HackerRank database-question fields, especially:

1. Question title
2. Candidate-facing problem description in HackerRank HTML format
3. PostgreSQL database table setup SQL
4. Expected output
5. Reference answer query
6. Interviewer guidelines
7. PostgreSQL validation notes
8. Difficulty and points
9. Exact-output test query

Always assume PostgreSQL unless the user explicitly asks for another database.

## Required input

Expect the user to share some material, such as:

- A role description
- A client request
- A PDF or pasted text with topics to evaluate
- A rough scenario
- A sample HackerRank question
- Required skills or technologies
- Tables, columns, or expected output ideas
- A previous question to adapt

If the material is incomplete, make reasonable assumptions and clearly mark them in the generated content. Do not block unless the missing information prevents creating a runnable SQL question.

## Workflow

### 1. Understand the assessment target

Identify:

- the role being evaluated
- the core technical skills requested
- what can be tested automatically in SQL
- what should be left for interviewer follow-up
- whether the question is better as easy, medium, or hard

For database questions, keep the automated HackerRank part focused on SQL behavior that can be tested deterministically.

Good database-question targets include:

- joins
- grouping and aggregation
- filtering
- date logic
- deduplication
- window functions
- ranking
- latest-record selection
- currency or numeric calculations
- expected reporting grain
- data quality edge cases
- relational modeling reasoning

Do not claim that HackerRank validates vendor-specific runtime behavior unless the generated SQL actually runs in that database. For this skill, PostgreSQL is the default runtime.

### 2. Design the question

Create a realistic but small schema.

Prefer 3 to 6 tables. Keep sample data small enough to understand, but include enough edge cases to test the intended skill.

For medium and hard questions, include at least one edge case such as:

- duplicate rows
- late-arriving correction
- one-to-many join risk
- missing optional record
- date-based conversion
- null handling
- tie-breaking rule
- latest status record
- aggregation at the wrong grain

The candidate-facing task must clearly state:

- what the candidate must return
- the exact output columns
- whether ordering matters
- deduplication or calculation rules
- any assumptions about dates, currency, nulls, or latest records

### 3. Generate candidate-facing HTML

Use HackerRank HTML format with:

- `<style type="text/css">`
- `<div class="ps-content-wrapper-v0">`
- `<div class="preheader" style="display:none;">`
- problem statement paragraphs
- collapsible `<details>` section for `Schema`
- collapsible `<details>` section for `Sample Data Tables`
- sample output
- explanation

Keep the collapsible sections interactive using:

```html
<details><summary class="section-title">Schema</summary>
<div class="collapsable-details">
...
</div>
</details>
```

and:

```html
<details><summary class="section-title">Sample Data Tables</summary>
<div class="collapsable-details">
...
</div>
</details>
```

Use uppercase table names in visible table headers, but use lowercase table names in SQL unless the user asks otherwise.

### 4. Generate PostgreSQL table setup SQL

Create the content for HackerRank's `Tables` field.

It must include:

- `CREATE TABLE` statements
- `INSERT INTO` statements
- enough rows to produce the visible expected output
- edge cases required by the question

Use PostgreSQL-compatible syntax only.

Avoid syntax that may not run on PostgreSQL, such as:

- `QUALIFY`
- Snowflake-only functions
- BigQuery-only functions
- SQL Server-only date functions
- Oracle-only syntax
- MySQL-only syntax unless explicitly requested

Prefer PostgreSQL-compatible patterns:

```sql
WITH ranked AS (
    SELECT
        *,
        ROW_NUMBER() OVER (
            PARTITION BY entity_id, event_time
            ORDER BY updated_at DESC
        ) AS rn
    FROM source_table
)
SELECT *
FROM ranked
WHERE rn = 1;
```

For dates and timestamps, use explicit values:

```sql
'2024-05-01'::date
'2024-05-01 10:00:00'::timestamp
```

For decimal values, use `DECIMAL(10,2)` or `NUMERIC(10,2)`.

### 5. Generate expected output

Create the exact content for HackerRank's `Expected Output` field.

Rules:

- Match the selected reference answer query.
- Keep column order exactly aligned with the problem statement.
- Use deterministic ordering in the reference answer.
- Avoid relying on unordered output unless the platform clearly accepts any order.
- Prefer `ORDER BY` in the reference query and expected output.

If numeric formatting is likely to vary in PostgreSQL, make the reference query cast or format the value to produce stable output.

For numeric calculations that should display two decimals, prefer:

```sql
ROUND(value::numeric, 2)
```

If exact text output is required, prefer:

```sql
TO_CHAR(ROUND(value::numeric, 2), 'FM999999990.00')
```

Use text formatting only when needed. Otherwise keep values as numeric.

### 6. Generate the reference answer

Create a correct PostgreSQL answer query.

The answer must:

- run against the generated tables
- return exactly the expected columns
- include deterministic `ORDER BY`
- handle all documented edge cases
- avoid relying on `DISTINCT` as a substitute for correct logic
- use clear CTEs when the logic is not trivial

### 7. Generate interviewer guidelines

Create the content for HackerRank's optional `Interviewer guidelines` field.

Use hidden/internal HackerRank HTML format with:

- style block
- wrapper div
- collapsible `Solution`
- collapsible `What to look for`
- collapsible `Interview follow-up questions`
- collapsible `Suggested scoring`

The guidelines must include:

- concepts covered
- expected approach
- reference solution
- green flags
- red flags
- follow-up questions tied to the scenario
- pass, borderline, fail criteria

Do not show interviewer-only hints in the candidate-facing description.

### 8. Validate PostgreSQL compatibility

Before finalizing, check the SQL for PostgreSQL compatibility.

At minimum, verify:

- table creation syntax
- insert syntax
- date and timestamp casting
- CTE syntax
- window function syntax
- joins
- aliases
- numeric calculation syntax
- order by columns
- no vendor-specific functions from another database

If a PostgreSQL execution environment is available, run:

1. table setup SQL
2. reference answer query
3. exact-output test query

If execution is not available, state that the SQL was reviewed for PostgreSQL syntax but not executed.

### 9. Assign difficulty and points

At the end of every generated question, mention one of:

- Easy, 10 points
- Medium, 50 points
- Hard, 100 points

Use this rubric:

Easy, 10 points:
- 1 to 3 tables
- simple joins or filters
- no window functions
- no tricky grain
- no late-arriving data
- no multi-step transformation

Medium, 50 points:
- 3 to 5 tables
- joins plus aggregation or window function
- clear edge case such as deduplication, latest record, or date logic
- requires understanding output grain
- solvable in 20 to 40 minutes

Hard, 100 points:
- 5 or more tables, or complex multi-step logic
- multiple edge cases
- advanced window functions
- incremental-like or late-arriving data scenario
- risk of incorrect grain or double counting
- requires production-style reasoning
- may need 45 to 90 minutes

For most practical hiring SQL screens, prefer Medium unless the user asks for a high-bar senior/staff challenge.

## Required output format

When generating a full HackerRank database question, use this structure:

```markdown
# Title

[question title]

# Candidate-facing description

```html
[full HackerRank HTML description]
```

# Tables

```sql
[PostgreSQL CREATE TABLE and INSERT statements]
```

# Expected Output

```text
[exact expected output]
```

# Reference Answer

```sql
[correct PostgreSQL query]
```

# Interviewer Guidelines

```html
[internal HackerRank interviewer guidelines HTML]
```

# PostgreSQL Validation

[short note explaining what was checked or executed]

# Difficulty

[Easy, 10 points / Medium, 50 points / Hard, 100 points]

# Exact Output Test

```sql
[SQL test that compares the reference answer result against the expected output]
```
```

## Exact-output test requirement

Always add an `Exact Output Test` section at the end.

The test must compare the expected answer against the exact expected output.

For PostgreSQL, use this pattern:

```sql
WITH candidate_result AS (
    -- paste the reference answer query here without the final semicolon
),
expected_result AS (
    SELECT *
    FROM (
        VALUES
            (1, 'Spring Launch', 10, 'Prospecting', 100, 'Video A', '2024-05-01 10:00:00'::timestamp, 1100, 55, 132.00::numeric, 121.44::numeric),
            (2, 'Retargeting', 20, 'Returning Users', 200, 'Video B', '2024-05-01 10:00:00'::timestamp, 800, 40, 90.00::numeric, 82.80::numeric)
    ) AS t (
        campaign_id,
        campaign_name,
        ad_group_id,
        ad_group_name,
        ad_id,
        ad_name,
        report_hour,
        impressions,
        clicks,
        spend_usd,
        spend_eur
    )
),
diff AS (
    (
        SELECT * FROM candidate_result
        EXCEPT
        SELECT * FROM expected_result
    )
    UNION ALL
    (
        SELECT * FROM expected_result
        EXCEPT
        SELECT * FROM candidate_result
    )
)
SELECT
    CASE
        WHEN EXISTS (SELECT 1 FROM diff) THEN 'FAIL'
        ELSE 'PASS'
    END AS test_result;
```

The test must return:

```text
PASS
```

when the answer exactly matches the expected output.

For a generated question, adapt the `VALUES` list and column names to the actual expected output.

If the result includes decimals, cast expected numeric values explicitly with `::numeric`.

If the result includes timestamps, cast expected timestamps explicitly with `::timestamp`.

If result ordering matters in HackerRank output, ensure the reference answer includes `ORDER BY`. The exact-output test using `EXCEPT` validates set equality, not order. When order must also be tested, add a second test using `ROW_NUMBER()` over the ordered result and compare row positions.

## Quality bar

The generated question must be usable in HackerRank with minimal editing.

Before finalizing, check:

- Candidate-facing description and schema match the SQL tables.
- Sample data tables match the insert data.
- Expected output matches the reference answer.
- Reference answer runs in PostgreSQL syntax.
- Interviewer guidelines align with the actual question.
- Difficulty points are included.
- Exact-output test is included at the end.
