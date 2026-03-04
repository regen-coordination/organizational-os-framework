# Skill: contract-interaction

**Interface with smart contracts relevant to organizational operations**

## Purpose

Enables agents to read from and propose interactions with smart contracts used by the organization — governance contracts (Gardens, Snapshot), funding contracts (Octant, Impact Stake, Artisan), identity contracts (EIP-4824), and role contracts (Hats Protocol).

## When to Use

- When checking governance proposal status in Gardens or Snapshot
- When reviewing Octant vault strategy or distribution status
- When checking Hats Protocol role assignments
- When preparing an on-chain registration for EIP-4824
- When checking on-chain balances or streaming rates (Superfluid)

## Safety Model

Like `capital-flow`, this skill **reads freely but proposes, never executes**:

| Action | Mode |
|--------|------|
| Read contract state | Autonomous |
| Check balances, proposals, roles | Autonomous |
| Prepare transaction data | Autonomous → present for approval |
| Sign or execute transactions | Requires explicit human action |

## Supported Protocols

### Gardens (Colony)
- Read DAO proposals and their status
- Check garden stake amounts
- Read governance parameters
- Prepare proposal transaction data

### Snapshot
- Read current proposals
- Check voting power for addresses
- Summarize active votes with deadlines
- Alert on active votes in `HEARTBEAT.md`

### Octant
- Read vault strategies and yields
- Check epoch status and funding distribution
- Read project allocation status
- Generate status reports for grant accountability

### Superfluid
- Read active streams
- Check campaign participation
- Monitor flow rates
- Alert when season ends / campaign changes

### Hats Protocol
- Read hat tree structure for the organization
- Check who wears which hat
- Verify role assignments match `data/members.yaml`
- Prepare hat minting/transfer transactions

### EIP-4824
- Read on-chain registration status
- Prepare daoURI update transactions
- Validate that `.well-known/dao.json` matches on-chain registration

### Impact Stake
- Read stake status and yield
- Check allocation parameters
- Monitor vault performance

## Configuration

Declare supported contracts in `IDENTITY.md`:

```markdown
## Contracts
- **Gardens DAO:** 0xGARDENS... (Gnosis Chain)
- **Hats Tree:** 12345
- **Octant vault:** 0xOCTANT... (Ethereum mainnet)
- **Superfluid:** active campaigns see data/superfluid-campaigns.yaml
- **EIP-4824 registration:** 0xREG... (Ethereum mainnet)
```

## Read Workflow

When asked about on-chain state:

1. Identify which contract/protocol is relevant
2. Read addresses from `IDENTITY.md` and `TOOLS.md`
3. Query via public RPC or API endpoint configured in `TOOLS.md`
4. Format response in human-readable markdown
5. Log significant findings to `memory/YYYY-MM-DD.md`

## Alert Conditions

Add to `HEARTBEAT.md` automatically when:
- Snapshot vote is active and deadline < 48 hours
- Gardens proposal needs quorum — deadline approaching
- Octant epoch ending — distribution window opening
- Superfluid campaign season ending soon
- Hats role assigned to address not in `data/members.yaml`

## RPC Configuration

```markdown
## RPC Endpoints (in TOOLS.md)
- Gnosis Chain: https://rpc.gnosis.gateway.fm
- Ethereum mainnet: https://eth.llamarpc.com
- Optimism: https://mainnet.optimism.io
```

## Integration

- **capital-flow** — Contract reads feed into capital flow decisions
- **funding-scout** — Reads on-chain funding distribution status
- **schema-generator** — On-chain state updates trigger schema regen
- **dao-os safe-treasury** — Deep Safe integration for multi-sig
- **dao-os hats-governance** — Deep Hats Protocol integration

## Limitations

- This skill reads public contract state. Private state or events may require indexing.
- Transaction preparation requires correct ABI — use references in `skills/contract-interaction/references/abis/`
- Never hardcode private keys or mnemonics. Use hardware wallet or Safe for execution.
