---
# No paths: field — loads unconditionally for all sessions
---

# Workspace Structure

Your workspace (`/workspace/`) is a git repository for syncing your agent context to GitHub.

## Directory Layout

```
/workspace/           <-- Git repository for context sync
├── CLAUDE.md         <-- Your identity (auto-loaded by Claude Code)
├── HEARTBEAT.md      <-- Heartbeat loop state
├── README.md         <-- Repository description (managed by you)
├── .claude/          <-- Claude Code context files
│   ├── rules/        <-- Workflow rules (this file)
│   └── settings.json <-- Claude Code settings
├── .heartbeat/       <-- Runtime data (NOT tracked by git)
└── projects/         <-- Work project repos (NOT tracked by git)
    └── some-project/ <-- Cloned repos for tasks
```

## What's Tracked by Git

**Tracked (sync to GitHub):**
- `CLAUDE.md` — Your identity
- `HEARTBEAT.md` — Heartbeat instructions
- `README.md` — Repository description
- `.claude/` — All Claude Code context files

**NOT tracked (runtime data):**
- `.heartbeat/` — Watermarks and heartbeat state
- `projects/` — Cloned project repositories

## Working with Projects

When you need to work on a project:

1. Clone into `projects/`:
   ```bash
   cd /workspace/projects
   git clone <repo_url>
   ```

2. The project's `CLAUDE.md` (if any) will be discovered lazily when you work on files in that directory.

3. Project repos are isolated from your context repo — they won't interfere with git operations in `/workspace/`.

## Git Remote

Your context repository: `https://github.com/mamkkl/agent-linus-developer`

See `workflow.md` for the Context Update Sync Protocol.
