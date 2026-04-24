# Setup Guide

This guide walks you through setting up your own personal OS from scratch. By the end, you will have a vault, a memory file, and your first skill running.

---

## Prerequisites

1. **Claude Code** -- You need an active Claude subscription with access to Claude Code. This is the engine that runs everything.

2. **A text editor** -- Obsidian is recommended (free for personal use, local-first, great for markdown). But any markdown editor or even a plain text editor works.

3. **A terminal** -- You will need to create directories and copy files.

4. **~15 minutes** for the initial setup. Another 15-30 minutes to run your first morning routine and get a feel for the flow.

---

## Step 1: Create your vault

The vault is where all your data lives. Copy the template:

```bash
# Pick your vault location -- anywhere works
cp -r vault-template/ ~/Documents/MyOS/
```

If you are using Obsidian, open `~/Documents/MyOS/` as a new vault (File > Open Vault > Open folder as vault).

You now have:
```
MyOS/
├── Daily Notes/
├── Tasks/
│   ├── active.md
│   └── done.md
├── Meetings/
│   ├── raw/
│   └── summaries/
├── Working notes/
├── projects/
├── Logs/
│   ├── daily/
│   └── reviews/
└── Snapshots/
```

Read `vault-template/README.md` for the full explanation of the three-tier taxonomy.

---

## Step 2: Write your MEMORY.md

This is how Claude remembers your context across conversations. Create the file:

```bash
mkdir -p ~/.claude/projects/-/memory/
```

Create `~/.claude/projects/-/memory/MEMORY.md` with at minimum:

```markdown
# My OS Memory

## User Context
- Name: [Your name], [Your role] at [Your company]
- Vault: Obsidian vault at [/full/path/to/your/vault/]

## Vault Structure
- Daily Notes/ -- one file per day (YYYY-MM-DD.md)
- Tasks/active.md -- running task list with [HIGH], [MEDIUM], [LOW] sections
- Tasks/done.md -- completed tasks organized by date
- Meetings/raw/ -- raw meeting notes
- Meetings/summaries/ -- processed meeting summaries
- Projects/ -- living project documents
- Logs/daily/ -- daily interaction logs
- Logs/reviews/ -- weekly self-review outputs

## Task Rules
- Tasks live in Tasks/active.md only -- never duplicated elsewhere
- Completed tasks move to Tasks/done.md during evening routine
- Priority format: [HIGH], [MEDIUM], [LOW]

## Communication Preferences
- [How you like lists formatted -- numbered? bulleted?]
- [Tone preference -- concise? detailed?]
- [Any formatting rules for your note system]
```

You will add to this over time. The self-review skill will propose additions based on your patterns.

---

## Step 3: Install your first skill

Skills live in `~/.claude/skills/`. Each skill is a directory containing a `SKILL.md` file.

Start with the morning routine:

```bash
mkdir -p ~/.claude/skills/morning-routine/
cp skills/morning-routine/SKILL.md ~/.claude/skills/morning-routine/SKILL.md
```

Open `~/.claude/skills/morning-routine/SKILL.md` and go through every `<!-- CUSTOMIZE -->` comment. At minimum, you need to:

1. Replace `$VAULT_PATH` with your actual vault path (e.g., `/Users/yourname/Documents/MyOS`)
2. Configure your calendar and email tool calls (or comment them out if you haven't connected MCP integrations yet)

---

## Step 4: Configure MCP integrations (optional but recommended)

MCP (Model Context Protocol) integrations let Claude connect to your external tools. They are configured in `~/.claude.json`.

**Starting recommendations (in order of value):**

1. **Calendar** -- Highest value. Lets Claude see your schedule, detect conflicts, and prep for meetings.
   - Google Calendar: Use the `gws` MCP server (Google Workspace CLI)
   - Outlook: Check for available Outlook MCP servers

2. **Email** -- Second highest. Lets Claude surface unread threads and flag things needing response.
   - Gmail: Available through `gws` or through claude.ai connectors
   - Outlook: Check for available Outlook MCP servers

3. **Chat** -- Third. Lets Claude scan Slack/Teams for mentions and urgent messages.
   - Slack: Available through claude.ai connectors

4. **Meeting notes** -- Fourth. Lets Claude pull meeting transcripts automatically.
   - Granola, Otter, Fireflies: Check for MCP integrations
   - Or manually save notes to `Meetings/raw/`

**Basic MCP config structure** (`~/.claude.json`):

```json
{
  "projects": {
    "/": {
      "mcpServers": {
        "your-server-name": {
          "command": "/path/to/server/binary",
          "args": ["--flag", "value"]
        }
      }
    }
  }
}
```

Each MCP server has its own setup instructions. The specifics depend on which tools you use. Search for "[tool name] MCP server" to find available integrations.

**If you have no MCP integrations yet:** That is fine. The morning routine still works -- it just will not have calendar or email data to pull from. You can add integrations one at a time as you identify which sources are most valuable.

---

## Step 5: Run your first morning routine

Open Claude Code (or a new Claude Code session) and type:

```
/morning-routine
```

Claude will walk through the routine. The first run will be rough:
- If you have no previous daily notes, gap detection will fire
- If you have no MCP integrations, the scan step will be limited
- The check-in questions will work regardless

That is all expected. The system improves with use. After a few days, it starts feeling natural.

---

## Next steps

Once you are comfortable with the morning routine:

1. **Add the evening routine** -- This is where the real value accumulates (task hygiene, meeting processing, reflection)
   ```bash
   mkdir -p ~/.claude/skills/evening-routine/
   cp skills/evening-routine/SKILL.md ~/.claude/skills/evening-routine/SKILL.md
   ```

2. **Add meeting summary** -- Once you have a meeting notes source connected
   ```bash
   mkdir -p ~/.claude/skills/meeting-summary/
   cp skills/meeting-summary/SKILL.md ~/.claude/skills/meeting-summary/SKILL.md
   ```

3. **Add self-review** -- Once you have a week of interaction logs (from the evening routine)
   ```bash
   mkdir -p ~/.claude/skills/self-review/
   cp -r skills/self-review/ ~/.claude/skills/self-review/
   ```

4. **Grow your MEMORY.md** -- As you notice patterns in how you correct Claude, add rules to your memory file. Or let the self-review propose them.

The general principle: start simple, add complexity only when you feel the need for it.
