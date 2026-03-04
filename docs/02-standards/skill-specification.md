# Skill Specification

**How to write AgentSkills-compatible organizational skills**

## Overview

Skills are self-contained, discoverable packages that teach agents capabilities. They follow the AgentSkills format (compatible with OpenClaw and other AgentSkills runtimes) and live in the `skills/` directory of any organizational workspace.

---

## Skill Anatomy

Every skill is a directory containing at minimum a `SKILL.md` file:

```
skills/
└── <skill-name>/
    ├── SKILL.md          # Required: documentation and instructions
    ├── scripts/          # Optional: helper scripts
    │   └── *.mjs
    ├── references/       # Optional: reference data, schemas
    │   └── *.yaml
    └── assets/           # Optional: templates and static files
        └── *.md
```

---

## SKILL.md Format

### Required Structure

```markdown
---
name: skill-name
version: 1.0.0
description: One-line description of what this skill does
author: organization-name
metadata:
  openclaw:
    requires:
      env: []        # Required environment variables
      bins: []       # Required CLI tools
      config: []     # Required config fields
---

# Skill Name

## What This Is

Brief explanation of the skill's purpose.

## When to Use

- List of scenarios where this skill is appropriate
- Be specific about triggers

## When NOT to Use

- Scenarios where another skill or approach is better
- Avoid ambiguity

## Usage

Step-by-step instructions for the agent.

### Common Workflows

...

## Notes

Important caveats, limitations, or tips.
```

### Frontmatter Fields

| Field | Required | Description |
|-------|----------|-------------|
| `name` | Yes | Kebab-case skill identifier |
| `version` | Yes | Semantic version (e.g., `1.0.0`) |
| `description` | Yes | One-line purpose description |
| `author` | No | Skill author (org or person) |
| `metadata.openclaw.requires.env` | No | Required environment variables |
| `metadata.openclaw.requires.bins` | No | Required CLI binaries |
| `metadata.openclaw.requires.config` | No | Required OpenClaw config fields |

---

## Organizational Skill Categories

Skills in an organizational workspace fall into these categories:

### 1. Operations Skills
Handle day-to-day organizational tasks.

- `meeting-processor` — Process transcripts, extract action items
- `project-tracker` — Update project statuses, generate reports
- `finance-reporter` — Summarize financial activity

### 2. Coordination Skills
Manage relationships with other organizations and networks.

- `funding-scout` — Identify and track funding opportunities
- `knowledge-curator` — Aggregate and organize shared knowledge
- `federation-sync` — Coordinate with peer nodes

### 3. Capital Skills
Manage and orchestrate capital flows.

- `capital-flow` — Queue transactions, report on treasury
- `safe-treasury` — Manage Gnosis Safe operations (requires dao-os)
- `grant-reporter` — Generate grant reports

### 4. Agent Infrastructure Skills
Manage the agent workspace itself.

- `schema-generator` — Regenerate EIP-4824 schemas
- `heartbeat-monitor` — Proactive health checks
- `memory-writer` — Write structured memory entries

---

## Example Skill: `meeting-processor`

```markdown
---
name: meeting-processor
version: 1.0.0
description: Process meeting transcripts and update organizational records
author: organizational-os
metadata:
  openclaw:
    requires:
      env: []
      bins: []
      config: []
---

# Meeting Processor

## What This Is

Processes meeting transcripts (from Google Meet, Telegram, Zoom) into
structured meeting notes following Organizational OS conventions, extracts
action items, updates project pages, and writes to organizational memory.

## When to Use

- After receiving a meeting transcript or audio recording
- When a team member shares meeting notes to be formatted
- When asked to extract action items from a meeting

## When NOT to Use

- For informal chat logs (use knowledge-curator instead)
- For asynchronous decision logs (use project-tracker)

## Usage

### 1. Receive Input

Accept any of:
- Raw transcript text (from Granola, Otter.ai, Google Meet, etc.)
- Formatted notes to standardize
- Audio file path (if transcription skill available)

### 2. Extract Structure

From the transcript, extract:
- **Date and participants**
- **Key decisions** (look for "we decided", "agreed to", "confirmed")
- **Action items** (look for "will", "should", "by [date]", "- [ ]")
- **Discussion topics** (major themes discussed)
- **Next meeting** (if mentioned)

### 3. Write Meeting Note

Save to `packages/operations/meetings/YYMMDD [Meeting Title].md`:

\`\`\`markdown
---
categories: [Meetings]
projects:
  - "[[YYMMDD Project Name]]"
date: YYYY-MM-DD
attendees: [Name1, Name2]
---
# Meeting Title

## Key Decisions
- Decision 1
- Decision 2

## Action Items
- [ ] Task (Owner, due: date)

## Discussion
Summary of main topics.
\`\`\`

### 4. Update Memory

Write a summary to `memory/YYYY-MM-DD.md`.

### 5. Update Project Pages

For each action item with a project reference, add `- [ ]` task to the
relevant project page.

## Notes

- Preserve transcript authenticity; correct obvious misspellings only
- Use consistent terminology (see `SOUL.md` for org-specific terms)
- Always check `HEARTBEAT.md` for related open tasks before processing
```

---

## Skill Discovery

Skills are loaded in priority order:
1. `skills/` in the current workspace (highest priority)
2. Skills declared in `federation.yaml` from peer nodes (shared)
3. System-level skills (if using OpenClaw)

Skills override each other by name — workspace-local skills always win.

---

## Writing Guidelines

- Use **imperative voice** ("Process the transcript", not "Processes the transcript")
- Be **specific about triggers** — when exactly should this skill activate?
- Include **examples** of input/output where useful
- Document **failure modes** and what to do when things go wrong
- Keep skills **focused** — one skill, one domain of capability
- Reference **workspace files** explicitly (`memory/`, `data/`, `packages/`)

---

## Bundled Resources

Skills can bundle supporting resources:

```
skills/funding-scout/
├── SKILL.md
├── references/
│   ├── funding-platforms.yaml    # Known funding platforms + URLs
│   └── application-templates.md  # Standard application frameworks
└── scripts/
    └── check-deadlines.mjs       # Script to check upcoming deadlines
```

Reference these in `SKILL.md` explicitly so agents know to use them.

---

## Related

- [Workspace File System](./workspace-file-system.md) — Where skills live
- [Agent Architecture](./agent-architecture.md) — How agents load and use skills
- [Skills Catalog](../04-skills-catalog/) — Reference skill implementations
