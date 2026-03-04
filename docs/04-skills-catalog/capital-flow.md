# Skill: capital-flow

**Orchestrate capital movements and queue on-chain transactions**

## Purpose

Manages capital flow operations: tracking treasury state, queuing transactions for human approval, reporting on outflows, and coordinating multi-sig operations. This skill is the organizational interface to on-chain financial infrastructure.

## When to Use

- When preparing payouts to contributors
- When reviewing treasury state before a governance proposal
- When coordinating multi-sig transactions (Safe)
- When reporting on fund allocation for grants or accountability
- When processing incoming funds and updating financial records

## Safety First

**This skill operates in strict approval-required mode. It NEVER executes transactions autonomously. It queues, drafts, and presents for human approval.**

Every action involving funds goes through:
1. **Draft** — agent prepares the transaction/report
2. **Present** — shows to operator for review
3. **Confirm** — operator explicitly confirms
4. **Execute** — operator executes (or agent executes with explicit per-action approval)

## Capabilities

### Treasury Monitoring
- Read Safe balance via Safe Transaction Service API
- Read on-chain balances for declared addresses in `IDENTITY.md`
- Summarize current treasury state on request
- Alert on unusual balance changes via `HEARTBEAT.md`

### Transaction Queuing
- Prepare Safe multisig transactions for human review
- Format transaction data in human-readable form
- Track pending transactions and their status
- Alert on transactions awaiting signatures in `HEARTBEAT.md`

### Payout Coordination
- Generate payout lists from `data/members.yaml` and project allocations
- Format for Safe CSV import or manual multisig
- Track payout history in `data/finances.yaml`

### Grant Reporting
- Generate financial summaries from `data/finances.yaml`
- Format budget vs. actual reports
- Prepare grant accountability reports

## Outputs

| Output | Location | Format |
|--------|----------|--------|
| Treasury summary | Response / memory | Markdown |
| Transaction draft | Response for approval | JSON + readable description |
| Payout list | `data/pending-payouts.yaml` | YAML |
| Financial report | `packages/operations/finances/` | Markdown |
| HEARTBEAT alerts | `HEARTBEAT.md` | Checkboxes |

## Treasury State Format

```markdown
## Treasury State — 2026-02-20

### Wallets
| Address | Chain | Token | Balance |
|---------|-------|-------|---------|
| 0xABCD...1234 (ReFi BCN Safe) | Gnosis | XDAI | 1,250 |
| 0xABCD...1234 (ReFi BCN Safe) | Gnosis | GIV | 45,000 |

### Pending Transactions
- [ ] Contributor payout Feb 2026 — 3 signatures, 2 of 3 collected

### Recent Activity (30 days)
- 2026-02-01: Received 500 XDAI from Octant distribution
- 2026-02-10: Sent 150 XDAI to Regenerant Catalunya pilot
```

## Workflow: Contributor Payout

1. Read member allocations from `data/members.yaml`
2. Calculate amounts based on active role weights or fixed allocations
3. Generate draft payout list:
   ```yaml
   payouts:
     - recipient: "0x1234..."
       name: "Contributor Name"
       amount: "50"
       token: "XDAI"
       reason: "Feb 2026 coordination contribution"
   ```
4. **Present to operator** — "Here is the proposed payout. Approve to proceed?"
5. On approval: generate Safe transaction batch or CSV
6. **Operator executes** via Safe UI or hardware wallet
7. Record in `data/finances.yaml`
8. Update `HEARTBEAT.md` (mark payout as completed)

## Configuration

Declare treasury addresses in `IDENTITY.md`:

```markdown
## Treasury
- **Primary Safe:** 0xABCD... (Gnosis Chain)
- **Operational wallet:** 0x1234... (Ethereum mainnet)
- **Impact Stake vault:** 0x5678... (Ethereum mainnet)
```

Configure Safe API in `TOOLS.md`:

```markdown
## Safe Config
- API: https://safe-transaction-gnosis.gateway.gnosis.io
- Chain ID: 100 (Gnosis Chain)
- Safe addresses: see IDENTITY.md
```

## Integration

- **schema-generator** — Financial data updates trigger schema regen
- **funding-scout** — Received grants trigger accounting entries
- **heartbeat-monitor** — Pending transactions and payment deadlines monitored

## Related (dao-os)

For organizations using `dao-os`:
- The **safe-treasury** skill provides deeper Safe SDK integration
- The **hats-governance** skill manages role-based access to treasury actions
- The **composer app** can visualize treasury flows

See [dao-os documentation](../../../dao-os/README.md) for DAO-specific capital flow capabilities.
