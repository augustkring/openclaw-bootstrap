---
title: TOOLS
tags: [stable, stable/tools, mirror]
updated: {{TODAY}}
---

*HOME › [[STABLE]] › **Tools***

# TOOLS.md — Tool Configuration

*Reference for tool-specific setup, access details, and usage notes. Don't store credentials in other files.*

## Connection status

Track what's actually wired up. Heartbeats and operations only check connected tools.

| Tool | Status | Notes |
|---|---|---|
| Email | ❌ Not connected | |
| Calendar | ❌ Not connected | |
| Slack | ❌ Not connected | |
| SMS | ❌ Not connected | |
| iMessage | ❌ Not connected | |
| Messenger | ❌ Not connected | |
| Notion | ❌ Not connected | |
| Notes / Files | ✅ Local workspace | `{{WORKSPACE_PATH}}` |
| Web search | ✅ Available | |

*Update this table as tools are connected. HEARTBEAT.md checks this to know what to scan.*

---

## Email

- **Provider:** [Gmail / Google Workspace / other]
- **Account:** [email address]
- **Access:** [Skill name or API config]
- **Status:** ❌ Not yet connected
- **Rules:**
  - Never send without permission (see AGENTS.md)
  - Draft replies in workspace before sending
  - Match {{USER_FIRST_NAME}}'s tone

## Calendar

- **Provider:** [Google Calendar / Outlook / other]
- **Account:** [linked to email above]
- **Access:** [Skill name or API config]
- **Status:** ❌ Not yet connected
- **Rules:**
  - Read freely, modify only with permission
  - Flag conflicts proactively during heartbeats
  - Protect deep work blocks

## Slack

- **Workspace:** [workspace name]
- **Account:** [username or email]
- **Access:** [Skill name or API config]
- **Status:** ❌ Not yet connected
- **Rules:**
  - Read all channels and DMs freely
  - Never send messages without permission
  - In group channels: observe, don't participate unless asked
  - DMs: draft responses for review

## SMS / iMessage

- **Provider:** iOS / Apple (SMS via carrier; iMessage via Apple ID)
- **Device:** [phone / Mac bridge]
- **Access:** [Skill name or API config]
- **Status:** ❌ Not yet connected
- **Rules:**
  - Read freely once wired
  - Never send without permission
  - Flag threads with people waiting 48h+
  - Draft replies in workspace for review

## Messenger

- **Provider:** Meta
- **Account:** [linked account]
- **Access:** [Skill name or API config]
- **Status:** ❌ Not yet connected
- **Rules:**
  - Same as SMS/iMessage — flag backlog, draft replies, never send without permission

## Notion

- **Workspace:** [workspace name]
- **Access:** [Skill name or API config]
- **Status:** ❌ Not yet connected
- **Rules:**
  - Read and search freely
  - Modify pages only with permission

## File system

- **Primary workspace:** `{{WORKSPACE_PATH}}`
- **Notes app:** [Obsidian / Notion / Apple Notes / other]
- **Access:** Local
- **Status:** ✅ Connected
- **Rules:**
  - Organise freely within the agent workspace
  - Ask before modifying files outside the workspace
  - Use trash/ instead of deleting

## Web

- **Browser access:** Skill-based (WebFetch, WebSearch via harness)
- **Status:** ✅ Available
- **Rules:**
  - Search freely for research and context
  - Don't fill out forms or create accounts without permission
  - Don't access anything requiring {{USER_FIRST_NAME}}'s credentials without instruction

---

## Adding new tools

When connecting a new tool:
1. Fill in the section above (or create a new one)
2. Update the connection status table at the top
3. Test the connection
4. Log in MEMORY.md that the tool was connected and when

Template:

```markdown
## [Tool name]
- **Purpose:** [what it's for]
- **Access:** [how to use it]
- **Status:** [✅ Connected / ❌ Not connected]
- **Rules:** [specific constraints]
```
