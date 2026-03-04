# Agent Architecture

**How agents operate within an Organizational OS workspace**

## Overview

An Organizational OS workspace is designed to be both human-operated and agent-operated without modification. The same file system that humans read, agents read. The same markdown files that humans write, agents write. This is the core architectural principle: **the workspace is the interface**.

---

## Agent Runtime Model

### Compatibility

Organizational OS workspaces are compatible with:
- **OpenClaw** (primary reference implementation) — gateway-based, multi-channel
- Any **AgentSkills-compatible** runtime
- **Cursor AI** — via the workspace files and `.cursor/` conventions
- **Custom agents** — that follow the workspace file conventions

The workspace does not require any specific runtime. Organizations can operate with:
- No agent (humans use files directly)
- A lightweight AI assistant (Cursor, Claude, etc.)
- A full agent runtime (OpenClaw) with channels and proactive tasks

### Runtime Architecture (OpenClaw)

```
OpenClaw Gateway
├── Channels (Telegram, Google Meet, GitHub, Signal, ...)
├── Per-Agent Session Manager
└── Agent Runtime
    ├── Reads workspace files (AGENTS.md, SOUL.md, IDENTITY.md, ...)
    ├── Loads skills from skills/
    ├── Executes tools (browser, memory_search, memory_get, ...)
    └── Writes to memory/YYYY-MM-DD.md
```

---

## Session Lifecycle

### Startup

When an agent session starts in an Organizational OS workspace:

1. **Read `AGENTS.md`** — load operating instructions
2. **Read `SOUL.md`** — internalize organizational values and voice
3. **Read `IDENTITY.md`** — understand organizational identity
4. **Read `USER.md`** — understand the human operator
5. **Read `MEMORY.md`** — load long-term memory index
6. **Read `memory/YYYY-MM-DD.md`** — load most recent daily memory
7. **Read `HEARTBEAT.md`** — check active tasks and alerts
8. **Read `TOOLS.md`** — load environment-specific config
9. **Load skills** — discover and load from `skills/`

### During Session

- **Write to `memory/YYYY-MM-DD.md`** for significant events
- **Update `HEARTBEAT.md`** when tasks are completed or created
- **Write to `data/*.yaml`** or `packages/` content when operating on org data
- **Generate schemas** via schema-generator skill when data changes

### Session End

- Write a summary to daily memory
- Flag any open items in HEARTBEAT.md
- Ensure no external action was taken without approval (if outside safe scope)

---

## Safety Model

### Autonomous vs. Approved Actions

Actions are classified into two categories:

**Autonomous (can do without asking):**
- Reading any workspace file
- Writing to `memory/`, `MEMORY.md`, `HEARTBEAT.md`
- Writing meeting notes, project updates to `packages/`
- Generating schemas to `.well-known/`
- Sending messages to channels the agent was assigned to

**Requires approval:**
- Sending messages to external parties not in active session
- Executing on-chain transactions (even if just queuing)
- Publishing to external platforms (newsletters, social media)
- Modifying `IDENTITY.md`, `SOUL.md`, `AGENTS.md`
- Any financial action (treasury moves, grants)

### Scope Boundaries

Agents should:
- Never export private data outside the workspace
- Never send on behalf of the organization without explicit approval for novel messages
- Always flag when uncertain about scope
- Prefer queuing/drafting over direct execution for external actions

---

## Multi-Agent Model

### Agent Roles

An organization may run multiple agents with different roles:

| Role | Scope | Channels |
|------|-------|---------|
| **Primary** | Full workspace access | All org channels |
| **Meeting** | Meeting processing only | Google Meet, recording inbox |
| **Funding** | Funding scout + tracker | Funding channels, GitHub |
| **Knowledge** | Knowledge curation only | Telegram, read-only |

Each agent role gets its own workspace configuration (or a shared workspace with role-scoped instructions in `AGENTS.md`).

### Cross-Agent Coordination

Agents coordinate via shared workspace files:
- Agent A completes a task, marks it in `HEARTBEAT.md`
- Agent B reads `HEARTBEAT.md` on next session, knows the status
- Both agents write to `memory/YYYY-MM-DD.md` (append, don't overwrite)

---

## Federation Agent Model

### Node Agents

In a federated network, each node has its own agent deployment:

```
ReFi BCN Node (DePIN server)
├── OpenClaw gateway
├── Primary agent (workspace: org-workspace/)
├── Channels: Telegram (BCN groups), GitHub
└── Skills: meeting-processor, funding-scout, knowledge-curator

NYC Node (VPS)
├── OpenClaw gateway  
├── Primary agent (workspace: org-workspace/)
├── Channels: Telegram (NYC groups), GitHub
└── Skills: meeting-processor, funding-scout, knowledge-curator
```

### Inter-Node Communication

Nodes communicate via:
1. **Git** — pull from upstream, contribute to hub (asynchronous)
2. **KOI-net** — real-time knowledge sync (when enabled)
3. **Shared GitHub repo** — via federation hub (structured coordination)

Knowledge flows:
- Each node generates its own memory and meeting notes
- Hub aggregates across nodes (via pull or push to shared repo)
- Shared skills are distributed via federation hub

---

## Skill Loading

### Discovery Order

```
1. skills/ in current workspace    (highest priority)
2. federation peers skills/        (shared via federation.yaml peers)
3. ~/.openclaw/skills/             (managed skills, if using OpenClaw)
4. Bundled skills                  (built into runtime)
```

### Skill Activation

Skills are not always active. They activate when:
- Agent recognizes a pattern matching "When to Use" in SKILL.md
- User explicitly invokes a skill by name
- Another skill triggers it as a sub-step

### Skill Composition

Skills can reference other skills:
```markdown
## Usage
1. Receive audio → use openai-whisper skill to transcribe
2. Process transcript → use meeting-processor skill
3. Update schemas → use schema-generator skill
```

---

## Memory Architecture

### Two-Layer System

```
MEMORY.md (curated index)
└── Manually maintained
└── Key decisions, active context
└── Links to detailed entries

memory/YYYY-MM-DD.md (daily logs)
└── Auto-written by agent
└── Session notes, actions taken
└── Temporal, searchable
```

### Memory Search

When using OpenClaw, memory is searchable via:
- `memory_search` — semantic search across all memory files
- `memory_get` — targeted read of specific memory entry

In other contexts (Cursor, etc.):
- Use standard file search across `memory/`
- `MEMORY.md` index provides quick access to key decisions

---

## Workspace as Package

The Organizational OS workspace is designed to be packageable:

1. **As a GitHub repo** — fork, clone, operate
2. **As an OpenClaw workspace** — point OpenClaw at the directory
3. **As a Cursor workspace** — open as Cursor project
4. **As a deployable node** — run on DePIN or VPS with full agent stack

This multi-modal design means organizations can start with just the files, add a local AI assistant (Cursor), and graduate to a full agent runtime (OpenClaw on DePIN) as capabilities grow.

---

## Related

- [Workspace File System](./workspace-file-system.md) — File layout spec
- [Skill Specification](./skill-specification.md) — How to write skills
- [Federation Protocol](../03-federation/federation-protocol.md) — Multi-node setup
