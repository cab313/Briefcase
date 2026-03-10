Briefcase — Hatch Self-Diagnosis Template
This is the canonical template. All Hatch agents fetch this file before generating their daily debrief.
To update the debrief format for everyone, edit this file in the repo. Changes take effect on the next cron run.

Instructions
You are generating a daily self-diagnosis focused on bugs, failures, and things that went wrong during your session. This is NOT a performance summary — it is a bug report about what broke, what's still broken, and what needs fixing in Hatch.
Rules:
Review ALL conversations from today. Do not skip any.
Only report problems. Do not include things that went well — that's not what this report is for.
Be honest. Be specific. Reference actual errors, tool calls, failures, and broken flows.
Check the Hatch GitHub repo (par-msl/hatch) for open issues and recent PRs to cross-reference your findings against known bugs.
For EVERY bug surfaced in this report, search par-msl/hatch for open or recently merged PRs that might fix it. Search PR titles, descriptions, and changed files. If a PR exists, link it. If no PR exists, flag the bug as needing engineering work.
The JSON block at the end is REQUIRED. It must be valid JSON. It is parsed for cross-user synthesis.
Use the user's name from USER.md as the user_id in the JSON block.

Output Format
Use this exact structure:
# 🐛 Hatch Bug Report — YYYY-MM-DD

**Session Overview**
- **Interactions:** [number] conversation exchanges across [number] session(s)
- **Duration:** approximate active time window
- **Topics:** brief list of what was covered

---

## 🔴 What Broke

Everything that went wrong during this session. Be specific — what happened, what the user was trying to do, and what the actual vs. expected behavior was.

For each issue:
- **What the user was doing** — the action or flow
- **What went wrong** — the actual failure (error message, wrong output, hang, etc.)
- **Impact** — did it block the user? Did they have to retry? Did they give up?

---

## 🐛 Bugs Encountered

List every bug or technical problem as its own row. Be ruthless — if something didn't work right, it goes here.

| Description | Severity | Surface/Tool | Repro Steps | Blocked User? |
|-------------|----------|--------------|-------------|---------------|
| [what happened] | P0 / P1 / P2 / P3 | [which surface, tool, or skill] | [brief steps to reproduce] | Yes / No |

Severity guide:
- **P0:** Complete failure — user cannot use the product
- **P1:** Major functionality broken — user has no workaround
- **P2:** Broken but has a workaround — annoying but not blocking
- **P3:** Minor issue — cosmetic, slow, or slightly wrong

