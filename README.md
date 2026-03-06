# Organizational OS Framework

**Standards, patterns, and guidance for agent-native organizational workspaces**

> This repository is a standalone mirror of `packages/framework` in the canonical [organizational-os monorepo](https://github.com/regen-coordination/organizational-os). For active development, use the monorepo.

Organizational OS Framework (v2) provides comprehensive documentation, schemas, and patterns for transforming GitHub repositories into complete operational workspaces — designed to be operated by both humans and AI agents. Built on EIP-4824/DAOstar standards for organizational identity, and OpenClaw/AgentSkills conventions for agent compatibility.

---

## What is Organizational OS?

Organizational OS transforms GitHub repositories into complete operational workspaces where organizations can:

- **Publish Organizational Identity** via EIP-4824 compliant schemas
- **Operate with AI Agents** through AgentSkills-compatible skills and workspace conventions
- **Manage Operations** through modular packages (meetings, projects, finances)
- **Maintain Memory** using file-based memory (daily logs, curated index, structured data)
- **Foster Federation** through peer node relationships and shared knowledge commons

## Core Principles

1. **Web3-Optional**: Works for any organization, Web3 features are optional enhancements
2. **Standards-First**: Built on EIP-4824 for organizational identity and interoperability
3. **Federated by Design**: Organizations maintain independence while sharing infrastructure
4. **Progressive Enhancement**: Start minimal, add complexity as needed
5. **Agent-Native**: Workspaces are designed for both human and agent operation
6. **File-as-Memory**: Markdown files are the database — no backend required

## Repository Structure

```
organizational-os-framework/
├── docs/
│   ├── 01-foundation/          # Core concepts and principles
│   ├── 02-standards/           # EIP-4824, workspace file system, skills, agents
│   ├── 03-federation/          # Federation protocol, node clustering, knowledge commons
│   ├── 04-skills-catalog/      # Reference skill implementations
│   ├── 06-case-studies/        # Real-world implementations
│   └── 07-knowledge-infrastructure/ # KOI integration
├── schemas/                    # JSON-LD schema definitions
│   ├── federation.yaml         # Federation manifest spec (v3.0)
│   ├── skills.json-ld          # Skill definition schema
│   ├── agents.json-ld          # Agent configuration schema
│   ├── node.json-ld            # Federation node schema
│   ├── meetings.json-ld
│   ├── projects.json-ld
│   └── finances.json-ld
└── LICENSE
```

## Quick Start

### 1. Understand the Architecture

- [What is Organizational OS?](docs/01-foundation/what-is-organizational-os.md)
- [Core Principles](docs/01-foundation/core-principles.md)
- [Workspace File System](docs/02-standards/workspace-file-system.md) — the unified file layout

### 2. Agent Architecture

- [Agent Architecture](docs/02-standards/agent-architecture.md) — how agents operate in workspaces
- [Skill Specification](docs/02-standards/skill-specification.md) — how to write skills

### 3. Federation

- [Federation Protocol](docs/03-federation/federation-protocol.md) — how nodes connect
- [Node Clustering](docs/03-federation/node-clustering.md) — multi-node deployments
- [Knowledge Commons](docs/03-federation/knowledge-commons.md) — federated knowledge sharing

### 4. Skills Catalog

- [meeting-processor](docs/04-skills-catalog/meeting-processor.md)
- [funding-scout](docs/04-skills-catalog/funding-scout.md)
- [knowledge-curator](docs/04-skills-catalog/knowledge-curator.md)
- [capital-flow](docs/04-skills-catalog/capital-flow.md)
- [contract-interaction](docs/04-skills-catalog/contract-interaction.md)

## Implementation

To implement Organizational OS for your organization:

1. Use the **[organizational-os-template](../organizational-os-template/)** — fork and run `npm run setup`
2. For DAO-specific features: see **[dao-os](../dao-os/)** for onchain modules and visual composer
3. For federated deployments: see [Federation Protocol](docs/03-federation/federation-protocol.md)

## Standards Compliance

- **EIP-4824**: Full compliance with DAO Common Interfaces standard
- **DAOstar**: Compatible with DAOstar ecosystem and indexers
- **AgentSkills**: Skills follow AgentSkills format (OpenClaw compatible)
- **JSON-LD**: Uses official `http://www.daostar.org/schemas` context

## Ecosystem

The framework defines standards for a linked ecosystem of repos. See [docs/ECOSYSTEM.md](docs/ECOSYSTEM.md) for the full registry.

**By cluster:**
- **Templates**: [organizational-os-template](../organizational-os-template/), [quartz-refi-template](../quartz-refi-template/)
- **Agent runtimes**: [openclaw-source](../openclaw-source/), [regen_eliza-refi_dao](../regen_eliza-refi_dao/)
- **DAO layer**: [dao-os](https://github.com/luizfernandosg/dao-os), [grants-os](https://github.com/luizfernandosg/grants-os), [ecosystem-canvas](https://github.com/luizfernandosg/ecosystem-canvas)
- **Knowledge infra**: [koi-net](../koi-net/), [koi-net-integration](../koi-net-integration/)
- **Federation hub**: [regen-coordination-hub](../regen-coordination-hub/)
- **Implementation nodes**: [Regenerant-Catalunya](../Regenerant-Catalunya/), [Local-ReFi-Toolkit](../Local-ReFi-Toolkit/)

**Cross-repo map**: [03 Libraries/ECOSYSTEM-MAP.md](../ECOSYSTEM-MAP.md)

## Related Projects

- **[organizational-os-template](../organizational-os-template/)** — Implementation template (fork this)
- **[dao-os](../dao-os/)** — DAO-specific implementation with visual composer and onchain modules
- **[openclaw-source](../openclaw-source/)** — Reference agent runtime
- **[koi-net](../koi-net/)** — Knowledge Organization Infrastructure for real-time federation sync

## License

MIT License — see [LICENSE](LICENSE) file for details.
