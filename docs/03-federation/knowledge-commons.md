# Knowledge Commons

**Federated knowledge sharing across organizational nodes**

## Overview

A knowledge commons is a shared space where multiple nodes contribute and consume organizational knowledge — meeting summaries, project updates, funding opportunities, domain expertise — without centralizing control. Each node retains ownership of its full data; only selected summaries flow to the commons.

This is not a centralized wiki. It is a federated knowledge network where each node publishes what it chooses, and others can discover and subscribe.

---

## What Gets Shared

### Publishable (contributed to commons)

| Content Type | Format | Notes |
|-------------|--------|-------|
| Meeting summaries | Markdown | Not full transcripts — just key decisions + actions |
| Funding opportunities | YAML | Platform, deadline, domain, amount, link |
| Project status updates | Markdown | Public-facing project summaries |
| Domain expertise | Markdown | Curated knowledge on shared domains |
| Skill definitions | SKILL.md | Shared skills for all nodes to use |

### Private (stays on node)

| Content Type | Why |
|-------------|-----|
| Full meeting transcripts | Contain sensitive/personal context |
| Member personal data | Privacy |
| Treasury details | Security |
| Internal deliberations | Organizational autonomy |
| API keys, credentials | Security |

---

## Knowledge Organization

### Domain-Based Structure

The commons is organized by domain, not by node:

```
hub/knowledge/
├── regenerative-finance/
│   ├── README.md           # Domain overview
│   ├── funding-opportunities.yaml
│   └── from-nodes/
│       ├── refi-bcn/
│       └── nyc/
├── local-governance/
│   ├── README.md
│   └── from-nodes/
├── agroforestry/
│   ├── README.md
│   └── from-nodes/
└── knowledge-infrastructure/
    ├── README.md
    └── from-nodes/
```

### Per-Node Contributions

Each node publishes its contributions to a namespaced directory:

```
hub/knowledge/regenerative-finance/from-nodes/refi-bcn/
├── 260220-funding-opportunities.yaml
├── 260215-project-update-regenerant-catalunya.md
└── 260101-domain-summary.md
```

---

## Sync Protocols

### Protocol 1: Git-based (Primary)

Simplest and most reliable. No real-time, but robust.

**Node → Hub (push):**
```bash
# Run on node after meeting processing or funding discovery
cd hub-repo
git pull origin main
cp ../workspace/memory/2026-02-20.md knowledge/regen-finance/from-nodes/refi-bcn/
cp ../workspace/data/funding-opportunities.yaml knowledge/regen-finance/from-nodes/refi-bcn/
git add . && git commit -m "sync: refi-bcn 2026-02-20" && git push
```

Automate with GitHub Actions:

```yaml
# .github/workflows/sync-to-hub.yml (on node repo)
name: Sync to Hub
on:
  push:
    paths:
      - 'memory/**'
      - 'data/funding-opportunities.yaml'
  schedule:
    - cron: '0 20 * * *'  # nightly

jobs:
  sync:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Push to hub
        env:
          HUB_TOKEN: ${{ secrets.HUB_PUSH_TOKEN }}
        run: |
          git config user.email "bot@refi-bcn.org"
          git clone https://x-access-token:${HUB_TOKEN}@github.com/regen-coordination/hub hub
          mkdir -p hub/knowledge/regenerative-finance/from-nodes/refi-bcn
          cp memory/*.md hub/knowledge/regenerative-finance/from-nodes/refi-bcn/ || true
          cp data/funding-opportunities.yaml hub/knowledge/regenerative-finance/from-nodes/refi-bcn/ || true
          cd hub && git add . && git diff --staged --quiet || git commit -m "sync: refi-bcn $(date +%Y-%m-%d)" && git push
```

**Hub → Node (pull):**
```bash
# Node pulls aggregated knowledge from hub
git fetch hub
git checkout hub/main -- knowledge/
```

### Protocol 2: KOI-net (Real-time)

For real-time knowledge sharing between nodes:

1. Each node runs a KOI-net process alongside OpenClaw
2. Knowledge is indexed and queryable across all participating nodes
3. Agents can search the full knowledge commons in real-time

**Setup (requires koi-net package):**
```bash
pip install koi-net
koi-net init --node-id refi-bcn --network regen-coordination
koi-net start
```

