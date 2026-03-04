# Organizational OS Framework Agent Guide

Standards, patterns, and documentation for Organizational OS.

## Quick Facts
- **Type**: Documentation and standards repository
- **Purpose**: Standards, patterns, and documentation for Organizational OS
- **Status**: Documentation complete, schemas defined

## Structure
```
docs/
├── 01-foundation/          # Core concepts
├── 02-standards/           # EIP-4824 integration
├── 06-case-studies/        # Implementation examples
└── 07-knowledge-infrastructure/  # KOI integration

schemas/                     # JSON-LD schema definitions
├── federation.yaml
├── meetings.json-ld
├── projects.json-ld
└── finances.json-ld
```

## Cursor AI Resources

### Project-Specific Resources

**Skills**: None (uses inherited resources)

**Agents**: None (uses root-level agents as needed)

**Rules**: None (follows root-level rules)

**Master Plans**:
- `.cursor/plans/organizational_os_master_plan_35a449cd.plan.md` - Master plan

### Inherited Resources

**From Root-Level** (`.cursor/` in Zettelkasten root):
- `refi-content-generation` - Generate ReFi ecosystem content
- `quick-push` - Quick git workflow operations
- `knowledge-curator` - Deep research and synthesis
- `meeting-processor` - Process meeting transcripts
- `docs-consolidator` - Consolidate documentation
- `project-reviewer` - Analyze project status

**From User-Level** (`~/.cursor/skills/`):
- `knowledge-synthesis` - Curate and synthesize content
- `git-workflows` - Git operations and PR creation

## Context Gathering

### Essential Reading (First Pass)
1. This `AGENTS.md` file
2. `README.md` - Overview, principles, quick start
3. `docs/01-foundation/what-is-organizational-os.md` - Core concepts
4. `.cursor/plans/organizational_os_master_plan_35a449cd.plan.md` - Master plan

### Architecture Understanding
- Standards and patterns documentation
- Schemas: `schemas/` (JSON-LD: meetings, projects, finances, federation)
- Case studies: `docs/06-case-studies/` (Regen Coordination implementation)
- KOI integration: `docs/07-knowledge-infrastructure/`

### Planning Context
- Master Plan: `.cursor/plans/organizational_os_master_plan_35a449cd.plan.md`
- Two-repository model: Framework (standards) + Template (implementation)
- EIP-4824 compliance: Core standard with operational extensions

### Code Navigation
- **Documentation**: `docs/01-foundation/`, `docs/02-standards/`, `docs/06-case-studies/`
- **Schemas**: `schemas/` (JSON-LD definitions)
- **Integration**: `docs/07-knowledge-infrastructure/` (KOI)

### Search Patterns
**When looking for standards**: Check `docs/02-standards/` (EIP-4824)  
**When working on schemas**: See `schemas/` (JSON-LD definitions)  
**When understanding architecture**: Read master plan (two-repo model)

### Integration Points

**Standards consumer**: This framework defines standards consumed by:
- [organizational-os-template](https://github.com/luizfernandosg/organizational-os-template) — primary implementation template
- [dao-os](https://github.com/luizfernandosg/dao-os) — DAO-specific extension
- [regen-coordination-hub](https://github.com/regen-coordination/regen-coordination-hub) — federation hub
- [Regenerant-Catalunya](https://github.com/luizfernandosg/Zettelkasten/tree/main/03%20Libraries/Regenerant-Catalunya), [Local-ReFi-Toolkit](https://github.com/luizfernandosg/Zettelkasten/tree/main/03%20Libraries/Local-ReFi-Toolkit) — implementation nodes

**External dependencies**: EIP-4824, DAOstar, KOI-net

**Ecosystem registry**: See [docs/ECOSYSTEM.md](docs/ECOSYSTEM.md) for complete cross-repo map. See [ECOSYSTEM-MAP.md](https://github.com/luizfernandosg/Zettelkasten/blob/main/03%20Libraries/ECOSYSTEM-MAP.md) in Zettelkasten for aggregated overview.

**For Complete Context**: See root `CONTEXT-GATHERING-GUIDE.md` for organizational-os-framework section.

## Integration
Part of Organizational OS ecosystem. See also `organizational-os-template` for implementation.
