# Core Principles

**Design principles guiding Organizational OS development**

## 1. Web3-Optional

**Principle**: Works for any organization, Web3 features are optional enhancements

**Rationale**: Not all organizations need blockchain features. Core functionality should work without Web3 dependencies.

**Implementation**:
- Core packages work without blockchain
- Web3 features in separate optional package
- Progressive enhancement model
- Can add Web3 features later

**Examples**:
- Organization can use meetings/projects without on-chain registration
- Can add token-gating later if needed
- On-chain registration is optional, not required

## 2. Standards-First

**Principle**: Built on EIP-4824 for organizational identity and interoperability

**Rationale**: Standards enable interoperability and ecosystem integration.

**Implementation**:
- Full EIP-4824 compliance
- Uses official DAOstar schemas
- Extends standards thoughtfully
- Contributes back to standards

**Examples**:
- All schemas use `http://www.daostar.org/schemas` context
- Compatible with DAOstar indexers
- Works with Snapshot, Tally, etc.

## 3. Federated by Design

**Principle**: Organizations maintain independence while sharing infrastructure

**Rationale**: Organizations need autonomy but benefit from shared patterns.

**Implementation**:
- Template-based architecture
- Upstream/downstream sync
- Independent customization
- Bidirectional contribution flow

**Examples**:
- Fork template, customize independently
- Pull updates from upstream
- Contribute improvements back
- Maintain organization-specific changes

## 4. Progressive Enhancement

**Principle**: Start simple (docs), add complexity (operations), scale (Web3)

**Rationale**: Organizations have different needs and maturity levels.

**Implementation**:
- Minimal viable setup
- Optional packages
- Can start with just knowledge base
- Add operational packages as needed

**Examples**:
- Start with documentation only
- Add meetings package when ready
- Add projects when needed
- Add Web3 features if desired

## 5. Agent-Native

**Principle**: Organizational workspaces are designed to be operated by agents (AI runtimes) as naturally as by humans

**Rationale**: As AI runtimes mature, organizations that structure their operations as machine-readable files gain compounding leverage. The same workspace a human reads, an agent reads. The same files a human writes, an agent writes.

**Implementation**:
- File system as agent memory (`MEMORY.md`, `memory/YYYY-MM-DD.md`, `HEARTBEAT.md`)
- Skills-based capability system (AgentSkills-compatible `skills/` directory)
- `AGENTS.md` operating instructions for agent runtimes
- `SOUL.md` values + `IDENTITY.md` org identity as agent grounding
- Compatible with OpenClaw, Cursor AI, and any AgentSkills runtime

**Examples**:
- OpenClaw agent processes meeting transcripts autonomously via `meeting-processor` skill
- `funding-scout` skill monitors platforms and updates `HEARTBEAT.md` with deadlines
- Schema generator auto-updates `.well-known/` after data changes
- Federated agents share knowledge via hub repository

## 6. File-as-Memory

**Principle**: Markdown files are the database. There is no separate backend.

**Rationale**: Git repositories are durable, portable, auditable, and human-readable. Keeping memory in files eliminates external dependencies, ensures data ownership, and makes all organizational history version-controlled.

**Implementation**:
- `MEMORY.md` as long-term memory index
- `memory/YYYY-MM-DD.md` as daily session logs
- `data/*.yaml` as structured operational ground truth
- `.well-known/*.json` as machine-readable schemas
- All of the above committed to Git

**Examples**:
- Meeting processed → note written to `packages/operations/meetings/`, summary to `memory/`
- Funding opportunity found → added to `data/funding-opportunities.yaml`
- Decision made → recorded in `MEMORY.md` key decisions section
- Full audit trail: `git log` shows every change, by whom, when

## Additional Principles

### Open Source

- All code open source (MIT license)
- Community contributions welcome
- Transparent development process

### Data Ownership

- Organizations own their data
- Data stored in Git repository
- No external dependencies required
- Portable and exportable

### Extensibility

- Modular package system
- Easy to add custom packages
- Plugin architecture
- Integration-friendly

### Documentation

- Comprehensive documentation
- Examples and case studies
- Best practices guides
- Community resources

## Applying Principles

### When Adding Features

1. **Web3-Optional**: Can it work without blockchain?
2. **Standards-First**: Does it align with EIP-4824?
3. **Federated**: Can it be shared via template?
4. **Progressive**: Can it be added incrementally?
5. **AI-Native**: Can AI assist with it?

### When Making Decisions

- **Simplicity over complexity**: Prefer simple solutions
- **Standards over custom**: Use standards when available
- **Open over closed**: Prefer open source solutions
- **Community over vendor**: Build with community

## Related Documentation

- [`what-is-organizational-os.md`](what-is-organizational-os.md) - Overview
- [`when-to-use.md`](when-to-use.md) - Use cases
- [`comparison-with-alternatives.md`](comparison-with-alternatives.md) - Alternatives
