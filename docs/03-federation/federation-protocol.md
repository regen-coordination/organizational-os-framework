# Federation Protocol

**How organizational nodes connect, share, and coordinate**

## Overview

Federation is the mechanism by which independent Organizational OS nodes maintain autonomy while sharing infrastructure, knowledge, and capabilities. Federation uses Git repositories as the substrate for coordination — no central server required.

---

## Federation Concepts

### Nodes

A **node** is a single Organizational OS workspace instance. Each node:
- Has its own identity (`IDENTITY.md`)
- Maintains its own memory (`memory/`, `MEMORY.md`)
- Operates its own agent(s)
- Controls its own data

### Hub

A **hub** is a shared coordination repository. A federation network has one hub that:
- Lists all participating nodes
- Aggregates shared knowledge
- Distributes shared skills
- Coordinates domain-based funding pools
- Does NOT control individual nodes

### Network

A **network** is the full set of nodes + hub for a particular coordination context.

```
regen-coordination network
├── hub (regen-coordination/hub)
├── ReFi BCN node
├── NYC node
├── Bloom node
└── GreenPill node
```

---

## federation.yaml v3.0 Specification

The `federation.yaml` file in each workspace declares the node's federation relationships.

### Full Specification

```yaml
# ── Identity ─────────────────────────────────────────────────────────────────
identity:
  name: string              # Organization/node name
  type: string              # DAO | Cooperative | Project | Foundation | Collective | LocalNode
  daoURI: string            # EIP-4824 identity URI
  emoji: string             # Visual identifier (optional)
  chain: string             # Primary chain (e.g., eip155:100) (optional)
  safe: string              # Gnosis Safe address (optional)
  hats: integer             # Hats Protocol tree ID (optional)

# ── Federation Network ────────────────────────────────────────────────────────
federation:
  version: "3.0"
  spec: "organizational-os/3.0"
  
  network: string           # Network identifier (e.g., "regen-coordination")
  hub: string               # Hub repo (e.g., "github.com/regen-coordination/hub")
  
  upstream:                 # Template/base repos this node inherits from
    - repo: string          # e.g., "organizational-os/organizational-os-template"
      url: string           # Full URL
      sync: boolean         # Whether to auto-sync
      last_sync: date       # ISO-8601

  peers:                    # Sibling nodes in the network
    - name: string          # Peer display name
      repo: string          # GitHub repo path
      url: string           # Full URL
      trust: string         # full | read | none

  downstream:               # Nodes that inherit from this one
    - name: string
      repo: string
      url: string

# ── Agent Configuration ────────────────────────────────────────────────────────
agent:
  runtime: string           # openclaw | cursor | custom
  workspace: string         # Path to workspace root (default: ".")
  
  skills:                   # Skills enabled on this node
    - string                # Skill name from skills/ directory
  
  channels:                 # Communication channels enabled
    - string                # telegram | google-meet | github | discord | signal
  
  proactive: boolean        # Whether agent runs proactive heartbeat checks
  heartbeat_interval: string # Cron expression or "30m", "1h", etc.

# ── Operational Packages ──────────────────────────────────────────────────────
packages:
  knowledge_base: boolean
  meetings: boolean
  projects: boolean
  finances: boolean
  coordination: boolean
  webapps: boolean
  web3: boolean

# ── Knowledge Commons ────────────────────────────────────────────────────────
knowledge-commons:
  enabled: boolean
  
  shared-domains:           # Domains this node contributes knowledge about
    - string                # e.g., "regenerative-finance", "agroforestry"
  
  sync-protocol: string     # git | koi-net | manual
  
  publish:                  # What this node publishes to the commons
    meetings: boolean       # Meeting summaries (not transcripts)
    projects: boolean       # Project status updates
    funding: boolean        # Funding opportunities discovered
  
  subscribe:                # What this node receives from the commons
    - domain: string
      from: string          # all | hub | specific-node

# ── Governance ───────────────────────────────────────────────────────────────
governance:
  maintainers:
    - name: string
      role: owner | maintainer | contributor
  decision_model: string    # consensus | voting | hierarchical | delegated
  
# ── Metadata ─────────────────────────────────────────────────────────────────
metadata:
  created: datetime         # ISO-8601
  last_updated: datetime    # ISO-8601
  framework_version: "3.0"
```

### Minimal Example (Local Node)

