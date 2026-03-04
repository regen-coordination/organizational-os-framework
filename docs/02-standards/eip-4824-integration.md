# EIP-4824 Integration Specification

**How Organizational OS implements and extends EIP-4824 for operational workspaces**

## Overview

EIP-4824 (Common Interfaces for DAOs) provides a standardized way for organizations to publish metadata about their identity, membership, governance, and activities. Organizational OS builds on this standard to enable GitHub-based operational workspaces that are interoperable with the broader DAO ecosystem.

## Core Standard Compliance

### Required EIP-4824 Fields

Every organization using Organizational OS MUST implement these core EIP-4824 fields:

```json
{
  "@context": "http://www.daostar.org/schemas",
  "type": "Organization",  // or "DAO", "Cooperative", "Project"
  "name": "Organization Name",
  "description": "Organization description",
  "membersURI": "https://org.example.com/.well-known/members.json",
  "proposalsURI": "https://org.example.com/.well-known/proposals.json",
  "activityLogURI": "https://org.example.com/.well-known/activities.json",
  "governanceURI": "https://org.example.com/governance",
  "contractsURI": "https://org.example.com/.well-known/contracts.json"
}
```

### Implementation Strategy

Organizational OS implements EIP-4824 using a **hybrid progressive approach**:

#### Phase 1: Off-Chain (Immediate)

- **Publish schemas** at `.well-known/dao.json` and related URIs
- **Serve via HTTPS** through GitHub Pages or custom deployment
- **Register with DAOstar** off-chain registry (optional)
- **No smart contracts required** - works for any organization

#### Phase 2: On-Chain (Optional)

- **Deploy EIP-4824 registration contract** (see Web3 package)
- **Publish daoURI on-chain** via smart contract
- **Emit DAOURIUpdate events** for indexers
- **Register with DAOstar** on-chain indexer

## Extended Operational Schemas

Organizational OS extends EIP-4824 with operational schemas for day-to-day management:

### Meetings Schema

**URI**: `meetingsURI` (optional extension)

**Purpose**: Track organizational meetings, decisions, and action items

**Schema**: See [`../../schemas/meetings.json-ld`](../../schemas/meetings.json-ld)

**Example**:
```json
{
  "@context": "https://www.daostar.org/schemas",
  "type": "MeetingRegistry",
  "meetings": [
    {
      "id": "meeting-20260125-1400",
      "type": "meeting",
      "title": "Weekly Team Sync",
      "date": "2026-01-25T14:00:00Z",
      "participants": ["eip155:1:0x1234abcd"],
      "status": "completed",
      "decisions": [...],
      "actionItems": [...]
    }
  ]
}
```

### Projects Schema

**URI**: `projectsURI` (optional extension)

**Purpose**: Track projects using IDEA framework (Integrate, Develop, Execute, Archive)

**Schema**: See [`../../schemas/projects.json-ld`](../../schemas/projects.json-ld)

**Example**:
```json
{
  "@context": "https://www.daostar.org/schemas",
  "type": "ProjectRegistry",
  "projects": [
    {
      "id": "project-organizational-os",
      "name": "Organizational OS",
      "status": "Develop",
      "lead": "eip155:1:0x1234abcd",
      "members": [...],
      "milestones": [...],
      "tasks": [...]
    }
  ]
}
```

### Finances Schema

**URI**: `financesURI` (optional extension)

**Purpose**: Track budgets, expenses, and revenues

**Schema**: See [`../../schemas/finances.json-ld`](../../schemas/finances.json-ld)

**Example**:
```json
{
  "@context": "https://www.daostar.org/schemas",
  "type": "FinancialRegistry",
  "budgets": [...],
  "expenses": [...],
  "revenues": [...]
}
```

## Complete daoURI Structure

When all operational packages are enabled, the complete `dao.json` structure:

```json
{
  "@context": "http://www.daostar.org/schemas",
  "type": "Organization",
  "name": "Organization Name",
  "description": "Organization description",
  
  // Required EIP-4824 fields
  "membersURI": "https://org.example.com/.well-known/members.json",
  "proposalsURI": "https://org.example.com/.well-known/proposals.json",
  "activityLogURI": "https://org.example.com/.well-known/activities.json",
  "governanceURI": "https://org.example.com/governance",
  "contractsURI": "https://org.example.com/.well-known/contracts.json",
  
  // Optional operational extensions
  "meetingsURI": "https://org.example.com/.well-known/meetings.json",
  "projectsURI": "https://org.example.com/.well-known/projects.json",
  "financesURI": "https://org.example.com/.well-known/finances.json"
}
```

## Schema Generation

### Automated Generation

Organizational OS template includes automated schema generation:

