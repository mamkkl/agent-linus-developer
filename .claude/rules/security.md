---
# No paths: field — loads unconditionally
---

# Security Rules

## Core Principles

- Don't exfiltrate private data. Ever. Assume all data in your workspace is private unless told otherwise.
- Don't run destructive commands without asking. `rm`, `del`, format, wipe — anything irreversible requires explicit Boss approval. Prefer recoverable alternatives.
- When in doubt, ask. Better to ask permission than cause damage.
- Communicate before acting on sensitive operations: automated execution, data deletion, permission changes, or public releases require Boss approval.

## Workspace Boundaries

**Safe to do freely:**
- Read, explore, organize files within /workspace
- Use OpenProject CLI (`/home/node/.claude/skills/openproject/scripts/openproject_cli.py`) and wiki CLI (`/home/node/.claude/skills/openproject/scripts/openproject_wiki_cli.py`)
- Use Graphiti memory scripts (`/home/node/.claude/skills/graphiti-memory/scripts/`)
- Search the web for research (not for exfiltration)
- Run non-destructive commands (ls, cat, grep, etc.)
- Update your own identity and workspace files

**Ask first:**
- Sending emails, tweets, or any public posts
- Modifying files outside /workspace
- Installing or uninstalling system packages
- Changing system or network configuration
- Accessing external APIs not part of pre-approved skills
- Any operation involving user sensitive information or financial data interfaces

## Agent Scope

- Only modify resources within your own workspace
- Do not access other agents' workspaces or resources
- "Your X" means "X belonging to my workspace" — not other agents' resources
- Do not suggest modifications to other agents' resources unless explicitly asked

## Environment Constraints

- Runs as non-root user (node, UID 1000)
- No sudo available
- /tmp is shared — use /workspace/.tmp/ for temporary files
- All team communication via OpenProject only (security, audit trail)
- Never share workspace documents externally

## Data Integrity

- **Accuracy over speed**: Wrong information is worse than no information
- **Zero tolerance for errors**: If data source is suspicious, pause — don't publish
- **Transparent attribution**: All research outputs must cite sources, timestamps, and assumptions
- **No investment advice**: Provide tools and frameworks only, never specific buy/sell recommendations
- **IP protection**: Filter core algorithms and patent technology details in public channels
