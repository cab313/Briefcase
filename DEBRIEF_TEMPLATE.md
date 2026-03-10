Briefcase — Hatch Self-Diagnosis Template
This is the canonical template. All Hatch agents fetch this file before generating their daily debrief.
To update the format, edit this file in the repo. Changes take effect on the next cron run.

Instructions
You are an AI agent surfacing bugs that users won't report themselves. Most users hit a problem, work around it, and move on — they never file a bug. Your job is to catch those moments and report them.

Rules:
- Review all conversations from today.
- Be brief. This is a targeted bug report, not a brain dump.
- Only surface bugs that meaningfully hurt the user experience. Skip cosmetic nitpicks, minor slowness, or one-off flukes that won't recur.
- Ask yourself: "Would fixing this noticeably improve Hatch for users?" If no, leave it out.
- For every bug you surface, check https://github.com/par-msl/hatch for open PRs, recent merges, or known issues that match. Search PR titles, descriptions, and changed files.
- The JSON block at the end is REQUIRED and must be valid JSON.
- Use the user's name from USER.md as the user_id.

Output Format
Use this exact structure:

# 🐛 Hatch Bug Report — YYYY-MM-DD

**User:** [name] | **Sessions:** [number] | **Duration:** [approx time]

---

## Bugs

The bugs that matter, stack ranked by severity. Each one should be something that, if fixed, would meaningfully improve the experience. Include whether a known issue or fixing PR already exists in https://github.com/par-msl/hatch.

| # | Bug | Severity | What Happened | Known Issue / PR |
|---|-----|----------|---------------|------------------|
| 1 | [short title] | P0/P1/P2/P3 | [1-2 sentences: what the user was doing, what went wrong] | [issue/PR link] or "None" |

Severity guide:
- **P0:** Product unusable
- **P1:** Major flow broken, no workaround
- **P2:** Broken but user can work around it
- **P3:** Annoying but minor

**Severity summary:** X/10 — one sentence justifying the rating (10 = disaster, 1 = clean).

If no meaningful bugs: "Clean session — no significant issues."

---

## PRs

Check https://github.com/par-msl/hatch for open and recently merged PRs relevant to the bugs above or to bug fixes in general. This gives a real-time view of what's being worked on.

| PR | Title | Status | Fixes Bug # |
|----|-------|--------|-------------|
| [PR link] | [title] | Open / Draft / In Review / Merged | [bug # from table above, or "other"] |

If no relevant PRs found: "No relevant PRs in flight."

---

## Root Cause Analysis

For the most impactful bugs above, a plain-English guess at why it's happening. Keep it short — one or two lines per bug.

| Bug # | What Broke | Likely Cause | Stack Layer |
|-------|-----------|--------------|-------------|
| [#] | [one-sentence summary] | [plain-English root cause] | frontend / backend / AI / infra / external |

---

## Patterns

Only include this section if you're seeing something recur across sessions — not one-off issues.
- Recurring errors or failures
- Flows that are consistently broken or degrading
- Regressions — things that used to work

If nothing recurring: omit this section entirely.

---

```json
{
  "user_id": "<name from USER.md>",
  "date": "YYYY-MM-DD",
  "session_count": <number>,
  "severity_rating": <1-10, 10 = worst>,
  "bugs": [
    {
      "description": "short title",
      "severity": "P0|P1|P2|P3",
      "what_happened": "1-2 sentence summary",
      "known_issue": "issue number or null",
      "fixing_pr": "PR number or null"
    }
  ],
  "prs": [
    {
      "pr_number": <number>,
      "title": "PR title",
      "status": "open|draft|in_review|merged",
      "fixes_bug": "bug description or null"
    }
  ],
  "root_causes": [
    {
      "bug": "which bug",
      "cause": "likely root cause",
      "stack_layer": "frontend|backend|ai|infra|external"
    }
  ],
  "patterns": [
    "recurring issues only — omit if none"
  ]
}
```
JSON rules:
- user_id: From USER.md, lowercase, no spaces.
- severity_rating: 1–10. Higher = worse session.
- bugs: Only meaningful bugs, sorted by severity. Empty array [] if clean session.
- prs: Relevant PRs from par-msl/hatch. Empty array [] if none found.
- root_causes: Only for impactful bugs. Empty array [] if clean session.
- patterns: Only recurring issues. Empty array [] if nothing recurring.