1. **Source Data**: Operational data stored in `data/` directory (YAML files)
2. **Generation Script**: `scripts/generate-all-schemas.mjs`
3. **Output**: JSON-LD schemas in `.well-known/` directory
4. **Automation**: GitHub Actions workflow runs on data changes

### Manual Generation

Schemas can also be generated manually:

```bash
npm run generate:schemas
```

This reads from:
- `data/members.yaml` → `.well-known/members.json`
- `data/projects.yaml` → `.well-known/projects.json`
- `content/meetings/*.md` → `.well-known/meetings.json`
- `data/finances.yaml` → `.well-known/finances.json`

## Validation

### Schema Validation

Validate schemas against framework definitions:

```bash
npm run validate:schemas
```

### Compliance Checklist

- [ ] `dao.json` includes all required EIP-4824 fields
- [ ] All URIs are accessible via HTTPS
- [ ] JSON-LD context is `http://www.daostar.org/schemas`
- [ ] Schemas validate against framework definitions
- [ ] Member IDs use CAIP-10 format or DIDs
- [ ] Dates use ISO-8601 format
- [ ] URIs are absolute (not relative)

## DAOstar Ecosystem Integration

### Registration

**Off-Chain Registration**:
1. Publish `dao.json` at `.well-known/dao.json`
2. Submit to DAOstar registry (optional)
3. Appears in DAOstar directory

**On-Chain Registration**:
1. Deploy EIP-4824 registration contract
2. Call `updateDaoURI()` with your daoURI
3. Emit `DAOURIUpdate` event
4. Indexed by DAOstar on-chain indexer

### Interoperability

Organizations using Organizational OS are:

- **Discoverable** by DAOstar indexers
- **Compatible** with DAO tooling (Snapshot, Tally, etc.)
- **Interoperable** with other EIP-4824 compliant systems
- **Part of** broader DAO standards ecosystem

### Tooling Compatibility

- **Snapshot**: Can read proposals from `proposalsURI`
- **Tally**: Can read governance from `governanceURI`
- **DeepDAO**: Can index organizational data
- **DAOstar Directory**: Can list organization

## Extension Guidelines

### Adding New Schemas

When adding new operational schemas:

1. **Define schema** in `schemas/` directory
2. **Use DAOstar context**: `https://www.daostar.org/schemas`
3. **Follow JSON-LD conventions**: Use `@context`, `@type`, `@id`
4. **Document in framework**: Add to integration guide
5. **Update template**: Add generation script
6. **Consider DAOIP**: Propose as DAO Improvement Proposal if broadly useful

### Schema Versioning

- Use semantic versioning for schemas
- Document breaking changes
- Maintain backward compatibility when possible
- Provide migration guides

## On-Chain Registration (Optional)

### When to Use On-Chain Registration

- Organization wants cryptographic verification
- Integration with on-chain governance tools
- Multi-chain identity requirements
- Compliance with DAO frameworks

### Implementation

See [`../05-web3-integration/on-chain-registration.md`](../05-web3-integration/on-chain-registration.md) for:
- Smart contract deployment
- Registration process
- Multi-chain considerations
- Gas cost estimates

## Best Practices

1. **Keep schemas updated**: Run generation regularly
2. **Validate before publishing**: Use validation tools
3. **Use HTTPS**: All URIs must be accessible
4. **Version your schemas**: Track schema versions
5. **Document customizations**: Note any extensions
6. **Test interoperability**: Verify with DAO tooling
7. **Monitor indexers**: Ensure proper indexing

## Compliance Checklist

- [ ] `dao.json` published at `.well-known/dao.json`
- [ ] All required EIP-4824 fields present
- [ ] URIs are accessible via HTTPS
- [ ] JSON-LD context is correct
- [ ] Schemas validate successfully
- [ ] Member IDs use standard formats
- [ ] Dates use ISO-8601 format
- [ ] Optional schemas documented
- [ ] On-chain registration (if applicable)

## References

- **EIP-4824 Standard**: https://eips.ethereum.org/EIPS/eip-4824
- **DAOstar Website**: https://daostar.org/
- **DAOstar Schemas**: https://www.daostar.org/schemas
- **CAIP-10 Specification**: https://github.com/ChainAgnostic/CAIPs/blob/master/CAIPs/caip-10.md
- **JSON-LD Specification**: https://www.w3.org/TR/json-ld11/

## Related Documentation

- [`../01-foundation/what-is-organizational-os.md`](../01-foundation/what-is-organizational-os.md) - Overview
- [`../04-technical-implementation/package-architecture.md`](../04-technical-implementation/package-architecture.md) - Technical details
- [`../05-web3-integration/on-chain-registration.md`](../05-web3-integration/on-chain-registration.md) - On-chain setup
- [`../../schemas/`](../../schemas/) - All schema definitions
