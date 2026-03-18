# SYNC.md — Unified Operating System

## Architecture

| System | Role | Authority |
|--------|------|-----------|
| **Notion** | Projects & tasks | Source of truth for structured data |
| **Discord** | Comms, ops, daily updates | Mirror + notifications |
| **GitHub** | System infrastructure, versioned logic | Source of truth for architecture |
| **Google Drive** | File storage | Source of truth for files |
| **Mac Mini** | Processing cache | Local copy only, Drive wins on conflict |

## Sources of Truth
- Projects/tasks → **Notion**
- Architecture/system config → **GitHub** (shrikeagent-create/shrike-architecture)
- Files/deliverables → **Google Drive** (SHRIKE Deliverables)
- Real-time ops → **Discord** (mirror only, not authority)

## Sync Rules

### Default: 1 Source + 1 Mirror
- Most items: Notion + Discord notification
- Cross-functional items (e.g., Apollo event touching ops + files + architecture): allowed in multiple systems

### Flow by Event Type
| Event | Source | Mirror | Notify |
|-------|--------|--------|--------|
| New project | Notion | — | Discord |
| Task update | Notion | — | Discord (if significant) |
| Task complete | Notion (archive) | — | Discord log |
| New file | Drive + Mac Mini cache | Notion (link) | Discord |
| Architecture change | GitHub (commit) | — | Discord summary |
| System config | GitHub | — | #shrike-log |

### Sync Timing
- **Event-driven** (primary): Sync at point of write. No delay.
- **Daily reconciliation** (8 AM): Full cross-check during morning briefing
- **Weekly audit** (Sunday): Health score 1-10, drift report

### Naming Convention
- **Files only**: `[project]-[type]-[YYYYMMDD]-[version].[ext]`
  - Example: `apollo-pitch-deck-20260318-v2.pdf`
- **Notion tasks**: Human-readable, no date prefix
- **Discord**: Free-form

### Required Fields (Notion)
- Name
- Status
- Owner
- Links (to Drive files, GitHub, etc.)

## Failure Handling
1. Write fails → retry once immediately
2. Still fails → queue for next sync cycle
3. Log failure with timestamp + context
4. If unresolved after daily reconciliation → notify Marina via Discord DM

## Verification
- **Trust writes** — don't double-check every operation
- **Log every write** — audit trail in memory/sync-log.md
- **Daily reconciliation**: Compare Notion ↔ Discord ↔ Drive for drift
- **Auto-fix to source of truth** — source wins, mirror updates

## Weekly Audit Output
- Changes made since last audit
- Actions taken (syncs, fixes)
- Conflicts detected + resolution
- Risks identified
- Health score (1-10)

## Efficiency Rules
- Batch updates (don't make 10 API calls when 1 batch works)
- Diff-based updates (only sync what changed)
- Minimize API calls (cache Notion schemas, Drive folder IDs)

## Google Drive Structure
```
SHRIKE Deliverables/
├── Apollo/          (1i7oAx7OLzeZdsoql1PXN2wuIhB5CFPeJ)
├── Corporate/       (1mpf8fgzAF3MYEPptkiiGvUwl2b1w-2d-)
├── Intelligence/    (1DXqTY2lYXamGdS57W4Hb_sxApFJQXHBs)
├── Infrastructure/  (10as4L6JU97qbK6Ea6P1UUil-fEKlKCvJ)
└── Personal/        (1lza0T23uo2AYViqI1raObs-LB8dWfDtC)
```

Root folder: `1wYSJu7whalPA1cmhVpmpzOZQlMsyCB71`
Account: `iamsukhova@gmail.com`

## Critical Thinking Mandate
Continuously challenge this system:
- Is this sync necessary?
- Can this be simpler?
- Is this over-engineered for current volume?
- What would break if we removed this?
