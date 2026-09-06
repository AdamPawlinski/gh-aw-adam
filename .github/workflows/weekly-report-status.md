---
name: Weekly Report Status
description: Publish a concise weekly activity report for the previous seven days.
engine: copilot
on:
  schedule:
    - cron: "0 9 * * 1"
  workflow_dispatch:
permissions:
  contents: read
  issues: read
  pull-requests: read
  copilot-requests: write
strict: true
tools:
  github:
    mode: gh-proxy
    toolsets: [default]
safe-outputs:
  create-issue:
    title-prefix: "[weekly-report] "
    close-older-issues: true
---

# Weekly Report Status

Review repository activity for the previous seven days ending at workflow start time in UTC.

Use `gh` through the configured GitHub tool to gather repository activity covering commits, issues, and pull requests for that window.

Create exactly one new issue with the configured `create-issue` safe output for the current run, and let the safe output close any older open weekly report issues from prior runs.

Use an issue title that clearly identifies the covered seven-day UTC window.

Keep the report concise, factual, and easy to scan. Use this structure in the issue body:

### Overview
- Report window in UTC.
- Total commits.
- Total issue events summarized in the report.
- Total pull request events summarized in the report.

### Commits
- Summarize notable commit activity by author or branch when activity exists.
- Include a short list of representative commits with short SHA, author, date, and subject line.
- If there were no commits, state clearly that no commit activity occurred in the previous seven days.

### Issues
- Summarize issues opened, closed, and other meaningful status changes in the window.
- Include a short list of the most relevant issues with links and one-line context.
- If there were no issue updates, state clearly that no issue activity occurred in the previous seven days.

### Pull Requests
- Summarize pull requests opened, merged, closed, and other meaningful status changes in the window.
- Include a short list of the most relevant pull requests with links and one-line context.
- If there were no pull request updates, state clearly that no pull request activity occurred in the previous seven days.

If the entire seven-day window has no commits, no issue activity, and no pull request activity, still create the issue and state clearly that no repository activity occurred in the previous seven days.

Do not invent classifications, importance, or missing details. Base the report only on repository data available through GitHub reads.

Use only the configured safe output for the visible write action. Do not post comments or attempt direct GitHub writes.