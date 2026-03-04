# Workspace File System Specification

**The standard layout for agent-compatible organizational workspaces**

## Overview

Organizational OS v2 adopts a file-system-first architecture compatible with OpenClaw (and other AgentSkills-compatible runtimes). Every organizational workspace is both a Git repository and a live agent workspace — the files **are** the memory, the identity, and the operating instructions.

This specification defines the standard file layout, each file's role, and how files interact.

---

## Standard Layout

```
org-workspace/
├── AGENTS.md              # Agent operating instructions
├── SOUL.md                # Organization soul — values, mission, voice
├── IDENTITY.md            # Org identity: name, type, daoURI, chain addresses
├── USER.md                # Primary operator profile
├── TOOLS.md               # Environment-specific config (endpoints, keys notes)
├── MEMORY.md              # Long-term memory index
├── HEARTBEAT.md           # Active monitoring tasks and health checks
├── BOOTSTRAP.md           # First-run onboarding ritual
│
├── memory/                # Daily organizational memory logs
│   └── YYYY-MM-DD.md
│
├── skills/                # Agent capability definitions (AgentSkills format)
│   ├── meeting-processor/
│   │   └── SKILL.md
│   ├── funding-scout/
│   │   └── SKILL.md
│   ├── knowledge-curator/
│   │   └── SKILL.md
│   ├── capital-flow/
│   │   └── SKILL.md
│   └── schema-generator/
│       └── SKILL.md
│
├── .well-known/           # EIP-4824 machine-readable schemas
│   ├── dao.json
│   ├── members.json
│   ├── meetings.json
│   ├── projects.json
│   └── finances.json
│
├── data/                  # Structured operational data
│   ├── members.yaml
│   ├── projects.yaml
│   └── finances.yaml
│
├── packages/              # Modular operational modules
│   ├── operations/
│   │   ├── meetings/
│   │   ├── projects/
│   │   └── finances/
│   ├── coordination/
│   ├── webapps/
│   └── web3/
│
├── federation.yaml        # Federation manifest (v3.0+)
└── package.json
```

---

## Core Files

### `AGENTS.md` — Operating Instructions

The primary instruction document read by agents at session start. Defines:
- Startup ritual (which files to read, in what order)
- Memory system usage
- Safety boundaries and external action policies
- Group/channel behavior rules
- Heartbeat and proactive check cadence

**Required fields:**
- Session startup sequence
- Memory read/write protocol
- Safety policy (what can be done autonomously vs. requiring approval)

