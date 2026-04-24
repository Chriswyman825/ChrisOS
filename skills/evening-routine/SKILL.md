# Evening Routine

<!-- ============================================================
     ABOUT THIS SKILL
     This skill wraps up your day. It:
     - Triggers meeting summary processing
     - Prompts evening reflection
     - Updates and reconciles your task list
     - Moves completed tasks to your done log
     - Writes an interaction log (used by the self-review skill)
     - Optionally triggers the weekly self-review

     TIME: ~10 minutes of active interaction
     DEPENDENCIES: Meeting summary skill, self-review skill (weekly)
     MCP INTEGRATIONS: Calendar, Email (optional: Chat)

     This is the most complex skill. Start with Steps 1-5 and add
     the rest as you get comfortable.
     ============================================================ -->

<!-- CATCHUP MODE: If this routine is being run retroactively (the daily note
     date is not today), run in lightweight mode: do meeting summaries + task
     hygiene (Steps 1, 4, 4.5, 5) only. Skip reflection questions (Step 2),
     email checks, and detailed interaction logs. This handles the common case
     of "I missed last night, catching up this morning." -->

Read today's daily note from `$VAULT_PATH/Daily Notes/`. Work through these steps one at a time.
<!-- CUSTOMIZE: Replace $VAULT_PATH with your actual vault path -->

---

## STEP 1: SUMMARIZE TODAY'S MEETINGS

<!-- WHY THIS EXISTS: Meeting processing is the longest-running step, so we kick it
     off first as a background task. By the time you finish the reflection questions,
     it is usually done. -->

Launch /meeting-summary as a background agent (do not wait for it to complete). Immediately proceed to STEP 2 while the agent runs in parallel.

The agent should return action items as a list rather than writing directly to active.md -- the main thread will handle the active.md write in STEP 4 to avoid race conditions.

<!-- CUSTOMIZE: If you don't have meeting notes connected yet, skip this step
     and manually process meetings during Step 4. You can always add this automation
     later when you connect a meeting notes tool. -->

---

## STEP 2: REFLECTION

<!-- WHY THIS EXISTS: This is the part that turns a task management system into
     something that actually helps you grow. The propose-first approach for "what got
     done" means you don't have to recall everything from memory -- Claude does the
     detective work and you confirm or correct. The learning and gratitude questions
     are open-ended by design. -->

Work through these questions one at a time, waiting for a response before moving on:

1. **What got done today? What didn't?**
   PROPOSE-FIRST: Use today's daily note, meeting summaries, and calendar to draft a "here's what I think got done / didn't" summary. Present it for confirmation. When proposing, distinguish between:
   - "Confirmed done" -- evidence in meeting notes, daily note, or chat
   - "Likely done but unconfirmed" -- on the calendar but no evidence of outcome
   - "Status unknown" -- no evidence found (does NOT mean it didn't happen -- ask)

   Never state something didn't get done unless there is positive evidence it was skipped or cancelled.

2. **What's one thing you learned or noticed about yourself today?**
   Ask directly. This is personal reflection, not factual recall.

3. **What are you grateful for today?**
   Ask directly.

<!-- CUSTOMIZE: Questions 2 and 3 can be daily or weekly. Running them daily builds
     the habit but takes more time. A common pattern is to run all three questions
     on one day per week (e.g., Thursdays) and only question 1 on other days.

     You can also swap these questions entirely. Some people prefer:
     - "What drained your energy today?"
     - "What would you do differently?"
     - "What are you looking forward to tomorrow?"

     The key is consistency: pick questions that surface useful signal and stick
     with them long enough for patterns to emerge in your logs. -->

---

## STEP 3: UPDATE DAILY NOTE

After the reflection, update today's daily note by adding an "Evening Reflection" section with:
- What got done / didn't
- Learning or observation (if asked)
- Gratitude note (if asked)
- Carry-over items for tomorrow

Before finalizing carry-overs, pull tomorrow's schedule:
<!-- CUSTOMIZE: Replace with your calendar MCP tool call.
     Example for Google Calendar:
     mcp__gws__calendar_events_list with calendarId=primary,
     timeMin=[tomorrow 00:00], timeMax=[tomorrow 23:59],
     singleEvents=true, orderBy=startTime -->

If any meetings need prep (first meeting with someone, customer call, key review), add a prep task to the carry-over list automatically.

---

## STEP 4: UPDATE TASK LIST

<!-- WHY THIS EXISTS: Task hygiene is the most important mechanical step. Without it,
     your active.md drifts from reality within days. The evening routine is the
     forcing function that keeps it accurate. -->

Using `$VAULT_PATH/Tasks/active.md`:

1. Wait for the meeting summary agent (Step 1) to complete if it hasn't already. Collect its returned action item list.
2. Present new action items: "Here's what your meetings produced today -- [N] new items. Adding to active.md now." Write new items (dedup against existing items first).
3. Check off what got done from the morning priorities.
4. If any tasks didn't get done, ask if they should carry to tomorrow's priorities.

<!-- CUSTOMIZE: If you have email connected, add an email check here:
     Pull unread threads from the last day. Surface threads where you are the last
     recipient and haven't replied. Present as: "You have [N] unresponded email
     threads -- want to add any as tasks?" Only flag threads from real people
     (skip notifications and automated emails). -->

---

## STEP 4.5: TASK RECONCILIATION

<!-- WHY THIS EXISTS: Active.md accumulates stale tasks. This reconciliation step
     catches items that were completed but never checked off -- things that got done
     in meetings, via Slack, or through other channels without you explicitly marking
     them. Without this, your task list slowly inflates with phantom items. -->

Before moving to Step 5, run a reconciliation pass across ALL items in active.md (not just today's priorities):

1. Cross-reference every open task against available context:
   - Today's meeting summaries and daily note
   - Recent meeting summaries (last 3-5 days)
   - Conversation context from the current session
   - Known project states
   - done.md (catch items already logged but not removed from active)

2. Flag any tasks that appear completed based on available evidence.

3. Present as: "I think these are also done based on what I've seen -- confirm or correct:"
   - For each item, cite the evidence

4. Only mark items complete after confirmation.

<!-- CUSTOMIZE: On Fridays (or your last work day of the week), do a deeper sweep:
     read the full week's daily notes and meeting summaries to catch anything missed
     during the week. This weekly deep reconciliation is optional but catches a
     surprising amount of stale tasks. -->

---

## STEP 5: MOVE COMPLETED TASKS TO DONE

Using `$VAULT_PATH/Tasks/done.md`:

1. Scan active.md for ALL items marked complete -- including any completed in prior sessions and not yet removed.
2. Create a new section in done.md with today's date (MM-DD-YYYY).
3. Move completed tasks to done.md under today's header, but only if they are not already logged from a prior date.
4. Remove all completed items from active.md.

---

## STEP 6: WRITE INTERACTION LOG

<!-- WHY THIS EXISTS: This is the fuel for the self-review feedback loop. Without
     interaction logs, the self-review skill has nothing to analyze. The logs capture
     what happened, what went wrong, and what patterns are emerging.

     If you are not ready to set up the self-review yet, you can skip this step.
     But once you add it, you will wish you had logs going back further. -->

Write a log to `$VAULT_PATH/Logs/daily/YYYY-MM-DD.md` capturing what happened in this session.

Key things to capture:
- Every skill invoked this session
- Ad hoc requests made during the session (summarized by type)
- Any corrections or rephrasing moments -- tag these `[CORRECTION]` -- these are highest signal for the self-review
- Any friction points (things that took multiple attempts) -- tag these `[FRICTION]`
- Which data sources were touched

If the log file already exists (e.g., morning routine ran earlier today), append a new section rather than overwriting.

<!-- CUSTOMIZE: The log format can be whatever works for you. The important thing
     is that [CORRECTION] and [FRICTION] tags are consistent so the self-review
     skill can find them. Here is a minimal format:

     ## Evening Session -- YYYY-MM-DD

     ### Skills Run
     - /evening-routine
     - /meeting-summary (3 meetings processed)

     ### Ad Hoc Requests
     - Drafted email to vendor (1x)
     - Looked up meeting attendee background (2x)

     ### Corrections
     - [CORRECTION] Asked for priorities in numbered list, not bullets
     - [CORRECTION] Rephrased summary to lead with decisions

     ### Friction
     - [FRICTION] Calendar MCP timed out, had to retry

     ### Sources Touched
     - Vault, Calendar, Email
-->

---

## STEP 7: WEEKLY SELF-REVIEW (OPTIONAL)

<!-- WHY THIS EXISTS: The self-review is what makes the system improve over time.
     It runs on a fixed cadence (weekly recommended) and uses the interaction logs
     from the week to propose improvements. -->

<!-- CUSTOMIZE: Pick a day for your weekly review. Thursday evenings work well because
     it gives you Friday to act on any "Build" decisions. But any consistent day works.

     If today is your review day, run /self-review.
     If today is not your review day, skip this step. -->

If today is [your review day]:
- Run /self-review
- This triggers based on the date of the routine being run, not the current date. If Thursday's routine runs on Friday morning, self-review should still trigger.

Skip this step on all other days.
