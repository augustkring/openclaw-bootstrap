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

Placeholders come in two flavours — both wrapped in double curly braces, filled at different times:

- **Pre-fill (sed target)** — known before the first session. Names like `{{AGENT_NAME}}`, `{{USER_FIRST_NAME}}`, `{{WORKSPACE_PATH}}`. Run the sed command below to substitute these in bulk.
- **`{{FROM_BOOTSTRAP}}`** — a single repeated marker for fields filled *during* the BOOTSTRAP conversation (goals, patterns, routines, etc.). The sed command deliberately leaves these alone; the agent replaces them as it talks with the user in session one.

After instantiation, run:

```bash
grep -r '{{' . --exclude-dir=.git --exclude-dir=auto-memory --exclude=README.md
```

The only hits should be `{{FROM_BOOTSTRAP}}` markers in `USER.md`. Anything else means a placeholder got missed.

*(Why the excludes? `auto-memory/*_example.md` files are deliberate scaffolds for future memories you'll write later — their placeholders are filled when you rename a scaffold into a real memory file. `README.md` documents the placeholder syntax using literal `{{` sequences and would trip the check otherwise.)*

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

**Quick find-replace (macOS / Linux).** Copy, set the values at the top, paste into your shell from inside the new workspace root:

```bash
# Set these, then run the block below.
AGENT_NAME="Ida"
AGENT_NAME_LOWER="ida"
AGENT_EMOJI="🟢"
AGENT_ROLE_SHORT="Personal assistant"
AGENT_ROLE_LONG="long-term thinking partner, operational backbone, pattern spotter, mentor, and honest mirror"
AGENT_VIBE="Direct, sharp, calm under pressure"
AGENT_AVATAR_PATH_OR_TBD="TBD"

USER_FIRST_NAME="First"
USER_LAST_NAME="Last"
USER_FULL_NAME="First Last"
USER_LOCATION="City, Country"
USER_TIMEZONE="CET / CEST"
USER_PRIMARY_LANGUAGE="English"
USER_WORKING_LANGUAGE="English"
USER_COMMUNICATION_STYLE="Direct, honest, value-driven, no filler"
USER_CURRENT_WORK="[one-line description]"

WORKSPACE_PATH="/home/ida/.openclaw/workspace"
VAULT_PATH="/home/ida/vault"   # set empty string if no vault
TODAY="$(date -I)"

find . -type f \( -name '*.md' -o -name 'LICENSE' \) -not -path './.git/*' -exec sed -i \
  -e "s|{{AGENT_NAME}}|${AGENT_NAME}|g" \
  -e "s|{{AGENT_NAME_LOWER}}|${AGENT_NAME_LOWER}|g" \
  -e "s|{{AGENT_EMOJI}}|${AGENT_EMOJI}|g" \
  -e "s|{{AGENT_ROLE_SHORT}}|${AGENT_ROLE_SHORT}|g" \
  -e "s|{{AGENT_ROLE_LONG}}|${AGENT_ROLE_LONG}|g" \
  -e "s|{{AGENT_VIBE}}|${AGENT_VIBE}|g" \
  -e "s|{{AGENT_AVATAR_PATH_OR_TBD}}|${AGENT_AVATAR_PATH_OR_TBD}|g" \
  -e "s|{{USER_FIRST_NAME}}|${USER_FIRST_NAME}|g" \
  -e "s|{{USER_LAST_NAME}}|${USER_LAST_NAME}|g" \
  -e "s|{{USER_FULL_NAME}}|${USER_FULL_NAME}|g" \
  -e "s|{{USER_LOCATION}}|${USER_LOCATION}|g" \
  -e "s|{{USER_TIMEZONE}}|${USER_TIMEZONE}|g" \
  -e "s|{{USER_PRIMARY_LANGUAGE}}|${USER_PRIMARY_LANGUAGE}|g" \
  -e "s|{{USER_WORKING_LANGUAGE}}|${USER_WORKING_LANGUAGE}|g" \
  -e "s|{{USER_COMMUNICATION_STYLE}}|${USER_COMMUNICATION_STYLE}|g" \
  -e "s|{{USER_CURRENT_WORK}}|${USER_CURRENT_WORK}|g" \
  -e "s|{{WORKSPACE_PATH}}|${WORKSPACE_PATH}|g" \
  -e "s|{{VAULT_PATH}}|${VAULT_PATH}|g" \
  -e "s|{{TODAY}}|${TODAY}|g" \
  {} +
```

> On macOS the system `sed` needs `-i ''` (empty string after `-i`). On Linux (and `gsed` on macOS via Homebrew) use `-i` with no argument as shown.

Run it once, then open `IDENTITY.md` and `USER.md` by hand — those still need real content filled in, not just substitutions.

### 3. Fill in the profile

Two files need actual authoring, not just find-replace:

- **`IDENTITY.md`** — confirm the agent's role, vibe, and emoji read right after substitution.
- **`USER.md`** — walk through `BOOTSTRAP.md` with the user. That conversation replaces every `{{FROM_BOOTSTRAP}}` marker with real content.

Run `BOOTSTRAP.md` as an actual dialogue. Don't let the user rate themselves 3+ on every Human OS dimension. Push for evidence.

### 4. Wire up tools

Edit `TOOLS.md` — flip `❌ Not connected` to `✅ Connected` as each tool comes online. `HEARTBEAT.md` only scans what's listed as connected, so this is load-bearing.

### 5. Delete BOOTSTRAP.md (and optionally README.md)

Once the BOOTSTRAP conversation is complete and `USER.md` is filled in, delete `BOOTSTRAP.md`. It's first-session only.

You can also delete this `README.md` at this point — the agent doesn't read it, and keeping it around just clutters the workspace. The canonical copy lives in the `openclaw-bootstrap` repo if you ever need it again.

### 6. First session check

Before you consider the agent live, confirm:

- [ ] `IDENTITY.md` reads like a real agent, not a template
- [ ] `USER.md` has real content in every section (no `{{FROM_BOOTSTRAP}}` markers left)
- [ ] `TOOLS.md` reflects what's actually wired up
- [ ] `MEMORY.md` has at least a "session 1 notes" entry seeded from BOOTSTRAP
- [ ] Today's `memory/YYYY-MM-DD.md` exists with the first session's log
- [ ] `BOOTSTRAP.md` has been deleted
- [ ] Auto-memory `MEMORY.md` points at the workspace `MEMORY.md` path
- [ ] `grep -r '{{' . --exclude-dir=.git --exclude-dir=auto-memory --exclude=README.md` turns up only `{{FROM_BOOTSTRAP}}`

## What's intentionally not in this template

- **`DESIGN-DNA.md`** — that's a per-user artefact (design taste varies). Agents doing UI work should author their own.
- **Scripts** (`github-backup.sh`, `mirror-sync.sh`) — operational, path-dependent, copy separately from the source agent if needed.
- **`assets/`** — avatar and similar assets are per-agent.

## Placeholder syntax

All placeholders use double curly braces (e.g. `AGENT_NAME` wrapped in `{{ ... }}`). Chosen because:
- Easy to grep (`grep -r '{{' .`) to confirm nothing was missed
- Easy to sed-replace
- Doesn't collide with Markdown, frontmatter, or shell syntax
- Obvious in a rendered preview that something still needs filling in
