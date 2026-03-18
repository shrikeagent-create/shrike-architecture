# SHRIKE — System Architecture

> Last updated: March 18, 2026

## System Architecture

### High-Level Overview

```mermaid
graph TB
    subgraph Runtime["🖥️ Mac Mini (Runtime)"]
        OC["🦞 OpenClaw<br/>Orchestrator"]
        MC["📊 Mission Control<br/>Dashboard"]
        OL["🧠 Ollama<br/>Embeddings"]
        Cache["📁 Local Cache<br/>Deliverables"]
    end

    subgraph Sources["Sources of Truth"]
        Notion["📋 Notion<br/>Projects & Tasks"]
        Drive["📁 Google Drive<br/>Files"]
        GH["🔧 GitHub<br/>Architecture & Logic"]
    end

    subgraph Comms["Communication Layer"]
        Discord["💬 Discord<br/>Ops & Updates"]
    end

    subgraph Services["External Services"]
        Gmail["📧 Gmail<br/>4 Inboxes"]
        GCal["📅 Google Calendar"]
        Slides["📊 Google Slides"]
        OnePass["🔐 1Password"]
    end

    OC -->|"projects/tasks"| Notion
    OC -->|"files"| Drive
    OC -->|"commits"| GH
    OC -->|"notifications"| Discord
    OC -->|"email ops"| Gmail
    OC -->|"scheduling"| GCal
    OC -->|"presentations"| Slides
    OC -->|"secrets"| OnePass
    OC -->|"local save"| Cache
    OC -->|"semantic search"| OL

    Notion -.->|"updates"| Discord
    GH -.->|"summaries"| Discord
    Drive -.->|"links"| Notion
    Cache -->|"upload"| Drive

    style OC fill:#e74c3c,color:#fff
    style Notion fill:#000,color:#fff
    style Drive fill:#0f9d58,color:#fff
    style GH fill:#333,color:#fff
    style Discord fill:#5865f2,color:#fff
```

### System Roles

- **Notion** → Execution source. All projects, tasks, events, tracking live here.
- **Google Drive** → File source. Single authority for all documents, decks, images.
- **GitHub** → System logic. Architecture, config history, versioned operating model.
- **Discord** → Ops layer. Real-time notifications, daily briefings, intel delivery. Mirror only.
- **OpenClaw** → Orchestrator. Connects everything. Runs sync, cron, agents, memory.
- **Mac Mini** → Runtime + cache. Processing engine. Local file cache (Drive is authority).

---

## Data Flow

### Project & Task Flow
```mermaid
flowchart LR
    A[New Project] --> B[Notion<br/>Create]
    B --> C{Cross-functional?}
    C -->|No| D[Discord<br/>Notify]
    C -->|Yes| E[Discord + GitHub<br/>if architecture]
    F[Task Update] --> G[Notion<br/>Update]
    G --> H{Significant?}
    H -->|Yes| D
    H -->|No| I[Log only]
    J[Complete] --> K[Notion<br/>Archive] --> D
```

### File Flow
```mermaid
flowchart LR
    A[File Created] --> B[Mac Mini<br/>Local Save]
    B --> C[Google Drive<br/>Upload]
    C --> D[Notion<br/>Link Added]
    D --> E[Discord<br/>Notify]

    style C fill:#0f9d58,color:#fff
```

### Architecture Flow
```mermaid
flowchart LR
    A[System Change] --> B[GitHub<br/>Commit]
    B --> C[Discord<br/>#shrike-log]
```

### Failure & Retry Flow
```mermaid
flowchart TD
    A[Write Attempt] --> B{Success?}
    B -->|Yes| C[Log Write]
    B -->|No| D[Retry Once]
    D --> E{Success?}
    E -->|Yes| C
    E -->|No| F[Queue for<br/>Next Sync]
    F --> G[Log Failure]
    G --> H{Resolved at<br/>Daily Recon?}
    H -->|Yes| C
    H -->|No| I[Notify Marina<br/>via Discord DM]
```

---

## Sync Logic

