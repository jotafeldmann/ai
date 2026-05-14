---
name: fireflies-transcript-export
description: Extract the complete transcript from a Fireflies.ai meeting page and download it as a Markdown file
---

# Fireflies Transcript Export

Author: Jota Feldmann

## Intro

  The `fireflies-transcript-export` skill extracts a complete transcript from a Fireflies.ai meeting page at `app.fireflies.ai/view/*`, formats each speech block with speaker and timestamp, and downloads the result as a Markdown file.

  Use this skill when you are already on a Fireflies.ai meeting page and need a local `.md` transcript export.

## Phase 1 — Orient and prepare the page

You are on a Fireflies.ai meeting page matching:

```text
app.fireflies.ai/view/*
```

Start by taking a screenshot to orient yourself on the page and confirm that the meeting page has loaded.

Then press `Escape` to close any popups, overlays, modals, cookie banners, or other UI elements that may block access to the transcript.

## Phase 2 — Locate the transcript container

Find all Fireflies ScrollArea viewports using:

```js
document.querySelectorAll('.ScrollArea-styled__Viewport-sc-59e946dc-1')
```

Select the viewport with the largest `scrollHeight` value. This viewport contains the transcript.

Navigate to the transcript container using:

```js
viewport.firstElementChild.firstElementChild
```

The transcript speech blocks should be available as:

```js
container.children
```

## Phase 3 — Extract transcript metadata

Extract the meeting title from `document.title` by removing:

```text
 - Meeting recording by Fireflies.ai
```

Extract the meeting date from `document.body.innerText` using this regex:

```js
/\w+\s+\d+\s+\d{4},\s+[\d:]+\s+[AP]M/
```

Create the filename from the meeting title before `" - Meeting recording"` and append `.md`.

Sanitize invalid filename characters before download.

## Phase 4 — Parse speech blocks

Parse each speech block from `container.children` using this regex:

```js
/^([A-Z][^\d]*?)\s*(\d{2}:\d{2})\s*([\s\S]*)$/
```

Regex groups:

```text
[1] Speaker name
[2] Timestamp
[3] Speech text
```

Clean whitespace in all extracted text using:

```js
.replace(/\s+/g, ' ')
```

Format each transcript line exactly as:

```text
Speaker Name [HH:MM]: speech text
```

## Phase 5 — Build and download the Markdown file

Build the Markdown output in this structure:

```md
# Meeting Title

Meeting Date: Mon DD, YYYY, HH:MM AM/PM

---

Speaker [HH:MM]: speech text
Speaker [HH:MM]: speech text
```

Generate the download using a `Blob` and a temporary `a` element:

1. Create a Blob with the Markdown content and type `text/markdown;charset=utf-8`
2. Create a link element with `href = URL.createObjectURL(blob)`
3. Set `download` to the sanitized filename
4. Append the link to `document.body`
5. Click the link
6. Remove the link
7. Revoke the object URL

## Complete Browser Console Script

Run this script on the Fireflies.ai meeting page:

```js
(() => {
  const TITLE_SUFFIX = ' - Meeting recording by Fireflies.ai';

  const sanitizeFilename = (name) =>
    name
      .replace(/[<>:"/\\|?*\x00-\x1F]/g, '')
      .replace(/\s+/g, ' ')
      .trim()
      .slice(0, 180);

  const cleanText = (text) =>
    text
      .replace(/\s+/g, ' ')
      .trim();

  const viewports = [...document.querySelectorAll('.ScrollArea-styled__Viewport-sc-59e946dc-1')];

  if (!viewports.length) {
    throw new Error('No Fireflies ScrollArea viewports found.');
  }

  const viewport = viewports.sort((a, b) => b.scrollHeight - a.scrollHeight)[0];

  if (!viewport) {
    throw new Error('Could not identify transcript viewport.');
  }

  const container = viewport.firstElementChild?.firstElementChild;

  if (!container) {
    throw new Error('Could not find transcript container at viewport.firstElementChild.firstElementChild.');
  }

  const speechPattern = /^([A-Z][^\d]*?)\s*(\d{2}:\d{2})\s*([\s\S]*)$/;

  const transcriptLines = [...container.children]
    .map((block) => cleanText(block.innerText || block.textContent || ''))
    .map((blockText) => {
      const match = blockText.match(speechPattern);

      if (!match) {
        return null;
      }

      const speaker = cleanText(match[1]);
      const timestamp = cleanText(match[2]);
      const speech = cleanText(match[3]);

      if (!speaker || !timestamp || !speech) {
        return null;
      }

      return `${speaker} [${timestamp}]: ${speech}`;
    })
    .filter(Boolean);

  const meetingTitle = document.title
    .replace(TITLE_SUFFIX, '')
    .replace(' - Meeting recording', '')
    .trim();

  const dateMatch = document.body.innerText.match(/\w+\s+\d+\s+\d{4},\s+[\d:]+\s+[AP]M/);
  const meetingDate = dateMatch ? dateMatch[0] : 'Date not found';

  const markdown = [
    `# ${meetingTitle}`,
    '',
    `Meeting Date: ${meetingDate}`,
    '',
    '---',
    '',
    ...transcriptLines
  ].join('\n');

  const filename = `${sanitizeFilename(meetingTitle || 'fireflies-transcript')}.md`;

  const blob = new Blob([markdown], { type: 'text/markdown;charset=utf-8' });
  const url = URL.createObjectURL(blob);

  const link = document.createElement('a');
  link.href = url;
  link.download = filename;

  document.body.appendChild(link);
  link.click();
  link.remove();

  URL.revokeObjectURL(url);

  console.log(`Downloaded ${filename}`);
  console.log(`Extracted ${transcriptLines.length} speech blocks.`);

  if (transcriptLines.length < 251) {
    console.warn(`Only ${transcriptLines.length} speech blocks were extracted. Verify the transcript fully loaded before running again.`);
  }
})();
```

## Verification

After the Markdown file downloads, verify that:

1. The first line is the meeting title as a level-one heading.
2. The meeting date appears below the title.
3. Every transcript line follows this format:

```text
Speaker Name [HH:MM]: speech text
```

4. The file contains all expected speech blocks, usually 251 or more.
5. Speakers, timestamps, and transcript text are properly preserved.
