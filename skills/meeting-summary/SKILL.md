# Meeting Summary

<!-- ============================================================
     ABOUT THIS SKILL
     This skill processes meeting notes into structured summaries with
     extracted action items. It:
     - Cross-checks your calendar to catch meetings with no notes
     - Creates clean summaries with decisions, discussion points, and action items
     - Extracts action items with owner, priority, and source tags
     - Handles both standalone and background invocation

     TIME: ~1-2 minutes (mostly automated)
     DEPENDENCIES: A meeting notes source (tool or manual)
     MCP INTEGRATIONS: Calendar (required), Meeting notes tool (recommended)
     ============================================================ -->

---

## How to get meeting data

<!-- CUSTOMIZE: Replace this section with your actual meeting notes source.
     Common options:

     OPTION A: Meeting notes tool (Granola, Otter, Fireflies, etc.)
     - Use your tool's MCP integration to pull today's meetings
     - Example: mcp__claude_ai_Granola__list_meetings with today's date range
     - This is the most reliable approach

     OPTION B: Local files
     - If your tool syncs files locally, check your Meetings/raw/ directory
     - Files may be named by meeting title rather than date, so search
       by date in the file content or frontmatter

     OPTION C: Manual notes
     - If you take notes by hand, point this skill at wherever you store them
     - The calendar cross-check (Step 0) will catch meetings with no notes

     OPTION D: No meeting notes tool yet
     - Skip the automated pull. The calendar cross-check will list your
       meetings, and you can manually add key points and action items.
     - This still adds value by ensuring no meeting is silently dropped.
-->

Primary source: Your meeting notes tool. Pull today's meetings and their content.

Fallback: Local files in `$VAULT_PATH/Meetings/raw/`. Search by date if files are not named by date.
<!-- CUSTOMIZE: Replace $VAULT_PATH with your actual vault path -->

**Timestamps:** If your meeting notes tool uses UTC timestamps, convert to your local timezone when writing summaries.

---

## Process

### STEP 0: CALENDAR CROSS-CHECK

<!-- WHY THIS EXISTS: This is the safety net. Meeting notes tools miss meetings
     (tool wasn't running, meeting was informal, phone call). The calendar cross-check
     ensures every meeting with 2+ attendees gets at least a stub in the summary,
     even if there are no notes. This prevents meetings from being silently dropped. -->

Pull today's calendar events:
<!-- CUSTOMIZE: Replace with your calendar MCP tool call.
     Example for Google Calendar:
     mcp__gws__calendar_events_list with calendarId=primary,
     timeMin=[today 00:00], timeMax=[today 23:59],
     singleEvents=true -->

For each event with 2+ attendees, check if your meeting notes source has a matching note.

For calendar events where no notes exist, output a stub in the summary file:

```
### [Meeting Name] ([time]) -- No transcript available
**Attendees:** [from calendar]
**Action items:** [prompt user: "Any quick action items from this meeting?"]
```

This ensures no meeting is silently dropped. Do not block processing of other meetings.

---

### STEP 1: CREATE SUMMARIES

Create one daily summary file at `$VAULT_PATH/Meetings/summaries/YYYY-MM-DD_Daily-Meeting-Summary.md`.

For each meeting with notes, create a summary section:

```markdown
### [Meeting Name] ([time])

**Attendees:** [list]

**Key Decisions:**
- [Decision 1]
- [Decision 2]

**Discussion Points:**
- [Topic 1]: [brief summary]
- [Topic 2]: [brief summary]

**Action Items:**
- [HIGH] [Owner]: [task description] (source: [meeting name])
- [MEDIUM] [Owner]: [task description] (source: [meeting name])
```

<!-- CUSTOMIZE: Adjust this format to match how you like summaries structured.
     Some people prefer decisions first (what was resolved). Others prefer
     discussion first (what was talked about). Lead with whatever you find
     yourself looking for most often. The self-review skill will catch it
     if you keep reformatting manually. -->

---

### STEP 2: EXTRACT ACTION ITEMS

Extract action items grouped by priority: [HIGH], [MEDIUM], [LOW]. Include owner and deadlines.

**Priority framework:**
- **HIGH:** Blocks someone else or has a hard deadline this week
- **MEDIUM:** Important but flexible timing
- **LOW:** Good to do but no immediate dependency

**Owner assignment rules:**
- If someone said "I'll do X" or was explicitly assigned, they are the owner
- If the owner can be reasonably inferred from role or topic area, assign with confidence
- If genuinely ambiguous, mark `[Owner unclear]` rather than defaulting to yourself
- Every action item must include a source tag: `(source: [meeting name])`

**Deduplication:**
- Cross-check extracted action items against `$VAULT_PATH/Tasks/done.md` to catch items already completed
- Cross-check against `$VAULT_PATH/Tasks/active.md` to avoid adding duplicates

---

### STEP 3: WRITE TO TASK LIST (CONDITIONAL)

<!-- WHY THIS EXISTS: This skill can be called standalone or as a background agent
     from the evening routine. When called from the evening routine, it should NOT
     write to active.md directly (the evening routine handles that to avoid race
     conditions). When called standalone, it writes directly. -->

**Called standalone** (e.g., `/meeting-summary`):
- Write new items directly to `$VAULT_PATH/Tasks/active.md`
- Dedup against existing items first

**Called as a background agent from evening-routine:**
- Do NOT write to active.md
- Instead, return the action item list as your final output
- The parent thread (evening routine Step 4) will handle the write

<!-- CUSTOMIZE: If you don't use the evening routine yet, always use standalone mode.
     The background agent pattern becomes relevant when you chain skills together. -->
