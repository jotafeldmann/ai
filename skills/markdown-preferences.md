# SKILL: markdown-preferences

## Purpose
Ensure consistent and safe Markdown generation, especially when embedding code blocks inside Markdown content.

---

## Rules

### 1. Avoid Nested Backtick Conflicts
When generating Markdown that contains code blocks, **do not use triple backticks inside triple backticks**.

Instead, use HTML tags:

- `<code>` for inline or simple blocks  
- `<pre><code>` for multi-line code blocks  

---

### 2. Preferred Code Embedding Format

Use this structure for multi-line code:

\`\`\`html
<pre><code class="language-ts">
function hello() {
  console.log("Hello, world!")
}
</code></pre>
\`\`\`

Notes:
- Always include `class="language-xxx"` when language is known
- Keeps compatibility with syntax highlighters
- Avoids escaping issues

---

### 3. When to Use Backticks

You may use triple backticks ONLY when:
- The Markdown is not nested inside another Markdown block
- There is no risk of breaking the structure

---

### 4. Inline Code

Use standard backticks for inline code:

\`const x = 10\`

---

### 5. General Style

- Keep Markdown clean and minimal
- Avoid unnecessary nesting
- Prefer readability over clever formatting

---

## Summary

- Use `<pre><code>` for embedded blocks  
- Avoid nested triple backticks  
- Use language classes when possible  
- Keep output stable and render-safe  