**federation.yaml configuration:**
```yaml
knowledge-commons:
  sync-protocol: "koi-net"
  koi-net:
    node_id: "refi-bcn"
    network: "regen-coordination"
    bootstrap_peers:
      - "nyc-node.koi-net.local:8765"
      - "bloom-node.koi-net.local:8765"
```

### Protocol 3: Manual

For low-frequency networks:
- Monthly: designated person collects summaries from each node
- Manually adds to hub `knowledge/` directory
- Distributes compilation back to nodes

---

## Funding Pool Coordination

A key use of the knowledge commons is coordinating domain-based funding pools.

### Pool Discovery

When a node's `funding-scout` skill identifies an opportunity:

1. Adds to `data/funding-opportunities.yaml`:
```yaml
opportunities:
  - id: artisan-regen-finance-001
    platform: artisan
    fund: "Regen Finance Fund"
    deadline: "2026-03-31"
    domain: "regenerative-finance"
    amount_range: "$500-$5000"
    matching: true
    url: "https://artisan.com/funds/regen-finance"
    relevant_nodes:
      - "all"
    discovered_by: "refi-bcn"
    discovered_date: "2026-02-20"
```

2. Syncs to hub via automation

3. All nodes receive the opportunity on next pull

4. Relevant node agents add application tasks to `HEARTBEAT.md`

### Pool Management

The hub coordinates domain-based pools:

```
hub/funding/
├── waste-management/
│   ├── pool-config.yaml   # Pool parameters, eligibility, governance
│   └── applications/      # Submitted applications from nodes
├── regenerative-finance/
│   ├── pool-config.yaml
│   └── applications/
└── agroforestry/
    ├── pool-config.yaml
    └── applications/
```

Pool config example:
```yaml
# hub/funding/waste-management/pool-config.yaml
pool:
  name: "Waste Management Innovation Pool"
  domain: "waste-management"
  network: "regen-coordination"
  size: "$50,000"
  funding_source: "Octant Vault + Artisan"
  governance: "consensus"
  eligibility:
    - must be active node in regen-coordination network
    - project must address waste management in a bioregion
    - must have submitted at least one meeting summary in last 60 days
  allocation:
    model: "domain-weighted"
    review_period: "quarterly"
```

---

## Knowledge Aggregation on Hub

The hub can run an aggregation action to compile knowledge from all nodes:

```yaml
# hub/.github/workflows/aggregate-knowledge.yml
name: Aggregate Knowledge
on:
  push:
    paths:
      - 'knowledge/**/from-nodes/**'
  schedule:
    - cron: '0 6 * * 1'  # weekly on Mondays

jobs:
  aggregate:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Generate domain summaries
        run: |
          node scripts/aggregate-domain-knowledge.mjs
      - name: Update funding opportunities index
        run: |
          node scripts/merge-funding-opportunities.mjs
      - name: Commit aggregated knowledge
        run: |
          git add knowledge/ && git diff --staged --quiet || \
          git commit -m "chore: aggregate knowledge $(date)" && git push
```

---

## Agent Search Across Commons

When OpenClaw is configured with KOI-net or the hub workspace is available:

```
User: "What funding opportunities are available for agroforestry projects?"

Agent:
1. Searches local data/funding-opportunities.yaml
2. Searches hub/knowledge/agroforestry/ (via git or koi-net)
3. Returns consolidated list across all nodes' discoveries
```

This is the core value of the commons: any node's agent can leverage the full network's knowledge discovery.

---

## Privacy and Governance

### What Nodes Control

- Each node decides what to publish (configured in `federation.yaml`)
- Nodes can withdraw published content at any time
- Nodes can set trust levels for each peer

### Hub Governance

The hub is governed by the network's decision model. For Regen Coordination:
- Consensus required for changes to shared skills
- Any node can propose additions to knowledge domains
- Hub maintainers review and merge contributions

### Data Minimization

Publish summaries, not raw data:
- Meeting summary: decisions + actions (not transcript)
- Project update: status + milestones (not internal deliberations)
- Funding opportunity: public info only (not internal strategy)

---

## Related

- [Federation Protocol](./federation-protocol.md) — Node relationships
- [Node Clustering](./node-clustering.md) — Multi-node deployment
- [Skills Catalog: knowledge-curator](../04-skills-catalog/knowledge-curator.md) — Curation skill
- [Skills Catalog: funding-scout](../04-skills-catalog/funding-scout.md) — Funding discovery skill