```yaml
identity:
  name: "ReFi BCN"
  type: "LocalNode"
  daoURI: "https://refi-bcn.github.io/.well-known/dao.json"
  emoji: "🌱"
  chain: "eip155:100"
  safe: "0xABCD1234..."

federation:
  version: "3.0"
  spec: "organizational-os/3.0"
  network: "regen-coordination"
  hub: "github.com/regen-coordination/hub"
  
  upstream:
    - repo: "organizational-os/organizational-os-template"
      url: "https://github.com/organizational-os/organizational-os-template"
      sync: true
      last_sync: "2026-02-20"
  
  peers:
    - name: "NYC Node"
      repo: "regen-coordination/nyc-node"
      url: "https://github.com/regen-coordination/nyc-node"
      trust: "read"

agent:
  runtime: "openclaw"
  skills:
    - meeting-processor
    - funding-scout
    - knowledge-curator
    - capital-flow
  channels:
    - telegram
    - google-meet
    - github
  proactive: true
  heartbeat_interval: "30m"

packages:
  meetings: true
  projects: true
  finances: true
  coordination: true
  web3: true

knowledge-commons:
  enabled: true
  shared-domains:
    - "regenerative-finance"
    - "local-governance"
  sync-protocol: "git"
  publish:
    meetings: true
    projects: true
    funding: true
  subscribe:
    - domain: "regenerative-finance"
      from: "all"

governance:
  maintainers:
    - name: "ReFi BCN Team"
      role: "owner"
  decision_model: "consensus"

metadata:
  created: "2026-02-20T00:00:00Z"
  last_updated: "2026-02-20T00:00:00Z"
  framework_version: "3.0"
```

---

## Coordination Patterns

### Pattern 1: Hub-and-Spoke

Most common for regional networks. Hub coordinates, nodes act.

```
Hub (central coordination)
├── Aggregates knowledge from all nodes
├── Distributes shared skills
├── Maintains network member list
└── Coordinates funding pools

Nodes (autonomous actors)
├── Operate independently
├── Push summaries to hub
└── Pull shared skills from hub
```

**Implementation:**
- Each node has a GitHub Action that pushes `memory/` summaries to hub
- Hub `knowledge/` directory aggregates these
- Hub `skills/` directory is shared to all nodes via `federation.yaml` upstream

### Pattern 2: Peer Mesh

Nodes connect directly without a hub. Good for tight-knit collaborations.

```
Node A ←→ Node B ←→ Node C
          ↕
         Node D
```

**Implementation:**
- Each node lists peers in `federation.yaml`
- Nodes sync directly via Git or KOI-net
- No hub repo required

### Pattern 3: Layered Federation

Template → Hub → Nodes. Used when nodes inherit from a shared template.

```
organizational-os-template (upstream)
└── regen-coordination-hub
    ├── ReFi BCN node
    ├── NYC node
    └── Bloom node
```

---

## Sync Mechanisms

### Git-based Sync (Primary)

Nodes sync with hub and upstream via standard Git operations:

```bash
# Sync from upstream template
git fetch upstream
git merge upstream/main --allow-unrelated-histories

# Push knowledge to hub
git push hub main

# Pull shared skills from hub
git pull hub skills/
```

Automated via GitHub Actions (`sync-federation.yml`).

### KOI-net Sync (Real-time)

When real-time knowledge sharing is needed:
- Each node runs a KOI-net node
- Knowledge is indexed and discoverable across the network
- Agents can search across federation peers in real-time

See [Knowledge Commons](./knowledge-commons.md) for KOI-net setup.

### Manual Sync

For low-frequency coordination:
- Node admins manually copy/paste summaries to hub
- Works for very small networks or when automation is not available

---

## Trust Levels

| Level | Description | What peers can do |
|-------|-------------|-------------------|
| `full` | High-trust peer | Read all data, propose PRs |
| `read` | Default | Read published summaries only |
| `none` | Blocked | No sharing |

Trust levels are set per-peer in `federation.yaml`.

---

## Hub Repository Structure

```
regen-coordination/hub
├── README.md              # Network overview
├── MEMBERS.md             # Active nodes in network
├── federation.yaml        # Hub's own federation config
├── skills/                # Shared skills for all nodes
│   ├── funding-scout/
│   └── knowledge-curator/
├── knowledge/             # Aggregated knowledge commons
│   ├── regenerative-finance/
│   ├── local-governance/
│   └── agroforestry/
├── funding/               # Domain-based funding pools
│   └── waste-management/
│       ├── pool-config.yaml
│       └── applications/
└── .github/
    └── workflows/
        └── aggregate-knowledge.yml
```

---

## Related

- [Node Clustering](./node-clustering.md) — Multi-node agent coordination
- [Knowledge Commons](./knowledge-commons.md) — Federated knowledge sharing
- [Agent Architecture](../02-standards/agent-architecture.md) — Agent model
