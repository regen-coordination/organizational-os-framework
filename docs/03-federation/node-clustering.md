# Node Clustering

**Running coordinated agent networks across multiple nodes**

## Overview

Node clustering connects multiple organizational agent deployments so they can share context, divide labor, and coordinate autonomously. In the Regen Coordination model, each local node (ReFi BCN, NYC, Bloom, GreenPill) operates its own agent that handles local context — then contributes to and draws from a shared coordination layer.

---

## Why Cluster?

| Single Node | Clustered Nodes |
|-------------|-----------------|
| All context in one workspace | Context distributed, shared selectively |
| One agent handles everything | Specialized agents per domain |
| Scale limited by single machine | Horizontal scale across DePIN/VPS |
| Single point of failure | Resilient (nodes operate independently) |
| One organization's perspective | Multiple local perspectives aggregated |

---

## Clustering Architecture

### Minimum Viable Cluster

Two nodes + shared hub:

```
Node A (ReFi BCN - DePIN)
└── workspace/
    ├── AGENTS.md
    ├── skills/meeting-processor/
    └── federation.yaml → hub: regen-coordination/hub, peers: [Node B]

Node B (NYC - VPS)
└── workspace/
    ├── AGENTS.md
    ├── skills/meeting-processor/
    └── federation.yaml → hub: regen-coordination/hub, peers: [Node A]

Hub (regen-coordination/hub - GitHub)
└── knowledge/ ← receives from both nodes
└── skills/ → distributed to both nodes
```

### Full Production Cluster

```
                    ┌─────────────────────┐
                    │  regen-coordination  │
                    │      /hub           │
                    │  (GitHub repo)      │
                    └──────────┬──────────┘
                               │
               ┌───────────────┼───────────────┐
               │               │               │
    ┌──────────▼───┐  ┌────────▼────┐  ┌──────▼──────┐
    │  ReFi BCN    │  │    NYC      │  │   Bloom     │
    │  DePIN Node  │  │  VPS Node   │  │  VPS Node   │
    │              │  │             │  │             │
    │ Telegram BCN │  │ Telegram NYC│  │ Telegram Bl │
    │ Google Meet  │  │ Google Meet │  │ GitHub      │
    └──────────────┘  └─────────────┘  └─────────────┘
```

---

## Setting Up a Cluster

### Step 1: Each Node Has Its Own Workspace

Each node is a forked `organizational-os-template` configured for that node:

```bash
# Node: ReFi BCN
git clone https://github.com/organizational-os/organizational-os-template refi-bcn-node
cd refi-bcn-node
npm run setup  # Interactive setup: name=ReFi BCN, type=LocalNode, network=regen-coordination
```

### Step 2: Configure federation.yaml on Each Node

```yaml
# refi-bcn-node/federation.yaml
identity:
  name: "ReFi BCN"
  type: "LocalNode"

federation:
  network: "regen-coordination"
  hub: "github.com/regen-coordination/hub"
  peers:
    - name: "NYC Node"
      repo: "regen-coordination/nyc-node"
      trust: "read"
    - name: "Bloom Node"
      repo: "regen-coordination/bloom-node"
      trust: "read"
```

### Step 3: Set Up the Hub

Fork or create the hub repository:

```bash
git clone https://github.com/organizational-os/organizational-os-template hub
cd hub
npm run setup  # type=Hub, network=regen-coordination
```

Configure hub `federation.yaml`:

```yaml
identity:
  name: "Regen Coordination Hub"
  type: "Hub"

federation:
  network: "regen-coordination"
  
  downstream:
    - name: "ReFi BCN"
      repo: "regen-coordination/refi-bcn"
      trust: "full"
    - name: "NYC Node"
      repo: "regen-coordination/nyc-node"
      trust: "full"
    - name: "Bloom Node"
      repo: "regen-coordination/bloom-node"
      trust: "full"
```

### Step 4: Set Up Agent on Each Node

Deploy OpenClaw (or another runtime) pointing at the node workspace:

```bash
# Example: OpenClaw on DePIN node
export OPENCLAW_WORKSPACE=/opt/refi-bcn/workspace
openclaw start --config openclaw.json
```

