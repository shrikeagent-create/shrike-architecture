# 🪶 SHRIKE — System Architecture

> **SHRIKE** is Marina's personal AI infrastructure: a always-on strategic intelligence layer running on a Mac mini, reachable via Telegram and Discord, powered by Claude AI, and connected to every tool Marina uses.

---

## 📐 1. Main System Architecture

```mermaid
graph TB
    classDef marina fill:#e8d5f5,stroke:#9b59b6,stroke-width:2px,color:#000
    classDef gateway fill:#d5e8f5,stroke:#2980b9,stroke-width:3px,color:#000
    classDef ai fill:#d5f5e3,stroke:#27ae60,stroke-width:2px,color:#000
    classDef channel fill:#fef9e7,stroke:#f39c12,stroke-width:2px,color:#000
    classDef tool fill:#fdebd0,stroke:#e67e22,stroke-width:2px,color:#000
    classDef external fill:#f2f3f4,stroke:#7f8c8d,stroke-width:2px,color:#000
    classDef memory fill:#fdedec,stroke:#e74c3c,stroke-width:2px,color:#000
    classDef hardware fill:#d6eaf8,stroke:#1a5276,stroke-width:3px,color:#000

    MARINA["👤 Marina\n(User)"]:::marina

    subgraph MAC["🖥️  Mac Mini — Always On  (192.168.1.171)"]
        direction TB

        subgraph GW["⚡ OpenClaw Gateway  (Port 18789)"]
            ROUTER["🔀 Message Router\n& Orchestrator"]:::gateway
        end

        subgraph BRAIN["🧠 AI Engine"]
            OPUS["🎯 Claude Opus 4.6\nOrchestrator / Strategist"]:::ai
            SONNET["⚙️  Claude Sonnet 4.6\nSub-Agent Workers (up to 8)"]:::ai
        end

        subgraph TOOLS["🛠️  Local Tools"]
            WHISPER["🎤 Whisper + ffmpeg\nVoice Transcription"]:::tool
            GOG["📧 gog CLI\nGoogle Workspace"]:::tool
            GH["🐙 GitHub CLI\nRepo Management"]:::tool
            BRAVE["🔍 Brave Search\nWeb Research"]:::tool
            CHROME["🌐 Chromium\nBrowser Automation"]:::tool
        end

        subgraph MEMORY["💾 Workspace & Memory"]
            SOUL["🪶 SOUL.md\nWho SHRIKE Is"]:::memory
            USER["👤 USER.md\nWho Marina Is"]:::memory
            AGENTS["📋 AGENTS.md\nHow to Operate"]:::memory
            MEMLONG["🧠 MEMORY.md\nLong-Term Memory"]:::memory
            DAILY["📅 Daily Logs\nmemory/YYYY-MM-DD.md"]:::memory
        end
    end

    subgraph CHANNELS["📡 Communication Channels"]
        TG["💬 Telegram\n(Primary)"]:::channel
        DC["🎮 Discord\n(Secondary)"]:::channel
    end

    subgraph EXTERNAL["☁️  External Services"]
        ANTHROPIC["🤖 Anthropic API\nAI Inference"]:::external
        GOOGLE["📊 Google Workspace\n4 Accounts"]:::external
        GITHUB["🐙 GitHub\nRepositories"]:::external
    end

    MARINA <-->|"Messages\n& Voice Notes"| TG
    MARINA <-->|"Messages"| DC
    TG <-->|"Bot API"| ROUTER
    DC <-->|"Bot API"| ROUTER
    ROUTER <-->|"Orchestrates"| OPUS
    OPUS <-->|"Delegates tasks"| SONNET
    OPUS --> WHISPER
    OPUS --> GOG
    OPUS --> GH
    OPUS --> BRAVE
    OPUS --> CHROME
    OPUS <-->|"Reads/Writes"| MEMORY
    SONNET <-->|"Reads/Writes"| MEMORY
    GOG <-->|"OAuth"| GOOGLE
    GH <-->|"Token Auth"| GITHUB
    OPUS <-->|"API Key"| ANTHROPIC
    SONNET <-->|"API Key"| ANTHROPIC
```