| Rule | Detail |
|------|--------|
| **Default** | 1 source + 1 mirror per item |
| **Exception** | Multi-system only when functionally required |
| **Primary** | Event-driven (sync at point of write) |
| **Daily** | Full reconciliation at 8 AM briefing |
| **Weekly** | Audit + health score (1-10) on Sunday |
| **Writes** | Trusted at write time, logged always |
| **Verification** | Daily reconciliation only (not per-write) |
| **Conflicts** | Source of truth wins, mirror updates |
| **Version control** | All system changes committed to GitHub |

---

## Integrations & APIs

| Integration | Purpose | Direction | Protocol |
|-------------|---------|-----------|----------|
| **Notion API** | Projects, tasks, databases | Read/Write | REST, key in 1Password |
| **Discord API** | Notifications, briefings, ops | Write (mirror) | Bot via OpenClaw |
| **GitHub API** | Architecture commits, repo management | Read/Write | `gh` CLI, authenticated |
| **Google Drive** | File storage, upload, organize | Read/Write | `gog drive` CLI |
| **Gmail** | Email triage, send (4 inboxes) | Read/Write | `gog gmail` CLI |
| **Google Calendar** | Events, scheduling | Read/Write | `gog calendar` CLI |
| **Google Slides** | Presentations | Read/Write | `gog slides` CLI |
| **Ollama** | Local embeddings for memory search | Local | `nomic-embed-text`, port 11434 |
| **1Password** | Secrets management | Read | `op` CLI |
| **Mission Control** | Dashboard, monitoring | Local | Next.js, port 3100 |

---

## Runtime Details

### Core System
- **Runtime**: OpenClaw gateway on Mac mini
- **Model**: Claude Opus (main) + Sonnet (sub-agents, cron, compaction)
- **Channel**: Discord only (Telegram disabled)
- **Memory**: Semantic search via ollama/nomic-embed-text (18 files, 48 chunks)
- **Heartbeat**: 60 min, active hours 7 AM–11 PM ET

### Cron Schedule (staggered)
| Time | Job | Target |
|------|-----|--------|
| 7:00 AM Mon | Finance & Markets | #finance-markets |
| 7:15 AM Mon+Thu | Wellness Intel | #wellness-intel + email |
| 7:30 AM Tue+Fri | Beauty & Commerce | #beauty-commerce |
| 8:00 AM Daily | Morning Briefing | DMs (to-do + calendar) |
| 8:30 AM Mon | LinkedIn Drafts | #linkedin-drafts |
| 9:00 AM Fri | Functional Fragrance | #wellness-intel |
| 7:00 PM Sun | Portfolio Review | #portfolio |

### Notion Databases
| Database | Purpose |
|----------|---------|
| 🏛️ Apollo Society Projects | Apollo events, marketing, product dev |
| 📊 Project Tracker | L'Oréal + Personal tracks |
| 🎵 Shows & Tickets | Events with dates |
| ✈️ Travel & Plans | Trips |
| 📄 Deliverables Archive | File log with Drive links |
| 💰 Portfolio Tracker | Investment tracking |
| ⚖️ Decision Log | Key decisions |

### Google Drive
```
SHRIKE Deliverables/
├── Apollo/
├── Corporate/
├── Intelligence/
├── Infrastructure/
└── Personal/
```

### Discord Channels
- **Projects**: #apollo-society, #corporate-track, #ventures
- **Operations**: #deliverables, #daily-briefing, #portfolio, #shrike-log, #linkedin-drafts
- **Intelligence**: #finance-markets, #wellness-intel, #beauty-commerce

---

## To-Do

### Active
1. Permanent MC remote access (stable tunnel)
2. Cron prompt compression
3. Memory file structure + write/retrieval rules
4. MC metrics dashboard
5. Model policy finalization
6. Gateway service reinstall
7. Portfolio tracker setup
8. Voice briefings (ElevenLabs)

### Blocked
- Apple Notes import (on Marina's laptop, not Mac mini)
- MC browser login via tunnel (CSP partially fixed, tunnel unstable)

---

## Maintenance
- Update this README on any system change
- Keep aligned with `SYNC.md`
- Commit changes with clear messages
- Review quarterly for unnecessary complexity
