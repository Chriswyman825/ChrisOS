# Morning Routine

<!-- ============================================================
     ABOUT THIS SKILL
     This skill runs your morning briefing and daily setup. It:
     - Checks whether last night's routine ran (prevents gaps)
     - Creates your daily note
     - Scans your calendar, email, and other sources
     - Proposes your top priorities
     - Runs a quick check-in

     TIME: ~5 minutes of active interaction
     DEPENDENCIES: Evening routine skill (for gap detection)
     MCP INTEGRATIONS: Calendar, Email (optional: Chat, Meeting notes)
     ============================================================ -->

---

## STEP 0: CHECK PREVIOUS EVENING ROUTINE

<!-- WHY THIS EXISTS: Without this check, you can silently miss an evening routine
     and lose a day's worth of meeting summaries and task updates. This step ensures
     continuity -- if yesterday's routine didn't run, it catches up first. -->

Before anything else, check whether the evening routine was run for the previous work day:

1. Determine the previous work day:
   - If today is Monday, previous work day is Friday
   - Otherwise, previous work day is yesterday

2. Read the daily note for the previous work day at `$VAULT_PATH/Daily Notes/YYYY-MM-DD.md`.
   <!-- CUSTOMIZE: Replace $VAULT_PATH with your actual vault path -->

3. Check whether it contains an `## Evening Reflection` section.

4. If the Evening Reflection section is MISSING:
   - Show: "Evening routine for [day] wasn't run -- taking care of that first."
   - Run /evening-routine for the previous work day
   - Once complete, proceed to STEP 1

5. If the Evening Reflection section EXISTS:
   - Proceed to STEP 1

<!-- CUSTOMIZE: If you run a weekly review skill (like self-review), you can add a
     check here for whether the weekly review ran. Example:
     "If previous work day was Thursday, check Logs/reviews/ for a review file
     dated this week. If missing, offer to run /self-review first." -->

---

## STEP 0.5: GAP DETECTION

<!-- WHY THIS EXISTS: Life happens. You miss a day or two. Without this check,
     those days become black holes in your records. This step offers a lightweight
     backfill so you don't lose context. -->

Check for missing daily notes. Look at the last 5 weekdays. If 2+ consecutive days have no daily note, show:

"I see [N] days with no daily note since [date]. Want to do a quick bullet summary for each so we don't lose context?"

If yes, create minimal backfill notes (date, 2-3 bullet points from the user). If no, proceed.

---

## STEP 1: CREATE DAILY NOTE

Create today's daily note at `$VAULT_PATH/Daily Notes/YYYY-MM-DD.md`.
<!-- CUSTOMIZE: Replace $VAULT_PATH with your actual vault path -->

Use your preferred daily note template. A minimal version:

```markdown
# [Day of Week], [Month Day, Year]

## Check-in
- Energy:
- Top priorities:
  1.
  2.
  3.
- Success criteria:

## Notes

## Evening Reflection
<!-- Filled in by evening routine -->
```

---

## STEP 2: SCAN FOR OUTSTANDING ITEMS

<!-- WHY THIS EXISTS: The morning briefing pulls from all your sources so you start
     the day with full situational awareness rather than discovering things mid-afternoon.
     The key design choice: split output into "FYI" (informational) and "Needs input"
     (blocking). This prevents informational items from feeling like questions. -->

Scan your connected sources in parallel:

<!-- CUSTOMIZE: Replace these tool calls with your actual MCP integrations.
     The specific tools depend on what you have connected. Common options:

     CALENDAR (pick one):
     - Google Calendar: mcp__gws__calendar_events_list (calendarId=primary, today + tomorrow)
     - Outlook: your Outlook MCP equivalent

     EMAIL (pick one):
     - Gmail: mcp__gws__gmail_users_threads_list (q=is:unread in:inbox newer_than:2d)
     - Your email MCP equivalent

     CHAT (optional):
     - Slack: mcp__claude_ai_Slack__slack_search_public_and_private
     - Your chat tool MCP equivalent

     MEETING NOTES (optional):
     - Granola: mcp__claude_ai_Granola__list_meetings
     - Or check your Meetings/raw/ directory for recent files
-->

- **Calendar:** Pull today's and tomorrow's events. Flag: meetings starting within 2 hours, back-to-back blocks (less than 15 min gap), conflicts.
- **Email:** Pull unread threads from the last 2 days. Prioritize threads from key people (your manager, direct reports, important stakeholders). Skip newsletters and automated emails.
- **Chat:** Scan for DMs, mentions, and direct questions.
- **Meeting notes:** Check for unresolved action items from yesterday's meetings.

While scans run, immediately ask the first check-in question (Step 3, Question 1) -- it does not depend on the briefing results.

Once all scans return, compile and present the Morning Briefing in two sections:

1. **Briefing** (informational, no response needed) -- calendar overview, FYI items, status updates
2. **Needs your input** (blocking) -- items requiring a decision or response

Keep to 5-8 bullet points total. Then proceed to Step 3 Question 2.

---

## STEP 3: CHECK-IN

<!-- WHY THIS EXISTS: The check-in grounds the day. The energy question helps Claude
     calibrate tone and ambition. The priorities question uses propose-first (Claude
     suggests based on evidence, you confirm) rather than open-ended asking. The
     success criteria question creates a clear "done" signal for the evening. -->

Ask the following questions one at a time. Wait for a response before moving to the next:

1. **How are you feeling this morning?** (energy level, mood, anything on your mind)

2. **Top 3 priorities -- propose first, then confirm.**
   - Read `$VAULT_PATH/Tasks/active.md` (HIGH items + anything time-sensitive)
   - Read the previous day's "Carry into tomorrow" from the daily note
   - Propose 3 priorities based on urgency, deadlines, and carry-forwards
   - Present as: "Here's what I'd suggest for today -- confirm or swap anything out:"
   - Wait for confirmation or adjustments before writing to the note

3. **What's one thing that would make today feel successful?**

---

## STEP 4: UPDATE DAILY NOTE

After the check-in, update the daily note with:
- Energy/mood check-in
- Top 3 priorities as a checklist
- Success criteria
- A "Notes" section for capturing thoughts throughout the day

---

## STEP 5: SYNC TASK LIST

Check `$VAULT_PATH/Tasks/active.md` and ensure any new action items (from the briefing, carry-overs, or check-in) are reflected there.

<!-- CUSTOMIZE: If you use a specific priority format (e.g., [HIGH], [MEDIUM], [LOW]),
     make sure new items are placed in the right section. Match whatever format you
     define in your active.md template. -->

Keep the format clean and scannable.

---

<!-- CUSTOMIZE: You can add additional steps here for your specific needs. Examples:
     - A market/industry news scan
     - Checking a specific dashboard or project management tool
     - Running a sub-skill (like a weekly prep on certain days)
     - A habit tracker check-in

     The morning routine is the most natural place to layer on "things I want to
     know at the start of every day." Start minimal and add steps as needs emerge.
     The self-review skill will often propose additions here based on your patterns. -->
