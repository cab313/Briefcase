Briefcase — Daily Session Debrief Template
This is the canonical template. All Hatch agents fetch this file before generating their daily debrief.
To update the debrief format for everyone, edit this file in the repo. Changes take effect on the next cron run.

Instructions
You are generating a daily self-diagnosis of your own performance. This is not a summary for the user — it is a quality report about how well YOU performed as an AI assistant today.
Rules:
Review ALL conversations from today. Do not skip any.
Be honest. Be specific. Do not sugarcoat.
Reference actual events, tool calls, errors, and decisions — not generalities.
The JSON block at the end is REQUIRED. It must be valid JSON. It is parsed for cross-user synthesis.
Use the user's name from USER.md as the user_id in the JSON block.

Output Format
Use this exact structure:
# 📅 Session Debrief — YYYY-MM-DD

**Session Overview**
- **Interactions:** [number] conversation exchanges across [number] session(s)
- **Duration:** approximate active time window
- **Topics:** brief list of what was covered

---

## 🟢 What Went Right
- Tasks completed successfully and why they worked well
- Good responses — moments where the experience felt seamless
- Smooth tool usage, accurate information, helpful recommendations
- Times you adapted well to the user's needs or communication style

---

## 🔴 What Went Wrong
- Bugs, errors, tool failures (be specific: what tool, what happened)
- Bad or incomplete answers
- Things that took too long or required too many steps
- Confusing, unhelpful, or redundant responses
- Times you misunderstood what the user wanted

---

## 🐛 Bugs & Issues

List each issue as a row. Every bug or technical problem gets its own entry.

| Description | Severity | Tool/Skill |
|-------------|----------|------------|
| [what happened] | low / med / high | [which tool or skill was affected] |

If no bugs were encountered, write: "No bugs or technical issues encountered today."

---

## 🧪 Unique Use Cases
- Anything unusual, creative, or novel that was explored
- Edge cases or unexpected requests
- New workflows the user tried
- Requests that pushed beyond typical usage patterns

If nothing unusual, write: "Standard usage patterns today — no notable edge cases."

---

## 🧠 Decision Reasoning
- Key decisions YOU made during the session and WHY
- Tradeoffs you considered (e.g. "gave a short answer because user prefers brevity")
- Alternative approaches you could have taken
- Moments where you chose one tool/method over another and why

---

## 📈 Areas for Improvement
- Patterns you noticed that could be better
- Feature gaps or missing capabilities that affected the session
- UX friction points — where the experience felt clunky
- Things you'd do differently next time

---

## ⭐ Overall Session Rating: X/10

One to two sentences justifying the rating. Be calibrated:
- **9-10:** Exceptional — everything worked, user was clearly delighted
- **7-8:** Good — solid session with minor issues
- **5-6:** Mixed — some things worked, some didn't
- **3-4:** Poor — significant issues, user likely frustrated
- **1-2:** Failed — major failures, couldn't complete basic tasks

JSON Block (REQUIRED)
Every debrief must end with this JSON block wrapped in a code fence. This is parsed programmatically for cross-user synthesis. Do not omit it. Do not alter the field names.
{
  "user_id": "<name from USER.md>",
  "date": "YYYY-MM-DD",
  "session_count": <number of conversation exchanges>,
  "overall_rating": <1-10>,
  "went_right": [
    "concise description of each positive item"
  ],
  "went_wrong": [
    "concise description of each negative item"
  ],
  "bugs": [
    {
      "description": "what happened",
      "severity": "low|med|high",
      "tool": "affected tool or skill name"
    }
  ],
  "unique_use_cases": [
    "concise description of each novel usage"
  ],
  "improvement_areas": [
    "concise description of each improvement"
  ]
}
JSON rules:
user_id: Read from USER.md. Use the "Name" or "What to call them" field. Lowercase, no spaces (e.g. christian, sarah).
date: Today's date in YYYY-MM-DD format.
session_count: Total number of back-and-forth exchanges today.
overall_rating: Integer 1–10. Match the rating in the markdown section above.
All arrays: Use short, specific strings. One item per bullet point from the corresponding markdown section.
bugs: If no bugs, use an empty array [].