---

## 💬 2. Message Flow — Voice Note to Response

> *How a single voice note from Marina becomes a researched, strategic response.*

```mermaid
sequenceDiagram
    actor Marina as 👤 Marina
    participant TG as 💬 Telegram
    participant GW as ⚡ Gateway
    participant WH as 🎤 Whisper
    participant OPUS as 🎯 Opus (Orchestrator)
    participant MEM as 💾 Memory
    participant SONNET as ⚙️ Sonnet (Sub-Agent)
    participant EXT as ☁️ External APIs

    Marina->>TG: Sends voice note 🎙️
    TG->>GW: Delivers audio file via Bot API
    GW->>WH: Passes audio for transcription
    WH-->>GW: Returns transcript text
    GW->>OPUS: Routes transcript + context

    Note over OPUS: Reads SOUL.md, USER.md,<br/>AGENTS.md, today's memory

    OPUS->>MEM: Loads session context
    MEM-->>OPUS: Identity + history

    alt Simple question or strategic decision
        OPUS->>OPUS: Handles directly
    else Research, coding, or heavy task
        OPUS->>SONNET: Spawns sub-agent with task
        SONNET->>EXT: Searches web / fetches data
        EXT-->>SONNET: Returns results
        SONNET->>MEM: Saves working notes
        SONNET-->>OPUS: Returns completed work
    end

    OPUS->>MEM: Saves response to daily log
    OPUS->>GW: Sends final response
    GW->>TG: Delivers message to Marina
    TG-->>Marina: ✅ Response received

    Note over MEM: Deliverables saved to<br/>~/Desktop/Deliverables/
```

---

## 🧠 3. Memory & Identity Architecture

> *How SHRIKE knows who it is, who Marina is, and what happened before.*

```mermaid
graph LR
    classDef identity fill:#e8d5f5,stroke:#9b59b6,stroke-width:2px,color:#000
    classDef memory fill:#fdedec,stroke:#e74c3c,stroke-width:2px,color:#000
    classDef session fill:#d5f5e3,stroke:#27ae60,stroke-width:2px,color:#000
    classDef load fill:#d5e8f5,stroke:#2980b9,stroke-width:2px,color:#000

    subgraph IDENTITY["🪶 Identity Layer — Who is SHRIKE?"]
        SOUL["SOUL.md\n🎯 Optimization engine\nStrategic priorities\nRisk profile"]:::identity
        ID["IDENTITY.md\n🏷️ Codename: SHRIKE\nFunction & vibe"]:::identity
        USR["USER.md\n👤 Marina's full profile\nGoals, constraints,\nwork tracks"]:::identity
        AGT["AGENTS.md\n📋 Operating protocols\nCommunication rules\nExecution frameworks"]:::identity
    end

    subgraph MEMORY_LAYER["💾 Memory Layer — What has happened?"]
        MEMLONG["MEMORY.md\n🧠 Long-term memory\nCurated learnings\nKey decisions"]:::memory
        DAILY["memory/YYYY-MM-DD.md\n📅 Daily session logs\nRaw notes\nWhat happened today"]:::memory
        HEART["HEARTBEAT.md\n💓 Periodic task config\nCheck-in schedule"]:::memory
    end

    subgraph SESSIONS["🔄 Per-Session Loading"]
        MAIN["Main Session\n(Telegram/Discord DM)\n\nLoads ALL files\nFull context + memory"]:::session
        GUILD["Group/Guild Session\n\nLoads identity files\nNO personal memory\n(privacy)"]:::session
        SUB["Sub-Agent Session\n\nLoads task context\nInherits SHRIKE identity"]:::session
    end

    SOUL --> MAIN
    ID --> MAIN
    USR --> MAIN
    AGT --> MAIN
    MEMLONG --> MAIN
    DAILY --> MAIN

    SOUL --> GUILD
    ID --> GUILD
    AGT --> GUILD

    SOUL --> SUB
    AGT --> SUB

    MAIN -->|"Updates after session"| MEMLONG
    MAIN -->|"Writes notes during session"| DAILY
```

