# Skill: knowledge-curator

**Aggregate, organize, and share knowledge across channels and commons**

## Purpose

Monitors organizational channels (Telegram, GitHub, meeting notes), extracts meaningful knowledge, organizes it by domain, and publishes curated summaries to the knowledge commons. The curator turns the constant flow of shared links, discussions, and ideas into navigable, searchable organizational knowledge.

## When to Use

- When Telegram channels accumulate shared links and resources
- After meetings where domain knowledge was discussed
- When asked "what do we know about [topic]?"
- On a scheduled basis (weekly curation)
- When preparing for a thematic discussion or proposal

## What Gets Curated

### From Telegram Channels
- Shared links with brief context
- Project updates mentioned in chat
- Funding opportunities surfaced in discussion
- Partner organization news and announcements
- Questions and answers that contain useful knowledge

### From Meeting Notes
- Domain expertise shared in discussions
- Project learnings and retrospectives
- Relationship context (who knows what, who works with whom)
- Decision rationale

### From GitHub
- Repository updates and releases from monitored orgs
- Proposals and governance decisions
- Technical developments relevant to the organization

## Outputs

| Output | Location | Format |
|--------|----------|--------|
| Domain knowledge file | `knowledge/<domain>/YYYY-MM-DD-curation.md` | Markdown |
| Commons contribution | Hub sync → `knowledge/<domain>/from-nodes/<node>/` | Markdown |
| Weekly digest | `memory/YYYY-MM-DD.md` + response | Markdown |
| Index update | `MEMORY.md` | Markdown |

## Curation Format

```markdown
# Knowledge Curation — Regenerative Finance
**Period:** 2026-02-13 → 2026-02-20  
**Curated by:** ReFi BCN node  

## Key Developments

### Funding Landscape
- Artisan Season 6 launching with 3 new pools (knowledge commons, waste mgmt, agroforestry)
  Source: Council sync 2026-02-20
- Superfluid campaigns shifting — Season 5 ending, Season 6 details pending
  Source: Telegram #regen-coordination

### Platform/Protocol Updates
- Base leaving OP Stack — implications for public goods funding on Optimism
  Source: [Link from Telegram]
- StreamVote launched for trust-graph governance — potentially relevant for domain pool governance
  Source: Meeting mention

### Partner Updates
- Earth.live fundraising phase — pitch deck + video coming. MOU with Bloom.
  Source: Council sync 2026-02-20
- Benjamin Life bioregional knowledge commons architecture using multi-agent approach
  Source: Meeting mention — follow-up 1:1 scheduled

## Resources
- [[StreamVote documentation]] — [URL]
- [[Earth.live platform]] — [URL]
- [[Artisan Season 6 guide]] — [URL]
```

## Curation Principles

1. **Synthesize, don't copy** — extract insights, not raw text
2. **Source everything** — always note where info came from
3. **Link to originals** — preserve discoverability
4. **Apply org voice** — match terminology from `SOUL.md`
5. **Domain-tag everything** — every item belongs to at least one domain
6. **Distinguish signal from noise** — only curate what has durable value

## Workflow

### Scheduled Curation (Weekly)

1. Read `AGENTS.md` for channel access configuration
2. Pull recent Telegram messages from monitored groups
3. Pull recent GitHub activity from monitored repos
4. Review this week's meeting notes in `packages/operations/meetings/`
5. Cluster by domain using `federation.yaml` `shared-domains`
6. Write domain curation files
7. Write memory entry with digest
8. Trigger hub sync (if configured)

### On-Demand Query

When asked about a topic:
1. Search `knowledge/` directory for existing curation
2. Search `memory/` for relevant session notes
3. Search meeting notes in `packages/operations/meetings/`
4. If insufficient: surface what's known and what's missing
5. Offer to do a fresh curation from channels if needed

## Channel Configuration

Channels to monitor are defined in `TOOLS.md`:

```markdown
## Telegram Channels to Monitor
- @regen_coordination — Council coordination
- @refi_bcn — Local BCN community
- @refi_ecosystem — ReFi DAO ecosystem

## GitHub Orgs to Monitor
- regen-coordination
- greenpill-network
- refi-dao
```

## Privacy Handling

- Only curate from public channels or channels the agent is explicitly authorized to access
- Do not quote individual messages without context — synthesize ideas, not speakers
- Personal names appear only when relevant and public (e.g., "project lead: Name")
- Check `SOUL.md` boundaries section for what the org considers private

## Integration

- **meeting-processor** — Meeting notes feed into curation
- **funding-scout** — Funding mentions get forwarded for tracking
- **federation-sync** — Curations are contributed to knowledge commons hub
