# Pattern Library

These are the signal types the self-review skill scans for in interaction logs. Each pattern type has a description and examples of what to look for.

---

## 1. Corrections and Rephrasing [CORRECTION]

**What it means:** You corrected Claude's output -- changed the format, rewrote a sentence, asked for a different structure. This is the highest-signal pattern because it reveals a gap between what Claude produced and what you actually wanted.

**Look for:**
- Same correction appearing 2+ times in a week
- Corrections to the same skill's output
- Formatting corrections (these are easy wins to fix in skill configs)

**Example log entry:**
```
[CORRECTION] Asked for meeting summary to lead with decisions, not attendees
[CORRECTION] Rewrote task description -- too verbose, wanted one-liners
```

---

## 2. Repeated Ad Hoc Requests

**What it means:** You asked for the same type of thing multiple times without using a skill. If you are doing something 3+ times a week, it probably deserves a skill or at least a shortcut.

**Look for:**
- Same request type appearing 3+ times
- Requests that follow a predictable pattern (same inputs, same output format)
- Requests tied to a recurring event (e.g., "every Monday I ask Claude to...")

---

## 3. Friction Points [FRICTION]

**What it means:** Something did not work smoothly. A tool timed out, an output was wrong and needed multiple attempts, a workflow was confusing.

**Look for:**
- Tool/MCP failures or timeouts
- Multi-attempt workflows (tried 2-3 times to get the right result)
- Confusion about which skill to use or how to invoke it

---

## 4. Unused Skills

**What it means:** A skill exists but was not invoked this week. Could mean the skill is not useful, or the workflow it supports has changed, or you forgot it exists.

**Look for:**
- Skills that have not been used in 2+ consecutive weeks
- Skills that were replaced by ad hoc requests (you are doing it manually instead)

---

## 5. Manual Steps That Recurred

**What it means:** You did something by hand that Claude could automate. Copy-pasting between tools, reformatting data, looking up the same reference repeatedly.

**Look for:**
- Steps within a skill that you always modify manually after
- Information lookups that happen before every meeting or every morning
- Data entry that follows a consistent pattern

---

## 6. Underspecified Workflows

**What it means:** A skill ran but produced output that needed significant editing. The skill is partially right but not tuned enough.

**Look for:**
- Skills where you consistently edit 30%+ of the output
- Skills where you skip certain sections every time
- Output formats that don't match how you actually use the information
