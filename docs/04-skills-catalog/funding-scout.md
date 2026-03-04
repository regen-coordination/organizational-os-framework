# Skill: funding-scout

**Identify, track, and report on funding opportunities**

## Purpose

Actively identifies funding opportunities relevant to the organization's domains, tracks application deadlines, monitors platform activity, and helps coordinate applications across federation peers.

## When to Use

- When asked "what funding opportunities are available?"
- On a scheduled basis (weekly scan)
- When a new funding platform or round is mentioned in conversation
- When reviewing `HEARTBEAT.md` for upcoming funding deadlines
- When preparing for a coordination call that includes funding topics

## Tracked Platforms

The skill monitors known platforms and can be extended via `references/funding-platforms.yaml`:

| Platform | Type | Cadence | Notes |
|----------|------|---------|-------|
| Artisan | Quadratic + matching | Seasonal (2-4 months) | Artifacts = fundraising, votes = signaling |
| Octant | Yield distribution | Quarterly | Long-term, relationship-driven |
| Superfluid campaigns | Streaming rewards | Monthly | Flow-based, start early |
| Gitcoin | Quadratic funding | Seasonal | Large ecosystem reach |
| Impact Stake | Yield staking | Ongoing | 1/3-1/3-1/3 split model |
| Celo PG | Grants | Per round | Ecological/regenerative focus |
| Arbitrum grants | Ecosystem grants | Per cycle | Tech-adjacent |
| Spinach | Automatic daily distribution | Monthly renewal | Passive, low friction |

## Outputs

| Output | Location | Format |
|--------|----------|--------|
| Opportunities list | `data/funding-opportunities.yaml` | YAML |
| Deadline alerts | `HEARTBEAT.md` | Checkboxes |
| Opportunity summary | Response to user / memory | Markdown |
| Commons publication | `knowledge/<domain>/funding-opportunities.yaml` | YAML (via hub sync) |

## Funding Opportunity Format

```yaml
# data/funding-opportunities.yaml
opportunities:
  - id: artisan-regen-finance-s6-001
    platform: artisan
    fund: "Regen Finance Pool"
    season: "Season 6"
    deadline: "2026-04-30"
    domain: "regenerative-finance"
    amount_range: "$500-$5000"
    matching: true
    eligibility: "Active project, web3-accessible"
    action_required: "Create Artisan profile + submit artifacts"
    url: "https://artisan.com/funds/regen-finance"
    status: pending | applied | approved | rejected
    relevant_nodes:
      - "all"
    discovered_by: "refi-bcn"
    discovered_date: "2026-02-20"
    notes: "Fund manager: Swift. Contact via Telegram."
```

## Workflow

### Scan Workflow (Scheduled)

1. Check `data/funding-opportunities.yaml` for upcoming deadlines
2. Query known platforms (via browser tool or platform APIs)
3. Cross-reference with federation hub's aggregated opportunities
4. Add new discoveries to `data/funding-opportunities.yaml`
5. Update `HEARTBEAT.md` with deadlines within 30 days
6. Write summary to `memory/YYYY-MM-DD.md`

### On-Demand Query

When asked about funding:
1. Read `data/funding-opportunities.yaml` for current list
2. Check hub knowledge for network-wide opportunities
3. Return ranked list by deadline and relevance to org domains
4. Suggest which ones match current activity level and capacity

### Application Support

When preparing an application:
1. Load platform-specific template from `references/application-templates.md`
2. Pull org identity from `IDENTITY.md` and `SOUL.md`
3. Pull relevant project data from `data/projects.yaml`
4. Draft application text for human review (never submit autonomously)
5. Create checklist in `HEARTBEAT.md` for submission steps

## Domain Matching

Match opportunities to the organization's declared domains in `federation.yaml`:

```yaml
knowledge-commons:
  shared-domains:
    - "regenerative-finance"
    - "local-governance"
    - "knowledge-commons"
```

Prioritize opportunities that match declared domains.

## Safety

- **Never submit applications autonomously** — always draft + present for approval
- **Never commit funds** without explicit approval
- **Flag ambiguous eligibility** — present for human decision
- **Preserve application deadlines** — add to HEARTBEAT with 7-day pre-alert

## Integration

- **knowledge-curator** — Shares discovered opportunities to commons
- **capital-flow** — Coordinates follow-through on funding received
- **heartbeat-monitor** — Deadline tracking

## References

See `skills/funding-scout/references/`:
- `funding-platforms.yaml` — Full platform database with URLs
- `application-templates.md` — Standard application frameworks
- `eligibility-checklist.md` — Common eligibility requirements
