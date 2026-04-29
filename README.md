# ChrisOS

A personal operating system built on Claude Code, Obsidian, and MCP integrations.

It runs my mornings, processes my meetings, manages my tasks, and gets smarter every week. This repo is the blueprint so you can build your own.

---

## What is this?

ChrisOS is a framework for turning Claude Code into a persistent personal assistant that actually knows your work. Instead of starting every conversation from scratch, you give Claude a structured knowledge base (an Obsidian vault), a set of automated workflows (skills), and a feedback loop that makes the whole system improve over time.

**Created by [Chris Wyman](https://www.linkedin.com/in/christopherwyman/).**

---

## Who is this for?

People who already use Claude Code and want to systematize their daily workflows, or people who have never used Claude Code and are looking for a place to start. Chiefs of Staff, ops leads, founders, EAs, anyone who manages a lot of context across many tools, and wants a system that helps them stay on top of it all.

---

## The Core Idea

1. **Vault** -- A structured markdown knowledge base (Obsidian recommended) that serves as your single source of truth. Your tasks, meeting notes, daily reflections, and project files all live here in a clear taxonomy.

2. **Skills** -- Claude Code slash commands that automate recurring workflows. A morning routine that briefs you on your day. An evening routine that processes your meetings, updates your tasks, and prompts reflection. A meeting summary skill that turns raw notes into structured summaries with extracted action items.

3. **Feedback loop** -- A weekly self-review where Claude analyzes its own interaction logs, detects patterns (things you corrected, friction points, repeated manual work), and proposes improvements. You decide what to build, save, or reject. Rejected ideas never come back. The system learns.

---

## What this gives you

Here is what a typical day looks like once this is running. These are all starting points, but you can ask Claude to modify these to fit your workflow.

**Morning (~5 minutes)**
You run `/morning-routine`. Claude checks whether last night's routine ran. It scans your calendar, email, Slack, and task list. It presents a briefing split into "FYI" and "needs your input." It proposes your top 3 priorities based on what is due, what carried over from yesterday, and what is on the calendar. You confirm or swap. Your daily note is created.

**Throughout the day**
You work normally. Claude is available for ad hoc tasks. Your vault is the shared context.

**Evening (~10 minutes)**
You run `/evening-routine`. Claude processes all your meetings into summaries with action items. It asks what got done and what did not (proposing answers based on evidence, not making you recall from scratch). It updates your task list, moves completed items to done, and flags unresponded emails. On Thursdays, it runs the self-review.

**Weekly (Thursdays, ~5 minutes)**
The self-review scans the week's interaction logs. Claude surfaces 3-5 proposals: "You corrected the meeting summary format 3 times this week -- want me to change the default?" You say Build, Save for Later, or Reject. Built items become tasks. Rejected items are logged and never resurface. Over weeks, the system molds to you.

---

## Architecture

```
You
 |
 v
Claude Code  --->  Skills (slash commands)
 |                    |
 |                    v
 |              Vault (Obsidian)
 |              - Daily Notes/
 |              - Tasks/
 |              - Meetings/
 |              - Projects/
 |              - Logs/
 |
 +--->  MCP Integrations
        - Calendar (Google, Outlook)
        - Email (Gmail)
        - Chat (Slack)
        - Meeting notes (Granola, Otter, Fireflies)
        - Project management (Notion, Linear)
```

The vault is the single source of truth. Skills read from and write to the vault. MCP integrations connect Claude to your external tools. The feedback loop (self-review) reads interaction logs and proposes changes to skills and memory.

---

## Getting Started

There are two ways to set up ChrisOS. Pick whichever fits your comfort level.

### Option A: Let Claude do it (recommended)

If you are new to Claude Code (or have never used it), this is the easiest path. Claude will ask you questions about your work and set everything up for you.

**Step 1: Install Claude Code.**
Claude Code is Anthropic's command-line tool. It runs in your terminal (the text-based interface on your computer) and can read and write files on your machine. That's what makes this whole system work.

To install it:
1. You need a Claude Pro or Team subscription at [claude.ai](https://claude.ai)
2. Follow the install instructions at [claude.ai/code](https://claude.ai/code)
3. If you've never used a terminal before: on Mac, press Cmd + Space, type "Terminal", and hit Enter. That's your terminal.

**Step 2: Download this repo.**
In your terminal, run these commands one at a time. This downloads ChrisOS into your Documents folder:

```bash
cd ~/Documents
git clone https://github.com/Chriswyman825/ChrisOS.git
cd ChrisOS
```

If you get an error about `git` not being found, you may need to install it first. On Mac, the terminal will prompt you to install developer tools -- say yes and try again.

**Step 3: Install Obsidian (recommended).**
Obsidian is a free app for browsing and editing your notes. Download it at [obsidian.md](https://obsidian.md). You don't need it to get started, but it makes navigating your vault much nicer.

**Step 4: Start Claude Code and let it set things up.**
In the same terminal (you should still be in the ChrisOS folder), run:

```bash
claude
```

This starts Claude Code. You'll see a text prompt where you can type or paste messages. Paste this:

```
I just cloned ChrisOS and I want to set up a personal operating system based on this repo. My vault should live in my Documents folder (~/Documents/MyOS/). Read the README, the skills in skills/, and the vault template. Then ask me the questions you need to tailor this to my work -- my role, my tools, my daily workflow. Once you understand my setup, scaffold everything: create my vault in ~/Documents/MyOS/, write my MEMORY.md, install and customize the skills, and walk me through my first morning routine.
```

Claude will read through the repo, then ask you a series of questions about your role, your tools (calendar, email, Slack, meeting notes), and how you work. Based on your answers, it will:
- Create your vault folder at `~/Documents/MyOS/` with the right structure
- Write a MEMORY.md file that describes who you are and how you work
- Install and customize the skills to match your setup
- Walk you through your first morning routine

This takes about 15-20 minutes.

**Step 5: Open your vault in Obsidian.**
Once Claude has created your vault, open Obsidian and click "Open" next to "Open folder as vault." Select `~/Documents/MyOS/` (or wherever Claude put your vault). Now you can browse all your notes visually.

**Step 6: Use it daily.**
Each morning, open your terminal, navigate to your vault, and start Claude Code:

```bash
cd ~/Documents/MyOS
claude
```

Then type `/morning-routine`. That's it. The system starts learning from day one. Each evening, run `/evening-routine` to close the loop.

---

### Option B: Manual setup (15 minutes)

If you already use Claude Code and prefer to set things up yourself.

#### 1. Set up your vault (~3 minutes)

Copy the `vault-template/` directory to your Documents folder (or wherever you want your vault to live):

```bash
cp -r vault-template/ ~/Documents/MyOS/
```

Then open Obsidian, click "Open" next to "Open folder as vault," and select `~/Documents/MyOS/`.

Read `vault-template/README.md` to understand the three-tier taxonomy (Projects, Working notes, Records).

#### 2. Write your MEMORY.md (~5 minutes)

Create `~/.claude/projects/-/memory/MEMORY.md`. This is how Claude remembers who you are, where your vault is, and how your system works. Start simple:

```markdown
# My OS Memory

## User Context
- Name: [Your name], [Your role] at [Your company]
- Vault: Obsidian vault at [/path/to/your/vault/]

## Vault Structure
- Daily Notes/ -- one file per day (YYYY-MM-DD.md)
- Tasks/active.md -- running task list
- Tasks/done.md -- completed tasks by date
- Meetings/summaries/ -- processed meeting summaries
- Projects/ -- living project documents

## Communication Preferences
- [How you like information presented]
- [Formatting preferences]
```

You will add to this over time. The self-review skill will even propose additions for you.

#### 3. Install your first skill (~3 minutes)

Copy the morning routine skill to your Claude skills directory:

```bash
mkdir -p ~/.claude/skills/morning-routine/
cp skills/morning-routine/SKILL.md ~/.claude/skills/morning-routine/SKILL.md
```

Open the file and follow the `<!-- CUSTOMIZE -->` comments to adapt it to your setup.

#### 4. Run it (~4 minutes)

Open Claude Code and type:

```
/morning-routine
```

Claude will walk you through your first morning briefing. It will be rough around the edges. That is expected. The system improves as you use it and the feedback loop kicks in.

---

## Skills Library

| Skill | What it does | Complexity |
|-------|-------------|------------|
| [Morning Routine](skills/morning-routine/SKILL.md) | Creates daily note, scans sources, proposes priorities, runs check-in | Medium |
| [Evening Routine](skills/evening-routine/SKILL.md) | Processes meetings, reflection prompts, task hygiene, interaction logging | High |
| [Meeting Summary](skills/meeting-summary/SKILL.md) | Turns raw meeting notes into structured summaries with action items | Medium |
| [Self-Review](skills/self-review/SKILL.md) | Weekly pattern detection and improvement proposals | Medium |

Start with morning routine. Add evening routine once morning feels solid. Add meeting summary when you have a meeting notes source connected. Add self-review once you have a week of interaction logs.

---

## The Feedback Loop

This is the part that makes ChrisOS different from a collection of prompts.

Every evening, Claude writes an interaction log that captures which skills ran, what ad hoc requests you made, what you corrected, and where there was friction. Every Thursday (or whatever cadence you choose), the self-review skill reads those logs and proposes improvements.

The key mechanics:

1. **Pattern detection** -- Corrections tagged `[CORRECTION]` are highest signal. If you rephrased something 3 times this week, Claude will notice and propose a fix.
2. **Interactive decisions** -- Proposals are presented one at a time. You choose Build (becomes a task), Save for Later (lower priority task), or Reject (logged, never resurfaces).
3. **Rejection memory** -- `rejected.json` accumulates over time. Claude checks it before every proposal. You never hear the same idea twice.
4. **Continuity** -- Reviews are saved to `Logs/reviews/`. Each week's review reads the previous one for context.

Over time, the system converges on your preferences. The things that annoyed you in week 1 are fixed by week 4. The workflows you did manually in month 1 are automated by month 3.

See [the self-review skill](skills/self-review/SKILL.md) for the full specification.

---

## Customization

This repo is a starting point. You will need to:

1. **Replace all vault paths** with your actual vault location
2. **Choose your MCP integrations** based on the tools you actually use (see [setup guide](guides/setup.md))
3. **Adjust the skill workflows** to match your role -- a founder's morning routine looks different from a Chief of Staff's
4. **Write your own MEMORY.md** that reflects your context, preferences, and terminology
5. **Add skills over time** as you identify repeated workflows worth automating

The included skills are annotated with `<!-- CUSTOMIZE -->` comments that explain what each section does and how to adapt it.

See [guides/customization.md](guides/customization.md) for a deeper walkthrough.

---

## FAQ

**How much does this cost?**
Claude Code requires a Claude subscription. MCP integrations are free (open-source tools that connect to your existing accounts). Obsidian is free for personal use.

**How much time does it take to set up?**
The quickstart takes 15 minutes. Getting the full daily loop (morning + evening + meeting summaries) tuned takes a week or two of daily use. The feedback loop starts paying off after 2-3 weeks.

**Do I need Obsidian?**
No. Any markdown-based note system works (Logseq, plain files, etc.). Obsidian is recommended because its local-first approach pairs well with Claude Code's file access, and features like backlinks and tags add value. But the core patterns are just markdown files in directories.

**Can I use this with a different AI model?**
The skills are written as Claude Code slash commands. The patterns (vault taxonomy, feedback loop, task management) are model-agnostic, but the skill files would need to be adapted for a different tool.

**What MCP integrations do I need?**
None are strictly required. The more you connect, the more useful the system becomes. Calendar and email are the highest-value starting points.

**Is this overkill?**
Maybe. If you have 2 meetings a week and a simple task list, you probably do not need this. If you are juggling 6+ meetings a day, context across multiple tools, and a task list that needs constant grooming, this pays for itself quickly.

---

## Project Structure

```
ChrisOS/
├── README.md                 # You are here
├── LICENSE                   # MIT
├── .gitignore
├── vault-template/           # Empty vault structure to copy
├── skills/                   # Annotated skill templates
│   ├── morning-routine/
│   ├── evening-routine/
│   ├── meeting-summary/
│   └── self-review/
├── guides/                   # Setup and customization guides
│   ├── setup.md
│   └── customization.md
└── examples/                 # Realistic example outputs
    ├── example-daily-note.md
    ├── example-evening-reflection.md
    ├── example-meeting-summary.md
    └── example-self-review.md
```

---

## License

MIT. See [LICENSE](LICENSE).

---

## Credits

Built by [Chris Wyman](https://www.linkedin.com/in/christopherwyman/). Born out of building a personal OS with Claude Code over several months, and wanting to share the patterns that stuck.

If you build something cool with this, I would love to hear about it.
