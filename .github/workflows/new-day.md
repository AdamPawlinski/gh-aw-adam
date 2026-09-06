---
name: New Day
description: Add the current UTC date to the Daily Updates navigation in index.html and create a matching accessible confirmation dialog.
engine: copilot
on:
  schedule: daily
  workflow_dispatch:
permissions:
  contents: read
  copilot-requests: write
strict: true
tools:
  edit: true
safe-outputs:
  create-pull-request:
    allowed-files:
      - index.html
    max: 1
---

# New Day

Use the workflow run's current UTC date as the only date source for this task.

Edit only `index.html`. Do not modify `styles.css` or any other file.

Inspect the existing Daily Updates navigation, dialog markup, and related script in `index.html` before making changes. Follow the current HTML structure, class names, element order, ID conventions, date wording, accessibility attributes, and overall tone already used on the page.

Derive the visible date label from the current UTC date using the same wording pattern already present in the page: ordinal day number followed by `of` and the full month name, such as `1st of August`.

Derive matching IDs from that date using the existing lowercase month-day convention:
- dialog id: `<month>-<day>-dialog`
- question id: `<month>-<day>-question`
- answer id: `<month>-<day>-answer`

In `index.html`:

1. In the existing Daily Updates navigation list, add one new list item containing a button that matches the current structure exactly:
   - class `daily-update-trigger`
   - `type="button"`
   - `aria-haspopup="dialog"`
   - `aria-controls` pointing to the new dialog id
   - `data-dialog-trigger`
   - visible text set to the current UTC date label

2. Add one matching accessible `<dialog>` that uses the existing dialog structure and classes already present in the file:
   - class `daily-update-dialog`
   - `id` set to the derived dialog id
   - `aria-labelledby` set to the derived question id
   - `aria-describedby` set to the derived answer id
   - header text in the same format: `Daily Update / <date label>`
   - close button structure unchanged from the existing dialog pattern

3. Make the new dialog confirm that the daily update ran for the current UTC date.
   - Keep the text concise.
   - Use the existing heading-and-paragraph structure.
   - Keep the confirmation factual and accessible.

Do not remove, rewrite, reorder, or duplicate any existing daily update entry or dialog.

Before making changes, check whether the current UTC date is already present in any of these places:
- the Daily Updates navigation text
- the target dialog id
- the target question id
- the target answer id

If the current UTC date is already present, make no file changes and call `noop` with a short reason that names the existing UTC date.

If the current UTC date is not present, preserve every existing daily update and append the new navigation item and the new dialog in the same style as the existing entries.

Use only the configured `create-pull-request` safe output for the visible write action. Do not modify any file other than `index.html`, and do not create more than one pull request.