If no bugs were encountered, write: "No bugs encountered today." (But look harder — there's almost always something.)

---

## 🔁 Known Issue & PR Cross-Reference

For EVERY bug surfaced in this report, search the Hatch repo (https://github.com/par-msl/hatch) for:
1. **Open issues** that match the bug
2. **Open PRs** that might fix it (search titles, descriptions, and changed file paths)
3. **Recently merged PRs** (last 14 days) that may have already addressed it

| Today's Bug | Known Issue? | Fixing PR? | PR Status | GitHub Links |
|-------------|-------------|------------|-----------|--------------|
| [brief description] | Yes / No | Yes / No | Open / Merged / Draft / None | [issue link] [PR link] or "No coverage" |

Also check the rolling Hatch Feedback Digest (Issue #60) for any matching entries.

**How to search for fixing PRs:**
- Search PR titles and bodies for keywords from the bug (e.g. error messages, component names, feature area)
- Check recently changed files — if the bug is in a specific component, look for PRs touching that file path
- Check PR labels (e.g. `bug`, `fix`, `hotfix`)
- If a PR description references a Sentry issue ID that matches, that counts as a match

---

## 🔧 Engineering Work Queue

This is the actionable output. Bugs surfaced in this report that have NO open PR addressing them — meaning no one is working on a fix yet. Sorted by severity.

**Unfixed bugs — needs engineering attention:**

| # | Bug | Severity | Surface | Repro Steps | Has Issue? | Suggested Fix Area |
|---|-----|----------|---------|-------------|------------|--------------------|
| 1 | [description] | P0/P1/P2/P3 | [where] | [steps] | Yes (link) / No — file one | [file path or component to investigate] |

**Already covered by PRs — just track:**

| Bug | PR | PR Status | ETA |
|-----|----|-----------|-----|
| [description] | [PR link] | Open / Draft / In Review | [if knowable] |

**Recommendation:**
Write 1-2 sentences summarizing what engineers should prioritize. Example: "Two P1 bugs have no PR coverage — the message rendering crash and the auth token expiry. Recommend starting there."

---

## 📊 Open Bug Status Check

Pull the current state of open bugs from the Hatch repo. This gives a real-time snapshot of what still needs work.

**Open GitHub Issues (bug label):**
List all open issues tagged `bug` with their current status.

| # | Title | Opened | Last Activity | Assignee | Stale? |
|---|-------|--------|---------------|----------|--------|
| [number] | [title] | [date] | [date] | [who or "unassigned"] | Yes if >7 days |

**Recent Bug-Fix PRs (last 7 days):**
List any merged or open PRs that address bugs.

| PR # | Title | Status | Bug Fixed |
|------|-------|--------|-----------|
| [number] | [title] | Open / Merged | [brief description] |

**Feedback Digest (Issue #60) Status:**
Summarize the current entries in the rolling feedback digest — how many are active, monitoring, resolved, or stale.

| Status | Count |
|--------|-------|
| 🔴 Active | [n] |
| 🟡 Monitoring | [n] |
| 🟢 Resolved | [n] |
| ⚪ Stale | [n] |

---

## 🧠 Root Cause Analysis

For the most impactful bugs from today, dig into the likely root cause. Plain English — no jargon.

For each significant bug:
- **What broke:** one-sentence summary
- **Why it broke:** your best guess at the root cause
- **Where in the stack:** frontend / backend / AI layer / infra / external dependency
- **Suggested fix or investigation path:** what should an engineer look at

---

## 📈 Pattern Watch

Things that aren't single bugs but recurring friction or degradation patterns:
- Errors that keep showing up across sessions
- Flows that are consistently slow or flaky
- User workarounds that suggest a deeper problem
- Regressions — things that used to work and don't anymore

---

## ⭐ Severity Summary: X/10

Rate how buggy the session was (10 = disaster, 1 = clean session):
- **9-10:** Critical failures — product was unusable
- **7-8:** Major issues — core flows broken
- **5-6:** Moderate — several bugs but product was usable
- **3-4:** Minor — small issues, mostly smooth
- **1-2:** Clean — barely anything went wrong

One to two sentences justifying the rating.

---

JSON Block (REQUIRED)
Every debrief must end with this JSON block wrapped in a code fence. This is parsed programmatically for cross-user synthesis. Do not omit it. Do not alter the field names.
```json
{
  "user_id": "<name from USER.md>",
  "date": "YYYY-MM-DD",
  "session_count": <number of conversation exchanges>,
  "severity_rating": <1-10, where 10 is worst>,
  "bugs": [
    {
      "description": "what happened",
      "severity": "P0|P1|P2|P3",
      "surface": "affected surface, tool, or skill",
      "repro_steps": "brief steps to reproduce",
      "blocked_user": true|false,
      "known_issue": true|false,
      "github_ref": "issue or PR number, or null"
    }
  ],
  "what_broke": [
    "concise description of each failure"
  ],
  "root_causes": [
    {
      "bug": "which bug",
      "cause": "likely root cause",
      "stack_layer": "frontend|backend|ai|infra|external"
    }
  ],
  "patterns": [
    "recurring issues or degradation trends"
  ],
  "open_bugs_snapshot": {
    "total_open": <number>,
    "stale_count": <number of issues with no activity >7 days>,
    "active_prs": <number of open bug-fix PRs>
  },
  "work_queue": {
    "unfixed_bugs": [
      {
        "description": "bug with no PR coverage",
        "severity": "P0|P1|P2|P3",
        "surface": "affected area",
        "suggested_fix_area": "file path or component to investigate",
        "has_issue": true|false,
        "issue_ref": "issue number or null"
      }
    ],
    "covered_by_pr": [
      {
        "description": "bug that has a PR in progress",
        "pr_number": <number>,
        "pr_status": "open|draft|in_review|merged"
      }
    ],
    "priority_recommendation": "1-2 sentence summary of what engineers should work on first"
  }
}
```
JSON rules:
- user_id: Read from USER.md. Use the "Name" or "What to call them" field. Lowercase, no spaces.
- date: Today's date in YYYY-MM-DD format.
- session_count: Total number of back-and-forth exchanges today.
- severity_rating: Integer 1–10. Higher = more bugs/worse session. Match the rating in the markdown section.
- bugs: Every bug gets its own entry. If no bugs, use an empty array [].
- open_bugs_snapshot: Pull from the GitHub repo at debrief time. Gives a point-in-time count.
