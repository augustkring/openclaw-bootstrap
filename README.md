# OpenClaw agent templates

Placeholder versions of the framework files that make up an OpenClaw agent's "personal layer". Copy this directory into a new agent's workspace and fill in the placeholders to bootstrap a new agent (for a new user, or a new agent for the same user).

## What's here

```
templates/
├── README.md              ← this file
├── LICENSE                ← MIT
├── .gitignore             ← standard ignores for workspace repos
│
├── IDENTITY.md            ← Tier 1 — per-agent (name, vibe, emoji)
├── USER.md                ← Tier 1 — per-user (profile, filled by BOOTSTRAP)
├── TOOLS.md               ← Tier 1 — per-user (what's actually wired up)
│
├── SOUL.md                ← Tier 2 — universal character, find-replace user name
├── AGENTS.md              ← Tier 2 — universal ops manual
├── BOOTSTRAP.md           ← Tier 2 — first-session onboarding script
├── HEARTBEAT.md           ← Tier 2 — proactive check-in protocol
├── HUMAN_OS.md            ← Tier 2 — 8-dimension coaching framework
│
├── MEMORY.md              ← Tier 4 — blank long-term memory index
├── memory/
│   └── YYYY-MM-DD.md      ← Tier 4 — example daily log
│
└── auto-memory/           ← Claude Code harness layer (not in workspace/)
    ├── README.md
    ├── MEMORY.md          ← pointer to workspace MEMORY.md
    ├── user_example.md
    ├── feedback_example.md
    ├── project_example.md
    └── reference_example.md
```

## How to instantiate a new agent

### 1. Copy the templates

```bash
cp -R templates/ /path/to/new-agent-workspace/
cd /path/to/new-agent-workspace
```

Move the top-level template files up if you copied the whole `templates/` folder:

```bash
# If you want the new workspace to be the templates/ contents directly:
mv templates/* templates/.* . 2>/dev/null
rmdir templates
```

Separately, put the `auto-memory/` contents into the Claude Code auto-memory directory for the new agent (see `auto-memory/README.md`).

### 2. Find and replace placeholders

Placeholders come in two flavours — both `{{DOUBLE_CURLY}}`, filled at different times:

- **Pre-fill (sed target)** — known before the first session. Names like `{{AGENT_NAME}}`, `{{USER_FIRST_NAME}}`, `{{WORKSPACE_PATH}}`. Run the sed command below to substitute these in bulk.
- **`{{FROM_BOOTSTRAP}}`** — a single repeated marker for fields filled *during* the BOOTSTRAP conversation (goals, patterns, routines, etc.). The sed command deliberately leaves these alone; the agent replaces them as it talks with the user in session one.

After instantiation, `grep -r '{{' .` should only turn up `{{FROM_BOOTSTRAP}}` markers. Anything else means a placeholder got missed.

The canonical pre-fill set:

**Agent identity:**
- `{{AGENT_NAME}}` — title case, e.g. `HAL`, `Ida`
- `{{AGENT_NAME_LOWER}}` — lowercase, e.g. `hal`, `ida` (used in tags, folder names)
- `{{AGENT_EMOJI}}` — single emoji, e.g. `🔴`
- `{{AGENT_ROLE_SHORT}}` — one phrase, e.g. `Personal assistant`
- `{{AGENT_ROLE_LONG}}` — fuller role, e.g. `long-term thinking partner, operational backbone, pattern spotter, mentor, and honest mirror`
- `{{AGENT_VIBE}}` — 1–2 adjectives, e.g. `Direct, sharp, calm under pressure`
- `{{AGENT_AVATAR_PATH_OR_TBD}}` — e.g. `assets/avatar.jpg` or `TBD`

**User identity:**
- `{{USER_FIRST_NAME}}` — e.g. `August`
- `{{USER_LAST_NAME}}` — e.g. `Kring`
- `{{USER_FULL_NAME}}` — e.g. `August Kring`
- `{{USER_LOCATION}}` — e.g. `Copenhagen, Denmark`
- `{{USER_TIMEZONE}}` — e.g. `CET / CEST`
- `{{USER_PRIMARY_LANGUAGE}}` — e.g. `Danish`
- `{{USER_WORKING_LANGUAGE}}` — e.g. `English`
- `{{USER_COMMUNICATION_STYLE}}` — one line, e.g. `Direct, honest, value-driven, no filler`
- `{{USER_CURRENT_WORK}}` — one line (for auto-memory only)

