# What is Organizational OS?

**GitHub repositories as complete operational workspaces**

## Core Concept

Organizational OS transforms GitHub repositories into complete operational workspaces where organizations can:

- **Publish Organizational Identity** via EIP-4824 compliant schemas
- **Manage Operations** through modular packages (meetings, projects, finances)
- **Maintain Knowledge Base** using Quartz-based documentation
- **Enable Interoperability** with DAO tooling and standards
- **Foster Federation** through upstream/downstream repository relationships

## The Problem It Solves

### Current Challenges

Many organizations struggle with:

- **Fragmented Tools**: Operations spread across multiple platforms (Notion, Airtable, GitHub, Discord)
- **No Standard Identity**: Difficult to establish organizational identity and interoperability
- **Knowledge Silos**: Documentation disconnected from operations
- **Tool Lock-in**: Vendor-specific solutions limit portability
- **No Interoperability**: Can't easily integrate with DAO tooling

### Organizational OS Solution

- **Single Source of Truth**: GitHub repository as operational workspace
- **Standard Identity**: EIP-4824 compliant organizational identity
- **Integrated Knowledge**: Documentation and operations in one place
- **Open Standards**: Based on open standards, portable and extensible
- **DAO Interoperable**: Works with Snapshot, Tally, and other DAO tools

## Key Features

### 1. Organizational Identity (EIP-4824)

Publish standardized organizational metadata:

- **daoURI**: Main organizational identity document
- **membersURI**: Membership registry
- **proposalsURI**: Governance proposals
- **governanceURI**: Governance documentation
- **contractsURI**: Smart contract registry

### 2. Operational Packages

Modular packages for day-to-day operations:

- **Meetings**: Meeting management, notes, action items
- **Projects**: Project tracking with IDEA framework
- **Finances**: Budget and expense tracking
- **Coordination**: Multi-organization coordination tools

### 3. Knowledge Base

Quartz-based documentation system:

- **Markdown-based**: Easy to edit and version
- **Searchable**: Full-text search
- **Linked**: Internal linking and cross-references
- **Publishable**: Deploy to GitHub Pages

### 4. Interactive Webapps

Embedded operational tools:

- **Task Manager**: View and manage tasks across projects
- **Budget Calculator**: Financial planning tools
- **Stakeholder Map**: Relationship visualization
- **Timeline Planner**: Project timeline visualization

### 5. Federation Support

Upstream/downstream repository relationships:

- **Template Inheritance**: Fork templates, sync updates
- **Shared Infrastructure**: Common packages and patterns
- **Independent Customization**: Maintain organization-specific changes
- **Bidirectional Flow**: Contribute improvements back

## Architecture

### Two-Repository Model

**organizational-os-framework**:
- Standards documentation
- Schema definitions
- Patterns and best practices
- Case studies

**organizational-os-template**:
- Implementable template
- Operational packages
- Setup scripts
- Generation tools

### Package System

Modular packages that can be enabled/disabled:

```
organizational-os-template/
├── packages/
│   ├── core/              # Base system
│   ├── knowledge-base/    # Quartz docs
│   ├── operations/        # Operational packages
│   │   ├── meetings/
│   │   ├── projects/
│   │   ├── finances/
│   │   └── coordination/
│   ├── webapps/           # Interactive tools
│   ├── web3/              # Optional Web3 features
│   └── integrations/      # External integrations
```

## Who Is It For?

### Target Organizations

- **DAOs**: Decentralized autonomous organizations
- **Cooperatives**: Member-owned organizations
- **Nonprofits**: Mission-driven organizations
- **Projects**: Open source projects and initiatives
- **Foundations**: Grant-making organizations

### Use Cases

1. **New Organization**: Start with template, configure identity
2. **Existing Organization**: Migrate operations to GitHub-based workspace
3. **Multi-Org Coordination**: Coordinate across multiple organizations
4. **Standards Compliance**: Publish EIP-4824 compliant identity
5. **Tool Integration**: Integrate with DAO tooling ecosystem

## How It Works

### Setup Flow

1. **Fork Template**: Fork `organizational-os-template`
2. **Run Setup**: Interactive setup script configures organization
3. **Select Packages**: Choose which operational packages to enable
4. **Generate Schemas**: Automated EIP-4824 schema generation
5. **Deploy**: Deploy to GitHub Pages or custom domain
6. **Operate**: Use operational packages for day-to-day work

### Operational Flow

1. **Create Content**: Add meetings, projects, finances
2. **Generate Schemas**: Automated schema generation from data
3. **Publish**: Schemas published at `.well-known/` URIs
4. **Interoperate**: DAO tooling reads schemas
5. **Sync**: Pull updates from template upstream

## Benefits

### For Organizations

- **Unified Workspace**: Everything in one GitHub repository
- **Standard Identity**: EIP-4824 compliant, interoperable
- **Open Source**: No vendor lock-in, full control
- **Version Controlled**: All changes tracked in Git
- **Extensible**: Add custom packages and integrations

### For Ecosystem

- **Interoperability**: Standard schemas enable tool integration
- **Discoverability**: Organizations discoverable via DAOstar
- **Standards**: Contributes to DAO standards ecosystem
- **Innovation**: Open platform for new tools and patterns

## Comparison with Alternatives

### vs. Traditional Tools

| Feature | Organizational OS | Notion/Airtable | Custom Solutions |
|---------|------------------|-----------------|------------------|
| **Open Source** | ✅ Yes | ❌ No | ✅ Yes |
| **Version Control** | ✅ Git | ❌ No | ✅ Git |
| **Standards Compliant** | ✅ EIP-4824 | ❌ No | ⚠️ Maybe |
| **DAO Interoperable** | ✅ Yes | ❌ No | ⚠️ Maybe |
| **Portable** | ✅ Yes | ❌ No | ⚠️ Maybe |
| **Cost** | ✅ Free | ❌ Paid | ⚠️ Varies |

### vs. Monorepo Approach

- **Independence**: Organizations maintain separate repos
- **Customization**: Each org can customize independently
- **Federation**: Upstream/downstream relationships
- **Scalability**: Better for distributed organizations

## Getting Started

1. **Read Framework**: Understand standards and patterns
2. **Fork Template**: Get implementation template
3. **Run Setup**: Configure for your organization
4. **Start Operating**: Use operational packages
5. **Extend**: Add custom packages as needed

## Next Steps

- **Read Standards**: [`../02-standards/eip-4824-integration.md`](../02-standards/eip-4824-integration.md)
- **Explore Patterns**: [`../03-operational-patterns/`](../03-operational-patterns/)
- **See Examples**: [`../06-case-studies/`](../06-case-studies/)
- **Get Template**: [`../../organizational-os-template/`](../../organizational-os-template/)
