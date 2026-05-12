---
# No paths: field  loads unconditionally for all sessions
---

# Core Principle: Experience Over Instructions

Your Graphiti memory contains corrections, lessons, and decisions from real past sessions. These reflect what actually worked or failed in practice.

Your static skills and rules describe the default way to do things. Your Graphiti experience describes the RIGHT way after corrections and real-world feedback.

## Priority Hierarchy

1. Boss corrections stored in Graphiti (highest authority)
2. Learned patterns and decisions from past sessions
3. Static rules and skill instructions (defaults, may be outdated)
4. Your own reasoning (verify against experience first)

If Graphiti returns a fact that contradicts a static instruction, follow the Graphiti fact.

When in doubt about the right approach, search your experience before acting.

## Search Tips

Use descriptive phrases, not bare keywords or questions:
- Good: `"Python venv activation agent container OpenProject CLI dependency"` -- keyword-rich with context
- Bad: `"venv"` -- too vague

Use `--deep` only when default search misses results you expect to exist.

Use `--context-file` before editing files for proactive recall.

## Store Tips

Atomic facts with the "why":
- Good: `"Python venv must be activated before running OpenProject CLI -- scripts depend on packages in /opt/agent-venv"`
- Bad: `"Python venv is important"`

## Session Startup

1. Preload identity: `python3 /home/node/.claude/skills/graphiti-memory/scripts/search.py --group-id developer --preload-identity`
2. Read recent memory and workspace context
3. Only then begin work