**Paths:**
- `{{WORKSPACE_PATH}}` — absolute path to this workspace, e.g. `/home/ida/.openclaw/workspace`
- `{{VAULT_PATH}}` — absolute path to the Obsidian vault if one exists, e.g. `/home/ida/vault` (else delete the line in `AGENTS.md` step 5)

**Dates:**
- `{{TODAY}}` — ISO date you're instantiating, e.g. `2026-04-21`

**Quick find-replace (macOS / Linux):**

```bash
# From inside the new workspace root
find . -type f -name '*.md' -exec sed -i \
  -e 's|{{AGENT_NAME}}|Ida|g' \
  -e 's|{{AGENT_NAME_LOWER}}|ida|g' \
  -e 's|{{AGENT_EMOJI}}|🟢|g' \
  -e 's|{{USER_FIRST_NAME}}|<first>|g' \
  -e 's|{{USER_LAST_NAME}}|<last>|g' \
  -e 's|{{USER_FULL_NAME}}|<first> <last>|g' \
  -e 's|{{USER_LOCATION}}|<city>, <country>|g' \
  -e 's|{{USER_TIMEZONE}}|<tz>|g' \
  -e 's|{{WORKSPACE_PATH}}|/home/ida/.openclaw/workspace|g' \
  -e 's|{{TODAY}}|2026-04-21|g' \
  {} +
```

Run it once, then open `IDENTITY.md` and `USER.md` by hand — those need real content in them, not just substitutions.

### 3. Fill in the profile

Two files need actual authoring, not just find-replace:

- **`IDENTITY.md`** — confirm the agent's role, vibe, and emoji read right after substitution.
- **`USER.md`** — walk through `BOOTSTRAP.md` with the user. That conversation replaces every `{{FROM_BOOTSTRAP}}` marker with real content.

Run `BOOTSTRAP.md` as an actual dialogue. Don't let the user rate themselves 3+ on every Human OS dimension. Push for evidence.

### 4. Wire up tools

Edit `TOOLS.md` — flip `❌ Not connected` to `✅ Connected` as each tool comes online. `HEARTBEAT.md` only scans what's listed as connected, so this is load-bearing.

### 5. Delete BOOTSTRAP.md

Once the BOOTSTRAP conversation is complete and `USER.md` is filled in, delete `BOOTSTRAP.md`. It's first-session only.

### 6. First session check

Before you consider the agent live, confirm:

- [ ] `IDENTITY.md` reads like a real agent, not a template
- [ ] `USER.md` has real content in every section (no `{{FROM_BOOTSTRAP}}` markers left)
- [ ] `TOOLS.md` reflects what's actually wired up
- [ ] `MEMORY.md` has at least a "session 1 notes" entry seeded from BOOTSTRAP
- [ ] Today's `memory/YYYY-MM-DD.md` exists with the first session's log
- [ ] `BOOTSTRAP.md` has been deleted
- [ ] Auto-memory `MEMORY.md` points at the workspace `MEMORY.md` path
- [ ] `grep -r '{{' .` turns up nothing

## What's intentionally not in this template

- **`DESIGN-DNA.md`** — that's a per-user artefact (design taste varies). Agents doing UI work should author their own.
- **Scripts** (`github-backup.sh`, `mirror-sync.sh`) — operational, path-dependent, copy separately from the source agent if needed.
- **`assets/`** — avatar and similar assets are per-agent.

## Placeholder syntax

All placeholders use `{{DOUBLE_CURLY}}`. Chosen because:
- Easy to grep (`grep -r '{{' .`) to confirm nothing was missed
- Easy to sed-replace
- Doesn't collide with Markdown, frontmatter, or shell syntax
- Obvious in a rendered preview that something still needs filling in