Configure OpenClaw channels for the node:
```json
{
  "channels": {
    "telegram": {
      "bot_token": "${TELEGRAM_BOT_TOKEN}",
      "allowed_groups": ["@refibcn", "@refibcn_council"]
    }
  }
}
```

---

## Cluster Coordination Patterns

### Pattern: Async Knowledge Sharing

Nodes push knowledge summaries to hub asynchronously (every day or on events):

```yaml
# .github/workflows/share-knowledge.yml (on each node)
on:
  push:
    paths:
      - 'memory/**'
      - 'packages/operations/meetings/**'
  schedule:
    - cron: '0 8 * * *'  # daily at 8am

jobs:
  push-to-hub:
    steps:
      - name: Push meeting summaries to hub
        run: |
          git clone https://github.com/regen-coordination/hub hub-repo
          cp memory/*.md hub-repo/knowledge/$(basename $PWD)/memory/
          cd hub-repo && git commit -am "sync: $(date)" && git push
```

### Pattern: Shared Skill Distribution

Hub distributes skills to all nodes:

```yaml
# .github/workflows/distribute-skills.yml (on hub)
on:
  push:
    paths:
      - 'skills/**'

jobs:
  distribute:
    strategy:
      matrix:
        node: [refi-bcn, nyc-node, bloom-node]
    steps:
      - name: Update skills on ${{ matrix.node }}
        run: |
          git clone https://github.com/regen-coordination/${{ matrix.node }} node-repo
          cp -r skills/* node-repo/skills/
          cd node-repo && git commit -am "chore: sync skills from hub" && git push
```

### Pattern: Funding Pool Coordination

When a funding opportunity is discovered by any node's funding-scout:

1. Node agent writes opportunity to `data/funding-opportunities.yaml`
2. Node pushes to hub via GitHub Action
3. Hub aggregates into `funding/opportunities.yaml`
4. Other nodes pull and see the opportunity
5. Relevant node agents act on it

---

## Agent Specialization

In a cluster, different nodes can run different skill sets:

| Node | Primary Skills | Secondary Skills |
|------|----------------|-----------------|
| ReFi BCN | meeting-processor, knowledge-curator | funding-scout, capital-flow |
| NYC | funding-scout, capital-flow | meeting-processor |
| Bloom | knowledge-curator, funding-scout | meeting-processor |
| GreenPill | meeting-processor, schema-generator | funding-scout |
| Hub | (no agent) | — |

Specialization reduces redundancy and focuses each node's agent on its strengths.

---

## DePIN Node Deployment

For organizations with physical DePIN hardware:

```
DePIN Node (e.g., Helium Hotspot or custom hardware)
├── OpenClaw gateway (runs as system service)
├── Workspace mounted at /opt/org/workspace
├── Channels: Telegram (local groups)
├── Memory stored locally (not in cloud)
└── federation.yaml → syncs to GitHub hub
```

Security advantages:
- Agent memory stays on-premises
- No cloud dependency for core operations
- Selective sharing to hub (you choose what goes public)
- Self-sovereign agent infrastructure

Setup:
```bash
# On DePIN node
npm install -g openclaw
openclaw init --workspace /opt/org/workspace
systemctl enable openclaw
systemctl start openclaw
```

---

## Monitoring a Cluster

Each node's `HEARTBEAT.md` is the local health check. For cluster-level monitoring:

```markdown
# Hub's MEMBERS.md

| Node | Status | Last Sync | Active Agent |
|------|--------|-----------|--------------|
| ReFi BCN | 🟢 Active | 2026-02-20 | OpenClaw v2026.2.20 |
| NYC | 🟢 Active | 2026-02-19 | OpenClaw |
| Bloom | 🟡 Delayed | 2026-02-15 | Cursor (no runtime) |
| GreenPill | 🔴 Offline | 2026-01-30 | None |
```

---

## Related

- [Federation Protocol](./federation-protocol.md) — How nodes connect
- [Knowledge Commons](./knowledge-commons.md) — What nodes share
- [Agent Architecture](../02-standards/agent-architecture.md) — How agents work
