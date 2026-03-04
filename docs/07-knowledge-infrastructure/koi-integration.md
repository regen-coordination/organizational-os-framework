# KOI Integration

**Knowledge Organization Infrastructure for Organizational OS**

## Overview

Knowledge Organization Infrastructure (KOI) provides the foundational knowledge management layer for Organizational OS, enabling:

- **Federated Knowledge**: Organizations maintain control while sharing knowledge with trusted partners
- **AI Context**: Automatic context generation for AI assistants and LLMs
- **Cross-Org Search**: Search knowledge across organizational boundaries with privacy controls
- **Real-time Sync**: Knowledge updates propagate in real-time through the network
- **Automatic Schemas**: EIP-4824 schemas generated automatically from operational data

## Why KOI?

### The Problem

Organizations using Organizational OS face several knowledge management challenges:

1. **Fragmentation**: Knowledge scattered across meetings, projects, documents
2. **Discovery**: Finding relevant information requires manual search
3. **Context Loss**: Historical decisions and rationale get buried
4. **Silos**: Each organization operates in isolation
5. **AI Limitations**: Generic AI lacks organizational context

### The Solution

KOI transforms your Organizational OS repository into a living knowledge base:

- **Automatic Indexing**: All organizational data automatically indexed with RIDs
- **Semantic Search**: Find knowledge by meaning, not just keywords
- **Knowledge Graphs**: Visualize relationships between meetings, projects, decisions
- **AI Context**: Feed organizational knowledge to AI assistants
- **Federation**: Share knowledge with partner organizations while maintaining privacy

## Architecture

### System Overview

```
┌─────────────────────────────────────────┐
│  Organizational OS Repository           │
│  ├── meetings/          [monitored]     │
│  ├── projects/          [monitored]     │
│  ├── data/finances.yaml [monitored]     │
│  └── .well-known/       [auto-updated]  │
└────────────┬────────────────────────────┘
             │ File Events
┌────────────▼────────────────────────────┐
│  KOI Sensors (TypeScript/Python)        │
│  - Detect changes                        │
│  - Generate RIDs                         │
│  - Broadcast events                      │
└────────────┬────────────────────────────┘
             │ KOI-net Protocol
┌────────────▼────────────────────────────┐
│  KOI Processors (Python)                 │
│  ├── Graph (Neo4j): Relationships       │
│  ├── Vector (ChromaDB): Semantic search │
│  └── Schema: EIP-4824 generation        │
└────────────┬────────────────────────────┘
             │ Query API
┌────────────▼────────────────────────────┐
│  Applications                            │
│  - Search interface                      │
│  - Knowledge graphs                      │
│  - AI assistants                         │
└─────────────────────────────────────────┘
```

### Data Flow

**Knowledge Ingestion**:
```
File Created → Sensor Detects → Generate RID → Broadcast Event →
Processors Store → Knowledge Searchable
```

**Knowledge Query**:
```
Application Request → KnowledgeAPI → HTTP to Coordinator →
Processor Query → Results Returned
```

## Integration Points

### 1. Automatic Knowledge Sensing

KOI sensors monitor your organizational data:

**Meetings** (`meetings/**/*.md`):
- RID Format: `rid:orgos:meeting:YYYYMMDD-title-slug`
- Indexed: Title, date, participants, decisions, content
- Searchable: Yes
- Privacy: Federated

**Projects** (`projects/**/*.md`):
- RID Format: `rid:orgos:project:slug`
- Indexed: Name, status, lead, members, milestones
- Searchable: Yes
- Privacy: Public

**Finances** (`data/finances.yaml`):
- RID Format: `rid:orgos:finance:type-date-hash`
- Indexed: Type, amount, date, category
- Searchable: Limited
- Privacy: Private

**Schemas** (`.well-known/**/*.json`):
- RID Format: `rid:orgos:schema:type`
- Auto-updated by schema processor
- Privacy: Public

### 2. Knowledge Processing

Processor nodes transform raw data into structured knowledge:

**Graph Processor**:
- Stores relationships in Neo4j
- Enables graph queries
- Finds related knowledge
- Tracks provenance

**Vector Processor**:
- Generates embeddings
- Enables semantic search
- Similarity queries
- Context preparation

**Schema Processor**:
- Generates EIP-4824 schemas
- Updates `.well-known/` files
- Maintains versions
- Real-time updates

### 3. Knowledge APIs

Applications can query organizational knowledge:

```typescript
import { KoiNetClient } from '@koi-net-integration/koi-bridge';
import { KnowledgeAPI } from '@koi-net-integration/knowledge-api';

const client = new KoiNetClient({
  coordinatorUrl: 'http://localhost:8000/koi-net',
});
const api = new KnowledgeAPI(client);

// Search knowledge
const results = await api.search('treasury management');

// Get organization graph
const graph = await api.getOrganizationGraph('your-org');

// Generate AI context
const context = await api.generateAIContext('project planning', {
  maxDocuments: 20,
  includeTypes: ['rid:orgos:meeting', 'rid:orgos:project'],
});
```

