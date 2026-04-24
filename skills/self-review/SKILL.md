# Self-Review

<!-- ============================================================
     ABOUT THIS SKILL
     This is the feedback loop that makes the whole system improve over time.
     It:
     - Reads the week's interaction logs
     - Detects patterns: corrections, friction, repeated manual work
     - Proposes concrete improvements (one at a time, interactively)
     - Learns from rejection (never re-surfaces rejected ideas)
     - Tracks what has been built and what has been passed on

     Without this skill, your system stays static. With it, the system
     progressively molds to your preferences and workflow.

     TIME: ~5 minutes of active interaction
     CADENCE: Weekly (pick a consistent day)
     DEPENDENCIES: Interaction logs in Logs/daily/ (written by evening routine)
     ============================================================ -->

**Cadence:** Weekly (recommended: Thursday evenings, called from evening-routine)

---

## Purpose

**What this skill is FOR:**
- Reading all daily interaction logs from the past week
- Detecting patterns: repeated requests, recurring corrections, manual steps that could be automated, underused skills
- Proposing concrete improvements interactively (not dumping a wall of ideas)
- Learning from rejection -- never re-surfacing what you have already said no to
- Feeding back into your system over time, making it progressively more useful

**What this skill is NOT for:**
- Building the actual skills (it proposes, you decide, then you build)
- Reviewing business content or meeting notes
- Making decisions for you -- proposes only, you decide
- Running mid-week (unless explicitly invoked)

---

## Dependencies

