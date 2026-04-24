# Customization Guide

This is a framework, not a product. Everything is meant to be changed. This guide explains the moving parts and how to make them yours.

---

## How to modify existing skills

Every skill file (`SKILL.md`) is a set of instructions that Claude follows when you invoke the slash command. To change behavior, you edit the instructions.

**Common modifications:**

1. **Change the questions.** The morning check-in and evening reflection questions are suggestions. Swap them for whatever prompts are useful to you. Some people prefer "What drained your energy today?" over "What are you grateful for?"

2. **Add or remove steps.** If you do not care about gap detection, delete Step 0.5 from the morning routine. If you want to add a habit tracker check, add a new step.

3. **Change the cadence.** The self-review defaults to weekly on Thursdays. Move it to Fridays, or run it biweekly. Change the day check in the evening routine accordingly.

4. **Adjust the output format.** If you prefer bullet points over numbered lists, or want summaries to lead with action items instead of decisions, edit the format templates in the skill files.

**The `<!-- CUSTOMIZE -->` comments** mark the most common customization points. But nothing in the file is sacred -- edit anything.

---

## How to add new skills

A skill is just a markdown file at `~/.claude/skills/[skill-name]/SKILL.md`. Claude reads it when you type `/[skill-name]`.

**To create a new skill:**

1. Create the directory:
   ```bash
   mkdir -p ~/.claude/skills/my-new-skill/
   ```

2. Create `SKILL.md` inside it with your instructions.

3. Invoke it: `/my-new-skill`

**What makes a good skill:**
- A workflow you repeat at least weekly
- A process with clear inputs and outputs
- Something with enough steps that you would forget one if doing it manually
- A task where Claude can do 80% of the work and you review the last 20%

**What makes a bad skill:**
- Something you do once (just ask Claude directly)
- A workflow that changes every time (too variable to template)
- Something that requires real-time judgment at every step (Claude cannot pause and wait for context that does not exist yet)

**Skill structure tips:**
- Start with a comment block explaining what the skill does, its dependencies, and time estimate
- Number your steps clearly
- Use `<!-- CUSTOMIZE -->` comments for parts that are specific to your setup
- Include guardrails (things the skill should NEVER do)
- Include failure modes (what happens when data is missing or tools fail)

---

## How the memory system works

`MEMORY.md` is Claude's persistent context about you. It is loaded at the start of every conversation. Think of it as Claude's briefing document.

**What to put in MEMORY.md:**

1. **Who you are** -- Name, role, company. Enough context for Claude to understand your work.

2. **Where things live** -- Vault path, key file paths, directory structure. Claude needs to know where to read and write.

3. **Rules and preferences** -- How you want tasks formatted, what tone you prefer, formatting conventions for your note system.

4. **Terminology** -- Company-specific terms, acronyms, project names. Prevents Claude from guessing wrong.

5. **Tool configuration** -- Which MCP servers are connected, how to use them, known quirks.

**What NOT to put in MEMORY.md:**
- Sensitive credentials or API keys
- Content that changes daily (that belongs in your vault)
- Long lists of rules that you have not actually tested (add rules when you notice Claude doing something wrong, not preemptively)

**How it grows:**
The most natural way to grow your memory file is reactively. When Claude does something wrong, correct it, then add a rule to MEMORY.md so it does not happen again. The self-review skill accelerates this by detecting correction patterns and proposing memory additions.

---

## How the feedback loop works

The feedback loop is the mechanism by which the system improves itself. Here is how the pieces connect:

```
Evening Routine
    |
    v
Interaction Log (Logs/daily/YYYY-MM-DD.md)
    - Skills run
    - Ad hoc requests
    - [CORRECTION] tags
    - [FRICTION] tags
    |
    v (weekly)
Self-Review
    |
    v
Proposals (Build / Save / Reject)
    |
    +---> Build: task added to active.md, logged to acted-on-log.json
    +---> Save: lower-priority task added, logged to acted-on-log.json
    +---> Reject: logged to rejected.json (never resurfaces)
    |
    v
You build the improvement (edit SKILL.md, update MEMORY.md, create new skill)
    |
    v
Next week's logs reflect the change
    |
    v
Self-review detects new patterns (cycle continues)
```

**The key files:**
- `Logs/daily/` -- Raw interaction data. Written by evening routine Step 6.
- `Logs/reviews/` -- Self-review outputs. Each review reads the previous one for continuity.
- `config/rejected.json` -- Rejected proposals. Checked before every new proposal. Never auto-pruned.
- `config/acted-on-log.json` -- Acted-on proposals. Prevents re-proposing things in the pipeline.

**Getting started with the loop:**
1. Run the evening routine for at least 4-5 days to build up interaction logs
2. Make sure the evening routine writes logs (Step 6) with `[CORRECTION]` and `[FRICTION]` tags
3. Run self-review at the end of the week
4. Act on at least one "Build" proposal to see the cycle in action

---

## Tips for getting the most out of this

1. **Start simple.** Morning routine only for the first week. Add evening routine in week 2. Meeting summary when you have a notes source. Self-review when you have a week of logs.

2. **Correct Claude out loud.** When Claude formats something wrong or misses the point, say so explicitly. "No, I want it like this..." These corrections are the raw material for the feedback loop.

3. **Keep MEMORY.md lean.** Only add rules you have actually needed. A 200-line memory file full of speculative rules is worse than a 30-line file with battle-tested ones.

4. **Trust the feedback loop.** Resist the urge to preemptively optimize every skill. Run them as-is, let the friction surface naturally, and let the self-review propose fixes. The proposals are better when they come from actual usage data.

5. **Review rejected.json occasionally.** After a few months, some previously-rejected ideas may become relevant. The file is human-readable and you can remove entries to allow re-surfacing.

6. **Your vault is the single source of truth.** If information lives in two places, it will drift. Tasks in active.md only. Project state in project files only. Do not duplicate.
