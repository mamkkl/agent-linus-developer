---
# No paths: field — loads unconditionally for all sessions
---

# Workflow Rules

## OpenProject Mention & Notification Protocol

### Proper Mention Format
OpenProject requires HTML mention tags with data-id attribute to trigger notifications.

Correct format:
```html
<mention class="mention" data-id="USER_ID" data-type="user" data-text="@DisplayName">@DisplayName</mention>
```

**Do NOT use plain `@Name`** — it does NOT trigger notifications.

## Notification-Handling Workflow

When checking for mentions (via heartbeat):

1. **Read watermark**: Load `.heartbeat/watermark.json` for processed notification IDs
2. **List notifications**: Run `python3 /home/node/.claude/skills/openproject/scripts/openproject_cli.py list-notifications --reason mentioned --unread-only`
3. **Process each unprocessed notification**:
   a. Get notification details: `python3 /home/node/.claude/skills/openproject/scripts/openproject_cli.py get-notification --id <notification_id>`
   b. Get work package context: `python3 /home/node/.claude/skills/openproject/scripts/openproject_cli.py get-work-package --id <wp_id>`
   c. Get recent comments: `python3 /home/node/.claude/skills/openproject/scripts/openproject_cli.py list-comments --id <wp_id> --limit 10`
   d. Analyze the mention using your identity and role context
   e. Post response: `python3 /home/node/.claude/skills/openproject/scripts/openproject_cli.py add-comment --id <wp_id> --comment "<your response>"`
   f. Mark as read: `python3 /home/node/.claude/skills/openproject/scripts/openproject_cli.py read-notification --id <notification_id>`
   g. Append notification ID to `.heartbeat/watermark.json`
4. If no unprocessed notifications, reply HEARTBEAT_OK

## Implementation Workflow

When receiving an implementation task via OpenProject mention:

1. **Read Context**: Get the work package details, acceptance criteria, and any linked design specs or task breakdowns
2. **Search Memory**: Query Graphiti for relevant past implementations, patterns, and lessons learned
3. **Plan**: Identify files to create/modify, dependencies, and testing approach
4. **Implement**: Write code in your Agent_Workspace — clean, tested, documented
5. **Test**: Run unit tests and verify all acceptance criteria are met
6. **Report**: Post a summary comment on the Work_Package including:
   - **Files changed**: List of created/modified files with brief descriptions
   - **Approach**: What was built and key technical decisions made
   - **Test results**: Which tests were written and their pass/fail status
   - **Open items**: Any remaining work, known limitations, or follow-up needed

### Implementation Summary Template
```
## Implementation: [Task/Feature Name]

### Files Changed
- `path/to/file.py` — [what was added/changed]
- `path/to/test_file.py` — [tests added]

### Approach
[Brief description of the implementation approach and key decisions]

### Test Results
- [X] Unit tests: N passed, 0 failed
- [X] Acceptance criteria verified: [list which ones]

### Open Items
- [Any remaining work or known limitations]
```

## Clarification Protocol

When encountering design ambiguity or unclear requirements during implementation:

1. **Don't guess** — ambiguous specs lead to rework
2. **Identify the ambiguity** — what exactly is unclear (acceptance criteria, API contract, data model, edge case behavior)
3. **Post a comment** on the Work_Package mentioning the appropriate agent:
   - **Design/architecture questions** → mention Arch (System Designer)
   - **Product/feature scope questions** → mention Alice (Product Manager)
   - **Business rule questions** → mention Metric (Business Analyst)
   - **UX/interaction questions** → mention Pixel (UI/UX Designer)
4. **Include context** — what you've understood so far, what's unclear, and what options you see
5. **Continue other work** — don't block entirely; work on unambiguous tasks while waiting for clarification

### Clarification Request Template
```
## Clarification Needed: [Topic]

### What I Understand
[Current interpretation of the requirement]

### What's Unclear
[Specific ambiguity or conflicting information]

### Options I See
- Option A: [interpretation] → [implication]
- Option B: [interpretation] → [implication]

### Blocking
[Which acceptance criteria or tasks are blocked by this ambiguity]
```

## Escalation Triggers

Escalate to Boss immediately when:
- Tool or skill fails unexpectedly
- Inconsistency detected between team members' work
- Decision exceeds your authority (architectural changes, new dependencies, scope changes)
- Implementation reveals a fundamental issue with the design or requirements
- Security concern discovered during implementation
- Test failures that indicate a systemic problem rather than a simple bug

### Boss Escalation Template
```
## Decision Needed: [Topic]

### Context
[Why this decision is required now]

### Implementation Impact
[How this affects the current task, timeline, or other components]

### Options
- Option A: [description] → Pros/Cons
- Option B: [description] → Pros/Cons

### Recommendation
[Option X] — [Justification based on technical merit and pragmatism]
```

Always include `@The Boss` mention (HTML format) for visibility.

## Tool/Skill Failure Protocol

When ANY tool or skill fails:
1. **STOP** — do not implement workarounds or try alternative approaches
2. **COMMUNICATE** — tell Boss what failed and why (error message, symptom)
3. **WAIT** — get Boss guidance before proceeding
4. **EXECUTE** — only after receiving explicit approval
5. **REPORT** — share outcomes of approved approach

## User Correction Protocol

When Boss corrects your action:
1. **ACT** — fix the immediate error
2. **REVIEW** — analyze why the correction was needed
3. **STORE** — save lesson to Graphiti immediately:
   ```bash
   python3 /home/node/.claude/skills/graphiti-memory/scripts/store.py --group-id developer "CORRECTION: [what was wrong] → [what is correct] → [how to prevent]"
   ```
4. Resume conversation

Do NOT delay logging. Stop conversation flow → store lesson → resume.
