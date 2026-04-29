# Setup Guide

This guide walks you through setting up your own personal OS from scratch. By the end, you will have a vault, a memory file, and your first skill running.

---

## Prerequisites

1. **Claude Code** -- You need an active Claude subscription with access to Claude Code. This is the engine that runs everything. Install at [claude.ai/code](https://claude.ai/code).

2. **Obsidian** (recommended) -- Free, local-first markdown editor with backlinks, graph view, and tags. Download at [obsidian.md](https://obsidian.md). Not strictly required (any markdown editor works), but ChrisOS is designed around Obsidian's features.

3. **A terminal** -- You will need to create directories and copy files. On Mac: Cmd + Space, type "Terminal", hit Enter.

4. **~15 minutes** for the initial setup. Another 15-30 minutes to run your first morning routine and get a feel for the flow.

---

## Step 1: Create your vault

The vault is where all your data lives. Copy the template:

```bash
# Pick your vault location -- anywhere works
cp -r vault-template/ ~/Documents/MyOS/
```

### Set up Obsidian

1. Download and install Obsidian from [obsidian.md](https://obsidian.md) if you haven't already
2. Open Obsidian
3. Click "Open" next to "Open folder as vault"
4. Select your vault folder (`~/Documents/MyOS/`)
5. Obsidian will create a `.obsidian/` config folder inside your vault (this is normal and already excluded via `.gitignore`)

### Recommended Obsidian settings

Once your vault is open:

1. **Settings > Files and links > Default location for new attachments** -- set to a subfolder like `attachments/` to keep binaries out of your notes
2. **Settings > Core plugins** -- enable these built-in plugins:
   - **Backlinks** -- shows which notes link to the current note
   - **Graph view** -- visual map of connections between your notes
   - **Tags** -- browse notes by tag
   - **Templates** -- reusable note templates
   - **Daily notes** -- quick-create today's note (optional since Claude creates these for you)
3. **Settings > Community plugins** -- consider these after you're comfortable:
   - **Obsidian Git** -- auto-backup your vault to a private GitHub repo
   - **Dataview** -- query your notes like a database (useful for task views)
   - **Calendar** -- visual calendar in the sidebar linked to daily notes

Don't worry about getting all of this right on day one. The core system works with zero Obsidian plugins. These just make browsing and navigating your vault nicer over time.

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

---

## Obsidian tips for ChrisOS

A few things to know about how Obsidian renders markdown, especially if Claude is writing to your vault:

- **`#word` mid-line creates a tag.** If you want a literal hashtag (like a Slack channel name), wrap it in backticks: `` `#channel-name` ``
- **`[[text]]` creates an internal link.** Useful for connecting notes. Don't create them accidentally.
- **`- [x]` renders with strikethrough.** Only use for actual completed tasks, not for status indicators on informational content.
- **Headers, tables, and standard markdown** work as expected.
- **The graph view** gets more useful over time as notes link to each other. The self-review and entity tagging features in ChrisOS are designed to build these connections automatically.

If you add Obsidian formatting rules to your MEMORY.md, Claude will follow them when writing to your vault. For example:

```markdown
## Obsidian Formatting Rules
- Slack channels: wrap in backticks to prevent Obsidian from creating tags
- Only use bare #word when intentionally creating a tag
- Headers (#) at the start of a line are fine (standard markdown heading)
```