## Setup Guide

### Prerequisites

- Organizational OS Template set up
- Docker & Docker Compose installed
- Node.js 18+

### Quick Start

```bash
# 1. Run setup wizard
npm run koi:setup

# 2. Start KOI network (in koi-net-integration)
cd ../../koi-net-integration/services
docker-compose up -d
cd -

# 3. Start sensors
npm run koi:start

# 4. Test search
npm run koi:search "test query"
```

### Detailed Setup

See [KOI Integration Setup Guide](../../../koi-net-integration/docs/org-os-integration.md)

## Features

### Semantic Search

Search by meaning, not just keywords:

```typescript
// Finds meetings about treasury even if they don't contain the word "treasury"
const results = await api.search('financial management');
```

### Knowledge Graphs

Visualize relationships:

```typescript
const projectKnowledge = await api.getProjectKnowledge('dao-os-integration');

// Returns:
// - Project node
// - Related meetings
// - Associated tasks
// - Decisions made
// - Contributing members
```

### AI Context Generation

Generate context for LLMs:

```typescript
const context = await api.generateAIContext('quarterly planning');

// Returns:
// - Relevant knowledge sources
// - Summaries
// - Citations
// - Formatted for LLM consumption
```

### Automatic Schema Updates

EIP-4824 schemas update automatically:

```
New meeting file → Sensor detects → Schema processor →
.well-known/meetings.json updated
```

## Federation

### Sharing Knowledge

Control what you share:

```yaml
# koi-config.yaml
privacy_mode: federated

sharing:
  public:
    - rid:orgos:project
    - rid:orgos:schema
  
  federated:
    - rid:orgos:meeting
    - rid:orgos:decision
  
  private:
    - rid:orgos:finance
    - rid:orgos:member
```

### Querying Federated Knowledge

```typescript
// Search across your federation
const federatedResults = await api.search('regenerative finance', {
  filters: {
    // No org filter = searches all accessible knowledge
  }
});

// Search only your organization
const localResults = await api.search('budget', {
  filters: {
    organization: 'rid:eip4824:org:your-org'
  }
});
```

## Use Cases

### 1. Onboarding New Members

New members can search organizational history:

```typescript
// "What decisions were made about treasury?"
const results = await api.search('treasury decisions', {
  includeTypes: ['rid:orgos:decision', 'rid:orgos:meeting']
});
```

### 2. Project Context

Get full context for a project:

```typescript
const knowledge = await api.getProjectKnowledge('sustainability-initiative');

// Shows:
// - All meetings discussing the project
// - Decisions made
// - Tasks and milestones
// - Contributing members
```

### 3. AI-Assisted Decision Making

Generate context for AI to help with decisions:

```typescript
const context = await api.generateAIContext('grant allocation strategy', {
  maxDocuments: 30,
});

// Feed to AI:
// "Based on this context, suggest grant allocation priorities..."
```

### 4. Cross-Organization Learning

Learn from partner organizations:

```typescript
// Search across federated organizations
const insights = await api.search('governance frameworks', {
  filters: {
    // Searches your org + trusted partners
  }
});
```

## Best Practices

1. **Consistent Frontmatter**: Use standard fields in meeting/project files
2. **Privacy Settings**: Set appropriate privacy levels
3. **Regular Backups**: Backup Neo4j and ChromaDB
4. **Monitor Sensors**: Check logs regularly
5. **Update Schemas**: Ensure sensors are running
6. **Federation**: Only share what's appropriate

## Security & Privacy

### Data Sovereignty

- Organizations own and control their data
- RIDs point to data, don't contain it
- Consent required for sharing
- Can revoke access anytime

### Privacy Levels

- **Public**: Anyone can query
- **Federated**: Member organizations only
- **Private**: Your organization only

### Encryption

- TLS 1.3 for all communication
- API key authentication
- Audit logging enabled

## Monitoring

### Check Status

```bash
# Sensor status
docker-compose logs -f meeting-sensor

# Processor status
docker-compose logs -f graph-processor

# Knowledge count
curl http://localhost:8000/info
```

### Metrics

- Events processed per second
- Search query latency
- Knowledge object count
- Cache hit rate

## Troubleshooting

See [Organizational OS Integration Guide](../../../koi-net-integration/docs/org-os-integration.md#troubleshooting)

## Next Steps

- [Federation Patterns](./federation-patterns.md) - Multi-org coordination
- [AI Context Generation](./ai-context-generation.md) - LLM integration
- [RID Schema Design](./rid-schema-design.md) - Custom RID types

## References

- [KOI-net Protocol](https://github.com/BlockScience/koi-net)
- [RID Specification](https://github.com/BlockScience/rid-lib)
- [KOI Research Paper](https://blockscience.com/koi)
- [EIP-4824 Standard](https://eips.ethereum.org/EIPS/eip-4824)