---

## 🔌 4. API & Integration Map

> *Every external service SHRIKE connects to — and why.*

```mermaid
graph TB
    classDef shrike fill:#d5e8f5,stroke:#2980b9,stroke-width:3px,color:#000
    classDef api fill:#d5f5e3,stroke:#27ae60,stroke-width:2px,color:#000
    classDef auth fill:#fef9e7,stroke:#f39c12,stroke-width:1px,color:#555,font-size:11px

    SHRIKE["🪶 SHRIKE\nCore System"]:::shrike

    subgraph AI_APIS["🤖 AI Services"]
        ANTH["Anthropic API\n─────────────\n🎯 Opus 4.6 → Orchestrator\n⚙️ Sonnet 4.6 → Workers\n─────────────\nAuth: API Key\nDir: Bidirectional"]:::api
    end

    subgraph COMM_APIS["💬 Communication"]
        TGAPI["Telegram Bot API\n─────────────\n📨 Receive messages\n📤 Send responses\n🎙️ Download voice notes\n─────────────\nAuth: Bot Token\nDir: Bidirectional"]:::api
        DCAPI["Discord Bot API\n─────────────\n📨 Receive messages\n📤 Send responses\n─────────────\nAuth: Bot Token\nDir: Bidirectional"]:::api
    end

    subgraph GOOGLE_APIS["📊 Google Workspace (4 accounts)"]
        GMAIL["Gmail API\n─────────\n📧 Read/send email\nAuth: OAuth2"]:::api
        GCAL["Google Calendar\n─────────\n📅 Check schedule\nCreate events\nAuth: OAuth2"]:::api
        GDRIVE["Google Drive\n─────────\n📁 Access files\nUpload docs\nAuth: OAuth2"]:::api
        GDOCS["Docs + Sheets\n─────────\n📄 Read/write\ndocuments\nAuth: OAuth2"]:::api
    end

    subgraph SEARCH_APIS["🔍 Research"]
        BRAVE_API["Brave Search API\n─────────────\n🌐 Web search\nNews & research\n─────────────\nAuth: API Key\nDir: Outbound"]:::api
    end

    subgraph DEV_APIS["🛠️ Development"]
        GH_API["GitHub API\n─────────────\n🐙 Create repos\nManage code\nOpen PRs\n─────────────\nAuth: Token\nAccount: shrikeagent-create\nDir: Bidirectional"]:::api
    end

    SHRIKE <--> ANTH
    SHRIKE <--> TGAPI
    SHRIKE <--> DCAPI
    SHRIKE --> BRAVE_API
    SHRIKE <--> GH_API
    SHRIKE <--> GMAIL
    SHRIKE <--> GCAL
    SHRIKE <--> GDRIVE
    SHRIKE <--> GDOCS
```

---

## ⚡ 5. Two-Tier Execution Model

> *Opus thinks strategically. Sonnet does the heavy lifting. Together they're more powerful than either alone.*

