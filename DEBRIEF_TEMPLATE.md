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
- For every bug you surface, check https://github.com/par-msl/hatch for open PRs or recent merges that might already fix it. Search PR titles, descriptions, and changed files.
- The JSON block at the end is REQUIRED and must be valid JSON.
- Use the user's name from USER.md as the user_id.

Output Format
Use this exact structure:

# 🐛 Hatch Bug Report — YYYY-MM-DD

**User:** [name] | **Sessions:** [number] | **Duration:** [approx time]

---

## Bugs

The bugs that matter. Each one should be something that, if fixed, would meaningfully improve the experience.

| Bug | Severity | Surface | What Happened | Fixing PR? |
|-----|----------|---------|---------------|------------|
| [short title] | P0/P1/P2/P3 | [area] | [1-2 sentences: what the user was doing, what went wrong] | [PR link] or "None" |

Severity guide:
- **P0:** Product unusable
- **P1:** Major flow broken, no workaround
- **P2:** Broken but user can work around it
- **P3:** Annoying but minor

If no meaningful bugs: "Clean session — no significant issues."

---

## Work In Flight

What's already being addressed in https://github.com/par-msl/hatch and what's not. This tells engineers where to focus.

**Being fixed (has PR):**

| Bug | PR | Status |
|-----|----|--------|
| [description] | [PR link] | Open / Draft / In Review / Merged |

**Not being fixed (no PR — engineers should look here):**

| Bug | Severity | Suggested Area to Investigate |
|-----|----------|-------------------------------|
| [description] | P0/P1/P2/P3 | [component, file path, or flow to look at] |

**Bottom line:** 1-2 sentences on what engineers should prioritize. Be direct.

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
      "surface": "affected area",
      "what_happened": "1-2 sentence summary",
      "blocked_user": true|false,
      "fixing_pr": "PR number or null"
    }
  ],
  "work_queue": {
    "has_pr": [
      {
        "bug": "short description",
        "pr_number": <number>,
        "pr_status": "open|draft|in_review|merged"
      }
    ],
    "needs_fix": [
      {
        "bug": "short description",
        "severity": "P0|P1|P2|P3",
        "suggested_area": "where to investigate"
      }
    ],
    "priority_recommendation": "what engineers should work on first"
  },
  "patterns": [
    "recurring issues only — omit if none"
  ]
}
```
JSON rules:
- user_id: From USER.md, lowercase, no spaces.
- severity_rating: 1–10. Higher = worse session.
- bugs: Only meaningful bugs. Empty array [] if clean session.
- work_queue.needs_fix: The most important part — unfixed bugs sorted by severity.
- patterns: Only recurring issues. Empty array [] if nothing recurring.
