---
name: google-forms-quiz
description: Build and configure a Google Forms quiz from structured input — title, questions, options, and correct answers — with full validation before touching the browser.
---

# Google Forms Quiz

Author: Jota Feldmann

## Intro

The google-forms-quiz skill was originally created for the [Superpowers](https://github.com/obra/superpowers) plugin collection — an official set of reusable workflows for Claude Code.

> When you invoke /google-forms-quiz, the agent follows a two-phase workflow: it first validates all input data in memory (refusing to proceed if any correct answer is missing), then configures the live Google Forms editor question by question, confirming each step with screenshots before moving on.

---

## Phase 1 — Validate input

You are the input validator for a Google Forms quiz builder.

Before opening or modifying any browser tab, check the user's input against every rule below. If **any** rule fails, stop immediately and report the issue — do not touch the form.

**Required fields:**

| Field | Rule |
|---|---|
| Form title | Non-empty string |
| Questions | At least 1 |
| Options per question | At least 2 labeled choices (e.g. A, B, C…) |
| Correct answer per question | At least 1 option explicitly marked correct |

**If validation fails**, respond with exactly this message and stop:

> "I can't proceed — question [N] ('[question title]') is missing a correct answer. Please specify which option is correct before I continue."

**If all questions pass**, confirm to the user:

> "Input validated — [N] question(s) found, all with correct answers. Starting form configuration now."

Then proceed to Phase 2.

---

## Phase 2 — Configure the form

You are the form configurator. Apply each step in order, taking a screenshot after every significant action to confirm the result before moving on.

### Step 1 — Set the form title

Locate the title field (the first `[contenteditable="true"]` element in the page) and replace its content using the `execCommand` pattern:

```js
el.focus();
document.execCommand('selectAll', false, null);
document.execCommand('delete', false, null);
document.execCommand('insertText', false, newTitle);
el.dispatchEvent(new Event('input', { bubbles: true }));
```

Take a screenshot. Confirm the new title appears in both the form header and the browser tab label before continuing.

---

### Step 2 — Configure each question

Repeat this sub-sequence for every question in the validated list:

**2a. Add a new question card**

Click the Add question button (`[aria-label="Add question"]`) while the previous card is active. The new card inserts immediately after the active one. Take a screenshot to confirm the empty card appeared.

**2b. Set question type to Multiple Choice**

New cards may default to Paragraph or Short answer. Always switch type first:

1. Click the type-selector dropdown on the active card (right side of the card header).
2. Dispatch a `MouseEvent` with explicit `clientX/clientY` coordinates — a plain `.click()` without coordinates is often ignored by the Forms framework.
3. Click "Multiple choice" in the dropdown list.
4. Take a screenshot. Confirm the dropdown now reads "Multiple choice" before adding options.

**2c. Set the question title**

Use the `execCommand` pattern on the active card's `[contenteditable="true"]`. Identify the correct element by its `getBoundingClientRect().top` value — do not rely on array index alone, as the form description field shares the same selector and sits at index 1.

**2d. Set answer options**

Answer option inputs require the native value setter — direct `.value =` assignment is silently ignored by the Forms framework:

```js
const nativeSetter = Object.getOwnPropertyDescriptor(
  window.HTMLInputElement.prototype, 'value'
).set;
nativeSetter.call(input, optionText);
input.dispatchEvent(new InputEvent('input', { bubbles: true, data: optionText }));
```

To create the next option, dispatch an Enter keydown/keyup on the current input after setting its value. Wait ~250 ms before targeting the new input — it may not yet exist in the DOM.

To locate the next empty option input:

```js
Array.from(document.querySelectorAll('input[type="text"]'))
  .filter(inp => {
    const r = inp.getBoundingClientRect();
    return r.top > 100 && r.left > 100 && r.width > 50;
  })
  .find(inp => inp.value === '' || inp.value.startsWith('Option'));
```

On the last option, call `.blur()` instead of dispatching Enter.

**2e. Mark the correct answer**

1. Scroll the active card into view using the internal scroll container (not `window.scrollTo`).
2. Click the **Answer key** button at the bottom of the card.
3. In the "Choose correct answers" panel, click the radio button next to each correct option.
4. Confirm a green checkmark (✓) appears on the selected option.
5. Click **Done** to close the panel and save.
6. Take a screenshot. The ✓ must be visible before moving to the next question.

**2f. Set the question as required**
1. Scroll the active card into view using the internal scroll container (not `window.scrollTo`).
2. Click the **Required** to enable it
---

### Step 3 — Final confirmation

After all questions are configured, tell the user:

> "Done — [N] question(s) configured with correct answers marked. You can preview the form by clicking the **eye icon** (Preview) in the top-right toolbar to test it as a respondent."

---

## Known pitfalls

| Pitfall | How to avoid |
|---|---|
| New question defaults to Paragraph | Always check and switch to Multiple choice before adding options |
| `execCommand` writes to form description instead of question title | Identify the target by `getBoundingClientRect().top`, not by array index |
| Direct `.value =` assignment silently ignored | Always use the native HTMLInputElement value setter |
| `window.scrollTo` has no effect | Use the internal scroll container (first element where `scrollHeight > clientHeight + 200`) |
| Type dropdown ignores plain `.click()` | Use `MouseEvent` with `clientX/clientY`; verify result with screenshot |
| Answer Key panel must be explicitly closed | Always click **Done**; do not dismiss by clicking elsewhere |
| Correct answer not confirmed before proceeding | Always take a screenshot after marking and verify the ✓ is visible |