```mermaid
flowchart TD
    classDef marina fill:#e8d5f5,stroke:#9b59b6,stroke-width:2px,color:#000
    classDef opus fill:#d5f5e3,stroke:#27ae60,stroke-width:3px,color:#000
    classDef sonnet fill:#d5e8f5,stroke:#2980b9,stroke-width:2px,color:#000
    classDef decision fill:#fef9e7,stroke:#f39c12,stroke-width:2px,color:#000
    classDef output fill:#fdedec,stroke:#e74c3c,stroke-width:2px,color:#000

    TASK["📩 Task arrives\n(from Marina via Telegram/Discord)"]:::marina

    OPUS["🎯 CLAUDE OPUS 4.6\n— Orchestrator —\n\n• Reads full context\n• Understands what Marina needs\n• Plans the approach\n• Quality reviews all output"]:::opus

    DECIDE{"What kind\nof task?"}:::decision

    DIRECT["✅ Opus handles directly\n\n• Quick strategic questions\n• Short answers\n• Personal advice\n• Decisions requiring\n  full context"]:::opus

    DELEGATE["🔀 Delegate to Sonnet\n\n• Deep web research\n• Writing long documents\n• Coding & scripts\n• Data analysis\n• Any high-token task"]:::decision

    subgraph WORKERS["⚙️ Sonnet Sub-Agents (up to 8 concurrent)"]
        S1["Sonnet Agent 1\nResearch task"]:::sonnet
        S2["Sonnet Agent 2\nCoding task"]:::sonnet
        S3["Sonnet Agent 3\nDraft writing"]:::sonnet
        SN["... up to 8\nsimultaneous"]:::sonnet
    end

    REVIEW["🎯 Opus reviews\n& synthesizes results\n\n• Quality check\n• Strategic framing\n• Adds context\n• Prepares final response"]:::opus

    OUTPUT["📤 Response delivered\nto Marina"]:::output

    TASK --> OPUS
    OPUS --> DECIDE
    DECIDE -->|"Simple / Strategic"| DIRECT
    DECIDE -->|"Research / Heavy / Long"| DELEGATE
    DELEGATE --> WORKERS
    WORKERS -->|"Auto-announce completion"| REVIEW
    DIRECT --> OUTPUT
    REVIEW --> OUTPUT
```

---

## 📊 6. Component Reference Table

| Component | What It Does | Why It Exists | How It Connects | Status |
|-----------|-------------|---------------|-----------------|--------|
| **Mac Mini (M-series)** | Always-on home server | Runs everything 24/7 without cloud costs | Physical host machine | ✅ Live |
| **OpenClaw Gateway** | Routes messages, manages sessions, controls agent | The brain stem — connects Marina to SHRIKE | Port 18789, loopback | ✅ Live |
| **Claude Opus 4.6** | Orchestrator, strategist, quality reviewer | Best model for complex reasoning and strategy | Anthropic API | ✅ Live |
| **Claude Sonnet 4.6** | Sub-agent worker, research, coding | Fast + cost-efficient for heavy execution tasks | Anthropic API | ✅ Live |
| **Telegram Bot** | Primary messaging channel | Marina's preferred communication app | Telegram Bot API | ✅ Live |
| **Discord Bot** | Secondary messaging channel | Group chat / secondary access point | Discord Bot API | ✅ Live |
| **Whisper + ffmpeg** | Transcribes voice notes to text | Marina can send voice messages naturally | Local CLI tool | ✅ Live |
| **gog CLI** | Gmail, Calendar, Drive, Docs, Sheets | Full Google Workspace control from terminal | Google OAuth (4 accounts) | ✅ Live |
| **GitHub CLI (gh)** | Create repos, manage code, open PRs | Code and document management | GitHub API (token auth) | ✅ Live |
| **Brave Search API** | Web search and research | Real-time web information for research tasks | REST API | ✅ Live |
| **Chromium** | Browser automation | Web scraping, form filling, UI automation | Local browser | ⚠️ Installed, configuring |
| **SOUL.md** | Defines SHRIKE's values, priorities, optimization targets | Ensures consistent strategic alignment | Loaded every session | ✅ Live |
| **USER.md** | Marina's full profile, goals, constraints, work tracks | Personalizes every interaction | Loaded every main session | ✅ Live |
| **AGENTS.md** | Operating protocols, communication rules, frameworks | How SHRIKE makes decisions | Loaded every session | ✅ Live |
| **MEMORY.md** | Long-term curated memory | Remembers decisions, context, lessons across months | Loaded in main sessions only | ✅ Live |
| **Daily memory logs** | Raw session notes | Continuity within and across days | Written after each session | ✅ Live |
| **HEARTBEAT.md** | Periodic task checklist | Proactive check-ins without manual prompts | Polled on heartbeat schedule | ✅ Live |
| **Google Workspace (4 accounts)** | Email, calendar, drive, docs | Marina's full digital work life | gog CLI → OAuth | ✅ Live |
| **GitHub (shrikeagent-create)** | Code repositories | Version control for deliverables and code | GitHub CLI | ✅ Live |
| **Desktop/Deliverables** | Output folder for all documents | Persistent storage for finished work | Local filesystem | ✅ Live |

