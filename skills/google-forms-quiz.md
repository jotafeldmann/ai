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

Before opening or modifying any browser tab, check the user's input against every rule below. If any rule fails, stop immediately and report the issue — do not touch the form.

Required fields:

| Field | Rule |
|---|---|
| Form title | Non-empty string |
| Questions | At least 1 |
| Options per question | At least 2 labeled choices (e.g. A, B, C…) |
| Correct answer per question | At least 1 option explicitly marked correct |

If validation fails, respond with exactly this message and stop:

> "I can't proceed — question [N] ('[question title]') is missing a correct answer. Please specify which option is correct before I continue."

If all questions pass, confirm to the user:

> "Input validated — [N] question(s) found, all with correct answers. Starting form configuration now."

Then proceed to Phase 2.

---

## Phase 2 — Configure the form

You are the form configurator. Apply each step in order, taking a screenshot after every significant action to confirm the result before moving on.

---

### Step 1 — Set the form title

There are TWO separate title fields in Google Forms that must both be updated:

**1a. The form header card title** (visible inside the form canvas)

Locate the first `[contenteditable="true"]` element whose `getBoundingClientRect().top` is between 80–200px. Use the execCommand pattern:

```js
el.focus();
document.execCommand('selectAll', false, null);
document.execCommand('delete', false, null);
document.execCommand('insertText', false, newTitle);
el.dispatchEvent(new Event('input', { bubbles: true }));
```

**1b. The document name** (shown in the top toolbar and browser tab)

This is a SEPARATE field from the form canvas. It lives in the top navigation bar (next to the Google Forms icon). It controls the browser tab label and the file name in Google Drive.

To update it:
1. Click directly on the document name text in the top toolbar (typically around coordinates y≈22, or use `find` to locate it).
2. Triple-click to select all existing text.
3. Type the new title.
4. Press Enter to confirm.
5. Click away into the form canvas to commit.

> **Critical:** If you only update the form header card title (1a) and skip the document name (1b), the browser tab will still show the old name (e.g. "Blank Quiz"). Always update both.

Take a screenshot. Confirm the new title appears in both the form header card AND the browser tab label before continuing.

---

### Step 2 — Configure each question

Repeat this sub-sequence for every question in the validated list:

**2a. Add a new question card**

Click the Add question button (`[aria-label="Add question"]`) using a MouseEvent with explicit `clientX`/`clientY` coordinates — a plain `.click()` without coordinates is often ignored:

```js
const btn = document.querySelector('[aria-label="Add question"]');
const rect = btn.getBoundingClientRect();
btn.dispatchEvent(new MouseEvent('click', {
  bubbles: true, cancelable: true,
  clientX: rect.left + rect.width / 2,
  clientY: rect.top + rect.height / 2
}));
```

The new card inserts immediately after the active one and defaults to **Multiple choice** — no type switch needed. Take a screenshot to confirm the empty card appeared.

**2b. Set the question title**

New question cards may render their `[contenteditable="true"]` with an empty string or with placeholder text that doesn't show up in `.textContent`. Identify the correct element by its `getBoundingClientRect().top` — target the one with the highest `top` value that is still within the viewport (> 600px typically). Do not rely on array index alone.

```js
const allEditable = document.querySelectorAll('[contenteditable="true"]');
const questionTitle = Array.from(allEditable).find(el => {
  const r = el.getBoundingClientRect();
  return r.top > 600 && r.left > 50;
});
questionTitle.focus();
document.execCommand('selectAll', false, null);
document.execCommand('delete', false, null);
document.execCommand('insertText', false, questionText);
questionTitle.dispatchEvent(new Event('input', { bubbles: true }));
```

Wait ~300ms after setting the title before proceeding to options.

**2c. Set answer options**

Answer option inputs require the native value setter — direct `.value =` assignment is silently ignored by the Forms framework:

```js
const nativeSetter = Object.getOwnPropertyDescriptor(
  window.HTMLInputElement.prototype, 'value'
).set;
nativeSetter.call(input, optionText);
input.dispatchEvent(new InputEvent('input', { bubbles: true, data: optionText }));
```

To locate the next empty option input after each Enter:

```js
Array.from(document.querySelectorAll('input[type="text"]'))
  .filter(inp => {
    const r = inp.getBoundingClientRect();
    return r.top > 100 && r.left > 100 && r.width > 50;
  })
  .find(inp => inp.value === '' || inp.value.startsWith('Option'));
```

To create the next option, dispatch Enter keydown/keyup on the current input **after** setting its value. Wait ~300ms before targeting the new input — it may not yet exist in the DOM.

On the **last option**, call `.blur()` instead of dispatching Enter.

**2d. Mark the correct answer**

1. Scroll the active card into view using the internal scroll container (not `window.scrollTo`).
2. Find and click the Answer key button for the **active card only** — always use the last one in the list:
```js
   const btns = document.querySelectorAll('[aria-label="Answer key and points"]');
   btns[btns.length - 1].click();
```
3. Wait ~500ms for the "Choose correct answers" panel to open. Take a screenshot to confirm it opened.
4. Click the radio button next to the correct option.
5. Confirm a green checkmark (✓) appears on the selected option. Take a screenshot.
6. Click **Done** to close the panel and save.
7. Take a screenshot after Done. The ✓ must be visible on the question card before moving on.

**2e. Set the question as required**

After closing the Answer key panel, enable the Required toggle:

```js
const requiredBtns = document.querySelectorAll('[aria-label="Required"]');
requiredBtns[requiredBtns.length - 1].click();
```

Using `[aria-label="Required"]` is more reliable than using the `find` tool (which may not discover all instances) or clicking by coordinate. Always target the last one in the list to affect the active card.

---

### Step 3 — Final confirmation

After all questions are configured, take a final screenshot from the top of the form, then tell the user:

> "Done — [N] question(s) configured with correct answers marked. You can preview the form by clicking the eye icon (Preview) in the top-right toolbar to test it as a respondent."

---

## Known pitfalls

| Pitfall | How to avoid |
|---|---|
| Document title (browser tab) not updated | The form has TWO separate title fields: the form canvas header card AND the document name in the top toolbar. Both must be updated. The document name is clicked directly in the top navigation bar and updated by triple-clicking → typing → Enter. |
| `find` tool misses some Required checkboxes | Use `document.querySelectorAll('[aria-label="Required"]')` and click `[length - 1]` instead of relying on the `find` tool, which may not enumerate all instances in a long form. |
| Add question button ignores plain `.click()` | Use `MouseEvent` with explicit `clientX`/`clientY` from `getBoundingClientRect()`. |
| New question card defaults inferred wrongly | Cards default to **Multiple choice** — no type switch is needed. Verify with a screenshot before adding options. |
| `execCommand` writes to wrong contenteditable | Identify the target by `getBoundingClientRect().top` (highest value still in viewport). Do not use array index — the form description field shares the same selector. |
| Direct `.value =` assignment silently ignored | Always use the native `HTMLInputElement` value setter via `Object.getOwnPropertyDescriptor`. |
| `window.scrollTo` has no effect | Use the internal scroll container: `document.querySelector('.freebirdFormeditorViewScrollContainer')` or find the first element where `scrollHeight > clientHeight + 200`. |
| Answer Key panel must be explicitly closed | Always click **Done**. Do not dismiss by clicking elsewhere. |
| Correct answer not confirmed before proceeding | Always take a screenshot after marking and verify the ✓ is visible on the card (not just inside the panel). |
| Empty contenteditable not found by text match | New cards render with an empty `.textContent`. Find them by position (`top > 600`), not by matching `'Question'` string — that text is a visual placeholder only and may not be in the DOM. |
