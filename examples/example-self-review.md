# Example Self-Review Session

This shows what a weekly self-review looks like -- Claude analyzing the week's interaction logs and proposing improvements.

---

**Claude:** Running Thursday self-review -- scanning this week's logs...

Found 4 of 4 expected logs (Mon-Thu). Reading previous review from April 17 for continuity. Loading rejected.json (3 items) and acted-on-log.json (5 items).

3 proposals this week.

---

## Proposal 1 of 3

**What I noticed:**
You corrected the meeting summary format twice this week (Tuesday and Thursday). Both times you asked for action items to be grouped by person rather than by priority level.

**What I'm proposing:**
Update the meeting-summary skill to default to grouping action items by owner, with priority as a secondary sort within each person's section.

**Why it's worth it:**
Removes a recurring manual reformatting step -- you clearly think about follow-ups by person, not by priority.

**Type:** Skill improvement

Build / Save for Later / Reject?

**You:** Build. That's a good catch.

**Claude:** Added to active.md: `[MEDIUM] Build: meeting-summary format change -- group action items by owner, priority as secondary sort`. Logged to acted-on-log.

---

## Proposal 2 of 3

**What I noticed:**
Three times this week (Monday, Wednesday, Thursday) you asked me to look up a person's background before a meeting -- their role, last interaction, any open items with them. Each time it was an ad hoc request with the same pattern.

**What I'm proposing:**
Create a "meeting prep" skill that, given a meeting name or attendee list, pulls together: who they are, last time you met, any open action items involving them, and relevant recent context.

**Why it's worth it:**
You are already doing this manually before important meetings. A skill would make it one command and ensure nothing is missed.

**Type:** New skill

Build / Save for Later / Reject?

**You:** Save for later. I like the idea but I want to finish tuning the core loop first.

**Claude:** Added to active.md: `[LOW] Consider: meeting-prep skill -- auto-pull attendee background, last interaction, open items`. Logged to acted-on-log.

---

## Proposal 3 of 3

**What I noticed:**
On Wednesday, the calendar MCP timed out twice during the morning routine [FRICTION]. You had to wait about 30 seconds each time before it recovered.

**What I'm proposing:**
Add a timeout fallback to the morning routine -- if the calendar call fails after 10 seconds, proceed with the rest of the briefing and retry the calendar pull at the end.

**Why it's worth it:**
Prevents a single tool failure from blocking the entire morning routine.

**Type:** Skill improvement

Build / Save for Later / Reject?

**You:** Build. That's annoyed me before.

**Claude:** Added to active.md: `[MEDIUM] Build: morning-routine calendar timeout fallback -- proceed on failure, retry at end`. Logged to acted-on-log.

---

## Self-Review Complete -- April 24, 2026

**This week's patterns:**
- Meeting summary formatting corrections were the strongest signal (2 corrections, same issue)
- Ad hoc meeting prep requests are becoming a pattern (3 this week)

**Decisions made:**
- Build: 2 -- added to MEDIUM tasks
- Save for Later: 1 -- added to LOW tasks
- Reject: 0

**Running total (all time):**
- Built: 7
- Rejected: 3

Review saved to Logs/reviews/2026-04-24_Self-Review.md.
