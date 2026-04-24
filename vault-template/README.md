# Vault Taxonomy

Your vault uses a three-tier system. Every file belongs to exactly one tier. This keeps things findable and prevents the vault from becoming a graveyard of random notes.

---

## Tier 1: Projects (`projects/`)

Living documents with a multi-week lifecycle. These get updated over time as work progresses. Each project gets its own subdirectory.

**Examples:**
- `projects/product-launch/` -- planning docs, timeline, stakeholder notes
- `projects/hiring-engineering/` -- job descriptions, candidate tracking, interview notes
- `projects/quarterly-planning/` -- OKR drafts, resource plans, strategy docs

**Rules:**
- Prefer fewer, thicker files over many thin ones. If a project folder has more than 4 active files, consolidate.
- Archive superseded files to `archive/` subdirectories rather than deleting.
- Skills can write here when output has ongoing value (e.g., a weekly 1:1 prep skill writes to `projects/ceo-1on1-prep/`).

**Drafting rule:** When drafting content tied to an active project (launch emails, job descriptions, strategy docs), write to that project's folder. Only use Working notes for scratch content with no project home.

---

## Tier 2: Working Notes (`Working notes/`)

Scratch documents and finite-lifecycle files. Created for a specific purpose, disposable after use. Think of these as your scratchpad.

**Examples:**
- Research notes for a one-time decision
- Draft outlines before they move to a project folder
- Temporary reference material

**Rules:**
- No embedded task lists. Tasks go in `Tasks/active.md` only.
- Move completed or superseded files to `Working notes/Archive/`.
- If a working note grows into something with ongoing value, promote it to a project file.

---

## Tier 3: Records (append-only, time-stamped, never edited after creation)

These are your system of record. Once created, they are not modified. This gives you a reliable historical trail.

**Directories:**
- `Daily Notes/` -- one file per day (YYYY-MM-DD.md), created by morning routine
- `Tasks/active.md` -- your running task list (this one IS edited, but it is the single source of truth for tasks)
- `Tasks/done.md` -- completed tasks organized by date
- `Meetings/raw/` -- raw meeting notes (from your meeting notes tool or manual capture)
- `Meetings/summaries/` -- processed meeting summaries (created by meeting-summary skill)
- `Logs/daily/` -- interaction logs (what Claude did each session, used by self-review)
- `Logs/reviews/` -- weekly self-review outputs
- `Snapshots/` -- periodic big-picture status documents

---

## Single Source of Truth Rules

These prevent information from drifting across files:

1. **Tasks live in `Tasks/active.md` only.** Never duplicate tasks in working notes or project files.
2. **Completed tasks live in `Tasks/done.md` only.** Remove checked items from active.md each evening.
3. **Project state lives in the most recent project file or meeting summary.** Working notes are not the live state of a decision.

---

## Getting Started

1. Copy this entire `vault-template/` directory to your preferred location.
2. If using Obsidian, open the directory as a new vault.
3. Start with just `Daily Notes/`, `Tasks/`, and `Meetings/`. Add the other directories as your system grows.
4. Create your first daily note manually, or let the morning routine skill do it.
