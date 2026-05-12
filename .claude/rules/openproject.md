---
# No paths: field — loads unconditionally
---

# OpenProject CLI Reference

## Available Commands

All commands use: `python3 /home/node/.claude/skills/openproject/scripts/openproject_cli.py <command> [args]`

### Work Package Operations
- `list-work-packages --project <id> [--status ...] [--assignee ...] [--limit N]`
- `get-work-package --id <wp_id>`
- `create-work-package --project <id> --subject "..." [--type Task] [--description "..."]`
- `update-work-package --id <wp_id> [--subject ...] [--description ...] [--status ...] [--assignee ...] [--priority ...] [--type ...] [--start-date YYYY-MM-DD] [--due-date YYYY-MM-DD]`
- `update-work-package-status --id <wp_id> --status "..."`
- `add-comment --id <wp_id> --comment "..."`
- `list-comments --id <wp_id> [--all] [--author ...] [--limit N]`
- `update-comment --id <activity_id> --comment "..."`

### Relation Operations
- `list-relations --id <wp_id> [--limit N]`
- `create-relation --from-id <wp_id> --to-id <wp_id> --type <relation_type> [--description ...] [--lag N]`

### Notification Operations
- `list-notifications [--reason mentioned] [--unread-only] [--limit N]`
- `get-notification --id <notification_id>`
- `read-notification --id <notification_id>`
- `unread-notification --id <notification_id>`
- `read-all-notifications`

### Metadata Lookups
- `list-projects`
- `list-statuses`
- `list-types [--project <id>]`
- `list-priorities`
- `list-users [--query ...] [--limit N]`

### Knowledge Artifacts
- `weekly-summary --project <id> [--output path.md]`
- `log-decision --project <id> --title "..." --decision "..." [--context ...] [--impact ...] [--followup ...]`

## Wiki Commands

Use: `python3 /home/node/.claude/skills/openproject/scripts/openproject_wiki_cli.py <command> [args]`

- `read-wiki --project <id> --title "..."`
- `write-wiki --project <id> --title "..." --content "..." [--content-file path]`
- `list-wiki --project <id>`

Wiki operations use browser automation (Playwright). Slower than API calls — confirm success from output. If browser automation fails, create local markdown artifacts and advise manual wiki sync.

## Usage Notes

- Default project: `kaironinv-dot-ai` (set via `OPENPROJECT_DEFAULT_PROJECT` env var)
- Multi-line comments: use file-based approach with `--comment "$(cat /tmp/comment.txt)"`
- Always verify comment formatting after adding
- Prefer `list-statuses` / `list-types` lookups before creating/updating when values are uncertain
- Include work package IDs (e.g., `#123`) when referencing tasks in outputs