**Required local files:**
- `$VAULT_PATH/Logs/daily/` -- this week's interaction logs (YYYY-MM-DD.md files)
- `config/rejected.json` -- ideas already rejected (never surface again)
- `config/acted-on-log.json` -- ideas already acted on (don't re-propose)

<!-- CUSTOMIZE: Replace $VAULT_PATH with your actual vault path.
     The config/ directory lives alongside this SKILL.md file, at:
     ~/.claude/skills/self-review/config/
     The first time you run this, these files can be empty arrays: [] -->

**Optional files:**
- `$VAULT_PATH/Logs/reviews/` -- prior review files (enables continuity across weeks)
- `references/pattern-library.md` -- common patterns to look for (see references/ directory)

---

## Process

### STEP 1: LOAD CONTEXT

Read all daily log files in `$VAULT_PATH/Logs/daily/` for the current week (Monday through today).

Read the most recent file in `$VAULT_PATH/Logs/reviews/` (for continuity with prior weeks).

Load `config/rejected.json` and `config/acted-on-log.json`.

**If no logs exist for this week:**
- Show: "No daily logs found for this week. Logs are written by the evening routine skill. Want to skip self-review?"
- Do not fabricate patterns from memory
- Wait for confirmation

**If partial logs (some days missing):**
- Proceed with what is there
- Note: "[N] of [expected] logs found -- [missing days] missing. Patterns based on partial week."

---

### STEP 2: DETECT PATTERNS

Scan the logs for these signals:

<!-- CUSTOMIZE: Adjust these pattern types based on what matters to you.
     The [CORRECTION] and [FRICTION] tags come from your interaction logs
     (written by the evening routine). If you use different tags, update
     the list below. You can also add your own pattern types. -->

1. **Corrections and rephrasing** -- tagged `[CORRECTION]` in logs -- these are highest signal. If you corrected the same thing multiple times, that is a clear improvement opportunity.

2. **Repeated ad hoc requests** -- same type of request appearing 3+ times this week. A task you do manually three times should probably be a skill.

3. **Friction points** -- tagged `[FRICTION]` in logs -- things that took multiple attempts. Tool failures, confusing outputs, workflows that are harder than they should be.

4. **Skills not used** -- which installed skills went unused this week. May indicate a skill that is not useful, or a workflow that changed.

5. **Manual steps that recurred** -- things done by hand that look automatable.

6. **Underspecified workflows** -- skills that were invoked but produced low-quality output requiring significant correction.

**Do NOT surface:**
- Single-occurrence items (unless tagged `[FRICTION]` with explicit note)
- Items already in `config/rejected.json`
- Items already in `config/acted-on-log.json`

---

### STEP 3: SYNTHESIZE PROPOSALS

1. Group detected patterns into distinct improvement ideas (deduplicate)
2. Classify each as:
   - **New skill** -- a repeating workflow that does not have a skill yet
   - **Skill improvement** -- an existing skill that produced friction or corrections
   - **Tweak** -- a small change to a skill file or MEMORY.md (not a full skill)
   - **Observation** -- worth noting but no clear action yet
3. Rank by potential value: high-signal corrections > repeated requests > single friction points
4. Cap at **5 proposals max** per review -- quality over quantity

---

### STEP 4: PRESENT INTERACTIVELY

<!-- WHY THIS EXISTS: Presenting one proposal at a time forces a decision on each.
     Dumping a list of 5 ideas leads to "I'll think about it" and nothing changes.
     One at a time with an immediate Build/Save/Reject decision ensures progress. -->

Present one proposal at a time:

```
## Proposal [N] of [Total]

**What I noticed:**
[1-2 sentences: what pattern appeared and where in the logs]

**What I'm proposing:**
[1-2 sentences: specific improvement or new skill idea]

**Why it's worth it:**
[1 sentence: what friction this removes]

**Type:** New skill / Skill improvement / Tweak / Observation

Build / Save for Later / Reject?
```

**Decision handling:**

| Decision | What happens |
|----------|-------------|
| **Build** | Add `[MEDIUM] Build: [proposal name] -- [1-line description]` to `$VAULT_PATH/Tasks/active.md`. Log to `config/acted-on-log.json`. |
| **Save for Later** | Add `[LOW] Consider: [proposal name] -- [1-line description]` to `$VAULT_PATH/Tasks/active.md`. Log to `config/acted-on-log.json`. |
| **Reject** | Log to `config/rejected.json` with date and brief reason. Ask: "Anything I should know about why, or just not interested?" Never surface again. |

After each decision, show brief confirmation, then move to next proposal.

---

### STEP 5: POST-REVIEW SUMMARY

After all proposals are handled:

```
## Self-Review Complete -- [Date]

**This week's patterns:**
- [1-line summary of what dominated the logs]

**Decisions made:**
- Build: [count] -- added to MEDIUM tasks
- Save for Later: [count] -- added to LOW tasks
- Reject: [count] -- logged, won't resurface

**Running total (all time):**
- Built: [count from acted-on-log]
- Rejected: [count from rejected.json]
```

---

### STEP 6: WRITE REVIEW TO VAULT

Write a structured review to `$VAULT_PATH/Logs/reviews/YYYY-MM-DD_Self-Review.md`.

Include:
- Patterns detected
- Proposals presented with decisions
- Any carry-forward observations (patterns not strong enough to propose yet, but worth watching)

This file is read by next week's self-review for continuity.

---

## Guardrails

### NEVER
- Never re-surface a rejected idea -- check `config/rejected.json` before every proposal
- Never present more than 5 proposals in one session
- Never add tasks without an explicit Build or Save for Later decision
- Never fabricate patterns -- only surface what appears in the actual logs
- Never skip loading rejected.json

### ALWAYS
- Always present proposals one at a time
- Always ask for rejection reason (optional)
- Always log decisions immediately after each one (do not batch at end)
- Always note when logs are missing
- Always include count of prior rejected/acted-on items in summary

### FAILURE MODES

**All patterns already rejected or acted on:**
- Show: "Everything I noticed this week was already handled. Nothing new to propose."
- Do not pad with weak observations to fill the slot

**Conflict with acted-on-log:**
- If a proposal matches something already in acted-on-log but not yet completed, note it: "You've already tasked this -- [task description from date]. Still in progress or want to revisit?"

---

## How Rejection Memory Works

`config/rejected.json` accumulates over time. Before each proposal is generated, Claude checks whether the idea (or a close variant) has been rejected before. If yes, it is skipped silently.

The rejection log is never pruned automatically. You can review it periodically and remove entries manually if a previously-rejected idea becomes relevant again.

## How the Acted-On Log Works

`config/acted-on-log.json` tracks everything that has been tasked (Build or Save for Later). It prevents re-proposing things already in the pipeline. If an acted-on item is later completed and removed from tasks, it stays in the log -- the idea was addressed.

## Continuity Across Weeks

The most recent review in `$VAULT_PATH/Logs/reviews/` provides carry-forward context. Observations flagged as "not ready to act on yet" may resurface if the pattern persists the following week.
