# SHRIKE Architecture

> Last updated: March 17, 2026

## SHRIKE Architecture

### Core System
- **Runtime**: OpenClaw gateway on Mac mini (192.168.1.177)
- **Model**: Claude Opus (main session) + Sonnet (sub-agents)
- **Channels**: Discord (bot: Shrike) + Telegram (shrike999bot) — cross-platform DM sync via dmScope: "main"
- **Config**: /Users/theshrike/.openclaw/openclaw.json
- **Workspace**: /Users/theshrike/.openclaw/workspace/

### Discord Server (Guild: 1478978953665577092)
**🏗️ Projects**: #apollo-society, #corporate-track, #ventures
**📋 Operations**: #deliverables, #daily-briefing, #portfolio, #shrike-log, #linkedin-drafts
**Intelligence Briefings**: #finance-markets, #wellness-intel, #beauty-commerce

### Notion Workspace (SHRIKE HQ)
- 🏛️ Apollo Society Projects (31d1afa8-22e8-81be-b402-ea243e2baafc) — Tags: Events/Marketing/Product Dev
- 📊 Project Tracker (31d1afa8-22e8-816e-b900-dcd6c8a48fe9) — Tracks: L'Oréal/Personal
- 🎵 Shows & Tickets (31d1afa8-22e8-812b-bfa3-e6f60c1b866a)
- ✈️ Travel & Plans (31d1afa8-22e8-81e7-b1f8-dadaa862bc75)
- 📄 Deliverables Archive (31d1afa8-22e8-81be-a825-fda1f0674a0e)
- 💰 Portfolio Tracker (31d1afa8-22e8-8161-89f7-dd6b11e3e6da)
- ⚖️ Decision Log (31d1afa8-22e8-8160-950d-d4db07a5b7ae)

### Cron Jobs (9 active)
| Job | Schedule | Target |
|-----|----------|--------|
| Finance & Markets | Mon 7 AM | #finance-markets |
| Wellness Intel | Mon+Thu 7 AM | #wellness-intel + email |
| Beauty & Commerce | Tue+Fri 7 AM | #beauty-commerce |
| Functional Fragrance | Fri 9 AM | #wellness-intel |
| LinkedIn Drafts | Mon 8 AM | #linkedin-drafts |
| Daily Briefing | Daily 8 AM | DMs (to-do + calendar + email) |
| Portfolio Review | Sun 7 PM | #portfolio |
| AA Credit Reminder | 1st+15th Apr-May | DMs |
| SW Credit Reminder | 1st+15th May-Jun | DMs |

### Email Triage
- 4 inboxes: iamsukhova@gmail.com, sukhova.mrn@gmail.com, marina@apollosociety.nyc, hello@apollosociety.nyc
- Digest: 7 AM daily (launchd)
- Urgent scan: every 2h, 8 AM-10 PM (launchd)
- Quiet hours: 11 PM - 7 AM
- Tool: gog gmail CLI

### Mission Control
- Port 3100, auto-start via launchd
- Remote access: Cloudflare tunnel (temporary quick tunnel)
- DB: SQLite at /Users/theshrike/mission-control/.data/mission-control.db

### Integrations
- Google Workspace (Gmail, Calendar, Slides, Drive) via gog CLI
- Notion API (key in 1Password)
- 1Password CLI (op)
- Cloudflare tunnel (cloudflared)

### Output Routing Rules
- Deliverables → /Users/theshrike/Desktop/Deliverables/ + Discord #deliverables + Notion
- Architecture → GitHub
- Daily data → Notion
- Alerts/urgent → Discord DMs
- Intel briefings → Discord channels (not email, except Wellness)
- LinkedIn drafts → #linkedin-drafts only (never auto-post)

---

## To-Do List

### AI Infrastructure
1. Permanent remote tunnel (Cloudflare named tunnel)
2. Email triage cron verification
3. Auto-routing automation (deliverables → Discord + Notion)
4. Memory search embeddings (semantic recall)
5. GitHub integration (architecture → repo)
6. Telegram outbound (automated messages)
7. MC dashboard customization
8. Portfolio tracker setup
9. Decision log — first entries
10. Voice briefings (ElevenLabs TTS)

### Blocked
- Apple Notes import (not on Mac mini)
- Google Slides for marina@apollosociety.nyc (untested)
- Magic Mouse BT connection (needs physical access)