**Session startup sequence:**
1. Read `SOUL.md` (understand values and voice)
2. Read `IDENTITY.md` (know who you are as an org)
3. Read `USER.md` (know who you're helping)
4. Read `MEMORY.md` and latest `memory/YYYY-MM-DD.md`
5. Read `HEARTBEAT.md` (check active tasks)
6. Read `TOOLS.md` (environment specifics)
7. Load skills from `skills/`

---

### `SOUL.md` — Organization Soul

Defines the organization's character, values, and voice. Informs how agents communicate on behalf of the organization.

**Required fields:**
- Mission statement
- Core values
- Communication voice and tone
- What the organization is / is not
- Boundaries (what the organization will never do)

**Example:**
```markdown
# SOUL.md

## Mission
We coordinate regenerative finance actors across bioregions to unlock
capital flows toward ecological restoration.

## Values
- Regenerative over extractive
- Local autonomy within federated commons
- Open by default, private by exception

## Voice
Plain, direct, people-first. No hype. No jargon without definition.
Authoritative but accessible.

## We are not
A VC fund. A single team. A gatekeeper.
```

---

### `IDENTITY.md` — Organizational Identity

Bridges OpenClaw's agent identity model with EIP-4824's organizational identity standard.

**Required fields:**
- `name`: Organization name
- `type`: Organization type (DAO, Cooperative, Project, Foundation, Collective, etc.)
- `daoURI`: Link to EIP-4824 compliant identity document
- `emoji` / `avatar`: Visual identity

**Optional fields:**
- `chain`: Primary blockchain (e.g., `eip155:1`)
- `safe`: Gnosis Safe address(es)
- `hats`: Hats Protocol tree ID
- `gardens`: Gardens DAO address

**Example:**
```markdown
# IDENTITY.md

- **Name:** ReFi BCN
- **Type:** Local Node / Cooperative
- **Emoji:** 🌱
- **daoURI:** https://refi-bcn.github.io/.well-known/dao.json
- **Chain:** eip155:100 (Gnosis Chain)
- **Safe:** 0xABCD...
- **Hats Tree:** 12345
```

---

### `USER.md` — Operator Profile

Profile of the primary human operator. Personalizes agent behavior.

**Fields:**
- Name and pronouns
- Timezone
- Working preferences (language, response style, autonomy level)
- Active domains and projects
- Communication channels

---

### `TOOLS.md` — Environment Notes

Environment-specific configuration that belongs to this deployment, not to skills. Skills define *how* tools work; TOOLS.md defines *your* specifics.

**Examples of what goes here:**
- API endpoint URLs for self-hosted services
- Channel IDs for Telegram groups
- GitHub org/repo names
- Safe wallet addresses in use
- DePIN node connection details

---

### `MEMORY.md` — Long-Term Memory Index

Lightweight index for persistent organizational memory. Detailed notes live in `memory/YYYY-MM-DD.md`.

**Structure:**
```markdown
## Quick Index
- Identity: `IDENTITY.md`
- Operator: `USER.md`
- Recent sessions: `memory/`

## Key Decisions
- [2026-02-20] Adopted domain-based funding pool structure
- [2026-01-15] Moved to OpenClaw for agent runtime

## Active Context
- [ongoing] Regen Coordination: Impact Stake proposal
- [ongoing] Artisan fund creation for knowledge commons domain
```

---

### `HEARTBEAT.md` — Active Monitoring

A living checklist of active organizational tasks and system health checks. Agents consult this proactively.

**Structure:**
```markdown
## Active Tasks
### Funding
- [ ] Submit Artisan application by March 1
- [x] Octant vault strategy confirmed

### Governance
- [ ] Review proposal #42 before Thursday

## System Health
- [ ] Check Telegram bot connectivity
- [x] Schema generation pipeline verified

## Reminders
- [ ] Weekly check-in with federation peers
```

---

### `BOOTSTRAP.md` — First-Run Onboarding

A one-time ritual run when an agent is first deployed in this workspace. Guides the agent through:
1. Understanding the organization
2. Setting up channels and integrations
3. Initializing memory
4. Running first heartbeat check

---

## Memory System

### Daily Logs (`memory/YYYY-MM-DD.md`)

Short, dated entries for session notes, decisions, and context. Format:

```markdown
# 2026-02-20

## Sessions
- Council sync: Discussed Impact Stake split, Artisan fund creation
- Key decision: Domain-based approach for first funding pool

## Actions Taken
- Created regen-coordination GitHub org
- Approved Artisan applications from Swift

## Next
- Follow up with Benjamin Life after break
- Prepare Impact Stake proposal draft
```

### Memory Hierarchy

1. `HEARTBEAT.md` — Active tasks (most urgent)
2. `MEMORY.md` — Quick index and key decisions
3. `memory/YYYY-MM-DD.md` — Daily session context
4. `data/*.yaml` — Structured operational data (ground truth)

---

## Skills Directory

Skills define *what agents can do* in this workspace. Each skill is an AgentSkills-compatible directory:

```
skills/
└── <skill-name>/
    ├── SKILL.md          # Required: skill documentation
    ├── scripts/          # Optional: helper scripts
    ├── references/       # Optional: reference data
    └── assets/           # Optional: templates
```

See [Skill Specification](./skill-specification.md) for the full format.

**Skills vs. Packages:**
- **Skills** teach agents capabilities (what to do, how to do it)
- **Packages** provide operational structure (templates, schemas, scripts)
- Skills often reference package content (e.g., meeting-processor skill uses `packages/operations/meetings/templates/`)

---

## Operational Packages

Packages provide modular operational structure that complements skills. They are not agent-specific — they work with or without an agent runtime.

```
packages/
├── operations/
│   ├── meetings/        # Meeting templates, action item formats
│   ├── projects/        # Project templates (IDEA framework)
│   └── finances/        # Budget/expense templates
├── coordination/        # Multi-org coordination tools
├── webapps/             # Interactive operational tools
└── web3/                # Optional: on-chain features
```

---

## EIP-4824 Integration

The `.well-known/` directory holds machine-readable organizational schemas:

| File | Content |
|------|---------|
| `dao.json` | Main org identity (EIP-4824 daoURI) |
| `members.json` | Membership registry |
| `meetings.json` | Meeting registry (generated from markdown) |
| `projects.json` | Project registry (generated from YAML + markdown) |
| `finances.json` | Financial registry (generated from YAML) |

These files are auto-generated by the `schema-generator` skill from content in `data/` and `packages/operations/`.

---

## Federation Manifest

`federation.yaml` (v3.0+) declares:
- Organizational identity
- Upstream template relationships
- Peer node relationships
- Agent configuration (runtime, enabled skills, channels)
- Knowledge commons sharing settings

See [Federation Protocol](../03-federation/federation-protocol.md) for the full spec.

---

## Progressive Adoption

Organizational OS is designed for progressive adoption:

| Stage | Files | Complexity |
|-------|-------|-----------|
| **Minimal** | `AGENTS.md`, `SOUL.md`, `IDENTITY.md`, `MEMORY.md` | Very low |
| **Operational** | + `USER.md`, `TOOLS.md`, `HEARTBEAT.md`, `memory/` | Low |
| **Agent-enabled** | + `skills/`, `BOOTSTRAP.md` | Medium |
| **Full stack** | + `.well-known/`, `data/`, `packages/` | Medium |
| **Federated** | + `federation.yaml` with peers, knowledge-commons | High |

Start minimal. Add complexity when you're ready.
