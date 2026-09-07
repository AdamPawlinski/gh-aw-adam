---
engine: copilot
on:
  schedule: every 6h
  workflow_dispatch:
permissions:
  contents: read
  copilot-requests: write
  issues: read
  pull-requests: read
tools:
  github:
    lockdown: false
  edit: true
  web-fetch: {}
network:
  allowed:
    - github.github.com
safe-outputs:
  create-pull-request:
    max: 1
    allowed-files:
      - index.html
---

# Daily Updates Highlights

## Task

Each run of this workflow:

1. Fetch the GitHub Agentic Workflows FAQ from https://github.github.com/gh-aw/reference/faq/
2. Determine today's UTC date (e.g., "7th of September")
3. Read index.html to check for existing Daily Updates dialogs and identify which FAQ questions are already represented
4. If today's date already has a Daily Updates dialog that contains a real FAQ (not a placeholder), make no changes and stop
5. If no unused FAQ questions remain, make no changes and stop
6. Select exactly one FAQ question and answer that are NOT already represented in index.html
7. Update index.html:
   - If today's date doesn't exist yet, add a new navigation button in the daily-updates-list (following the existing pattern with proper order)
   - If today's date doesn't exist yet, add a new dialog element below the existing dialogs (following the existing structure, ID conventions, accessibility attributes, and styling)
   - Preserve all existing Daily Updates navigation items and dialogs exactly as-is
8. Create a single pull request with these changes (if any)

## FAQ Selection Strategy

When selecting an unused FAQ:
- Prioritize "Capabilities" and "Guardrails" sections for practical, frequently-asked questions
- Avoid questions already in index.html (currently: "I like deterministic CI/CD. Isn't this non-deterministic?")
- Avoid placeholder dialogs (e.g., the September 6 entry)
- Choose questions with clear, actionable answers
- Ensure the question title and answer are accurate to the source FAQ

## HTML Structure Requirements

Match the existing HTML conventions exactly:

**Navigation Button Format** (within `<ul class="daily-updates-list">`):
```html
<li>
  <button
    class="daily-update-trigger"
    type="button"
    aria-haspopup="dialog"
    aria-controls="[date-id]-dialog"
    data-dialog-trigger
  >
    <span>[Ordinal date text]</span>
    <span aria-hidden="true">&#8594;</span>
  </button>
</li>
```

**Dialog Format** (at end of document before closing `</body>`):
```html
<dialog
  class="daily-update-dialog"
  id="[date-id]-dialog"
  aria-labelledby="[date-id]-question"
  aria-describedby="[date-id]-answer"
>
  <article class="daily-update-dialog-content">
    <header class="daily-update-dialog-header">
      <p>Daily Update / [Ordinal date text]</p>
      <form method="dialog">
        <button class="dialog-close" type="submit" aria-label="Close dialog" title="Close dialog">
          <span aria-hidden="true">&#10005;</span>
        </button>
      </form>
    </header>
    <h2 id="[date-id]-question">[FAQ Question]</h2>
    <p id="[date-id]-answer">
      [Concise, accurate FAQ answer - 2-3 sentences maximum]
    </p>
  </article>
</dialog>
```

**ID Convention**: Use date format like `august-1-dialog`, `september-6-dialog`, `september-7-dialog` (lowercase month, number, suffix omitted from IDs).

**Date Text Format**: Use ordinal format like "1st of August", "6th of September", "7th of September".

Never duplicate dates, navigation buttons, dialogs, or FAQ content. Do not modify existing entries.