---

## 🔮 7. Coming Soon — Planned Capabilities

> The infrastructure is designed to grow. These capabilities are next in the roadmap.

```mermaid
graph LR
    classDef live fill:#d5f5e3,stroke:#27ae60,stroke-width:2px,color:#000
    classDef soon fill:#fef9e7,stroke:#f39c12,stroke-width:2px,color:#000
    classDef future fill:#f2f3f4,stroke:#7f8c8d,stroke-width:2px,color:#555

    subgraph NOW["✅ Live Today"]
        L1["Telegram + Discord\nMessaging"]:::live
        L2["Voice Note\nTranscription"]:::live
        L3["Google Workspace\nIntegration"]:::live
        L4["Web Research\n(Brave Search)"]:::live
        L5["GitHub\nManagement"]:::live
        L6["Two-Tier AI\nExecution"]:::live
        L7["Memory &\nIdentity System"]:::live
    end

    subgraph SOON_["🔜 Coming Soon"]
        S1["💓 Heartbeat Monitoring\nProactive email + calendar\ncheck-ins without prompting"]:::soon
        S2["⏰ Cron Jobs\nScheduled tasks at\nexact times (9am reports, etc.)"]:::soon
        S3["🌐 Full Browser Automation\nWeb scraping, form filling,\nautomated browsing"]:::soon
        S4["📸 Instagram / Social Posting\nScheduled content publishing\nacross social platforms"]:::soon
        S5["📧 Wix Email Campaigns\nApollo Society newsletter\nautomation"]:::soon
        S6["📊 Remote Dashboard\nAccess OpenClaw dashboard\nfrom anywhere securely"]:::soon
    end
```

| Capability | Description | Use Case | Timeline |
|------------|-------------|----------|----------|
| **💓 Heartbeat Monitoring** | Proactive email + calendar check-ins on a schedule | "You have a meeting in 30 mins" — without asking | 🔜 Soon |
| **⏰ Cron Jobs** | Scheduled tasks at exact times | Daily 9am briefings, weekly reports | 🔜 Soon |
| **🌐 Full Browser Automation** | Chromium fully configured for web automation | Scraping, form submissions, research | 🔜 Soon |
| **📸 Social Media Posting** | Instagram, LinkedIn content publishing | Apollo Society content distribution | 📅 Planned |
| **📧 Wix Email Campaigns** | Apollo Society newsletter automation | Email marketing without manual work | 📅 Planned |
| **📊 Remote Dashboard** | Secure external access to OpenClaw dashboard | Manage SHRIKE from anywhere | 📅 Planned |

---

## 🏗️ Infrastructure Summary

```
🖥️  MAC MINI (Always On, Home Network)
│
├── ⚡ OpenClaw Gateway (:18789)
│   ├── 💬 Telegram Bot ←→ Marina (primary)
│   ├── 🎮 Discord Bot ←→ Marina (secondary)
│   └── 🔀 Routes every message to the right agent
│
├── 🧠 AI Engine (Anthropic API)
│   ├── 🎯 Opus 4.6 — Orchestrates, reasons, reviews
│   └── ⚙️  Sonnet 4.6 — Executes, researches, codes (×8)
│
├── 🛠️  Tools
│   ├── 🎤 Whisper — Voice → Text
│   ├── 📧 gog CLI → Google (Gmail, Cal, Drive, Docs)
│   ├── 🔍 Brave Search → Web research
│   ├── 🐙 GitHub CLI → Repos & code
│   └── 🌐 Chromium → Browser automation
│
└── 💾 Memory Layer
    ├── 🪶 SOUL.md — Who SHRIKE is
    ├── 👤 USER.md — Who Marina is
    ├── 📋 AGENTS.md — How to operate
    ├── 🧠 MEMORY.md — What was learned
    └── 📅 Daily logs — What happened
```

---

*Document generated: 2026-03-05 | SHRIKE v2026.2.26 | Architecture v1.0*
