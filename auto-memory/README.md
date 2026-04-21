# Auto-memory — Claude Code harness layer

This folder contains the files that live in the **Claude Code harness auto-memory directory**, not in the agent's workspace. On a Linux host running OpenClaw, the real path is typically:

```
~/.claude/projects/-home-{{AGENT_NAME_LOWER}}--openclaw-workspace/memory/
```

(The exact folder name depends on the agent's home directory.)

## What lives here vs. in workspace/

**Auto-memory (this folder)** is what Claude Code loads into every conversation context automatically. Keep it thin:

- Terse feedback notes (dos/don'ts, preferences, corrections)
- Pointers to where real memory lives in the workspace
- Short user/project/reference notes that must be available on the *very first* turn of every session

**Workspace memory (`{{WORKSPACE_PATH}}/MEMORY.md` + `memory/YYYY-MM-DD.md`)** is the substantive long-form memory. The session-boot sequence in `AGENTS.md` tells the agent to read it at the start of every main session.

The index `MEMORY.md` in this folder should include one line that points at the workspace memory (see the template).

## File types (conventions)

- `user_*.md` — who the user is (stable facts)
- `feedback_*.md` — dos/don'ts learned from the user
- `project_*.md` — ongoing projects or initiatives
- `reference_*.md` — pointers to external systems (e.g. Linear, Grafana, Slack channels)

## Installing

On a new agent host, copy the contents of this folder to the Claude Code auto-memory directory for that agent. Then fill in real user/feedback/project/reference files as the user and the agent learn from each other.
