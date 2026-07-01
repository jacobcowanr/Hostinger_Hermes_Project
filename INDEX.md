# Hermes Master Index

**Author:** Jacob Cowan  
**Repository:** [jacobcowanr/VPS_Hermes_Project](https://github.com/jacobcowanr/VPS_Hermes_Project) (private)  
**Live home:** `/opt/data/` · **Git backup:** `/opt/data/Projects/VPS_Hermes_Project/`  
**Last Updated:** June 20, 2026 (v0.17.0 live, provider order fix, port corrections, desktop gateway)
**Project:** `VPS_Hermes_Project` (git repo, docs, and filesystem folder)

> **Start here** when you need to find anything — docs, paths, commands, or who owns what layer.

---


---

## Why This Hermes Stands Out

```mermaid
flowchart LR
    A["Persistent memory
agentmemory"] --> E["Elite Agent"]
    B["Institutional RAG
Chroma 27+23 chunks"] --> E
    C["Self-operating crons
8 jobs"] --> E
    D["Documented paths
+ skills"] --> E

    style E fill:#2d1b4e,color:#fff
```

| vs typical AI | Jacob's Hermes |
|---------------|----------------|
| Forgets each session | agentmemory persists facts |
| Hallucinates setup | Chroma cites indexed docs |
| Manual health checks | stack_status + Uptime Kuma + Beszel + Netdata |
| "Run docker ps for me" | Operates via canonical paths + snapshot |

Full narrative: [README.md](README.md) · Per-app depth: [APPLICATIONS.md](APPLICATIONS.md)

---

## June 19, 2026 — Day Timeline

Everything shipped or documented on this day, in order.

```mermaid
timeline
    title June 19, 2026 (VPS = UTC · Mac = CDT)
    section Morning
        SSH fix : Mac ~/.ssh/config → <VPS_TAILSCALE_IP>
        Nord policy : Disconnect before VPS admin
        Egress audit : Plain Hostinger egress documented
    section Afternoon
        n8n live : n8n-eywu-n8n-1 :32771
        Netdata verified : <VPS_TAILSCALE_IP>:19999 only
        Doc audit : 17 .md files aligned to live stack
    section Evening
        Chroma re-index : vps_knowledge=27 python_lessons=23
        3 new crons : improvement · exposure · admin-plane
        Cron schedule : All 3 daily at 11:00 UTC (6am CDT)
        stack_status.sh : Netdata IP + n8n URL fixes
    section Late night
        P1-P4 remediation : orphans killed · MCP +x fix · model swap
        Filesystem cleanup : 22 root files · state-snapshots/ · git d1e9158
        Git repo tidy : removed stale logs · discord · gateway · runtime copies
```

| Time (UTC) | Event | Git / ops |
|------------|-------|-----------|
| Morning | Mac SSH `Could not resolve hostname` fixed | `HostName <VPS_TAILSCALE_IP>` in `~/.ssh/config` |
| Morning | Nord + Tailscale coexistence documented | [SECURITY.md](SECURITY.md) Mac Admin Plane |
| Afternoon | n8n deployed via Hostinger catalog | `n8n-eywu-n8n-1` · `:32771` |
| Afternoon | 9router, Ackee evaluated — not installed | APP_INSTALL_POLICY.md |
| Afternoon | Hermes v0.17.0 upgraded live (in-container) | In-container upgrade; force-recreate reverts — see [HERMES_VPS_SETUP.md](HERMES_VPS_SETUP.md) |
| Evening | Full doc pass (17 files) + Chroma re-index | Commits `ceeb996` … `b2c81fa` |
| Evening | 3 productive crons added | `ebecfa28836e`, `46e149d7f26a`, `2aa32ff8c08e` |
| Evening | New crons scheduled daily **11:00 UTC** = **6:00 AM CDT** | `cron/jobs.json` |
| Late night | P1–P4 remediation: 8 orphan TUI sessions, MCP `+x` root cause, model → `deepseek/deepseek-v4-flash` | [PERFORMANCE.md](PERFORMANCE.md) |
| Late night | `/opt/data/` root cleaned — 23 mandatory files + folders only | `state-snapshots/` · commit `d1e9158` |
| Late night | Git repo deduped — removed stale `logs/`, `discord/`, `gateway/`, `runtime/` under `Projects/` | Live state stays at `/opt/data/` root |

---

## Navigation Map

```mermaid
graph TB
    INDEX["📑 INDEX.md<br/><i>you are here</i>"]

    INDEX --> SETUP["🛠️ Setup & Architecture"]
    INDEX --> OPS["⚙️ Operations"]
    INDEX --> PERF["📊 PERFORMANCE.md"]
    INDEX --> IDENTITY["🧠 Identity & Rules"]
    INDEX --> HISTORY["📜 History"]

    SETUP --> S1["HERMES_VPS_SETUP.md"]
    SETUP --> S2["Architecture.md"]
    SETUP --> S3["SECURITY.md"]
    SETUP --> S4["VPS-Setup.md → redirect"]

    OPS --> O1["Workflow.md"]
    OPS --> O2["BACKUPS.md"]
    OPS --> O3["Aliases.md"]
    OPS --> O4["README.md — hub overview"]

    IDENTITY --> I1["SOUL.md"]
    IDENTITY --> I2["Best-Practices.md"]

    HISTORY --> H1["HERMES_BUILD_RETROSPECTIVE.md"]

    style INDEX fill:#2d1b4e,color:#fff,stroke:#9b59b6
    style SETUP fill:#1a3a2a,color:#fff
    style OPS fill:#1a2a4a,color:#fff
    style PERF fill:#3a2a1a,color:#fff
    style IDENTITY fill:#4a2800,color:#fff
    style HISTORY fill:#1a3a3a,color:#fff
```

---

## Find by Task

| I want to… | Read this |
|------------|-----------|
| Understand the full system design | [Architecture.md](Architecture.md) |
| Set up or recover the VPS container | [HERMES_VPS_SETUP.md](HERMES_VPS_SETUP.md) |
| Run daily commands / pick a tier | [Workflow.md](Workflow.md) |
| Back up or restore config | [BACKUPS.md](BACKUPS.md) |
| Check costs, speed, resource usage | [PERFORMANCE.md](PERFORMANCE.md) |
| Supporting apps (agentmemory, Chroma, Beszel) | [APPLICATIONS.md](APPLICATIONS.md) |
| Dev tools & Python packages | [DEVELOPMENT.md](DEVELOPMENT.md) |
| Review security posture / egress / VPN | [SECURITY.md](SECURITY.md) (Egress & VPN Posture section) |
| Harden admin UIs (Tailscale-only ports) | [SECURITY.md](SECURITY.md) (Planned Hardening section) |
| Upgrade Hermes to v0.17.0 | [HERMES_VPS_SETUP.md](HERMES_VPS_SETUP.md) (Version & Upgrade) |
| Evaluate new Hostinger catalog apps | [APPLICATIONS.md](APPLICATIONS.md) (Candidate Applications) |
| Install new apps / services | APP_INSTALL_POLICY.md |
| **Full reconfigure Hermes (Discord)** | HERMES_FULL_CONFIGURE_PROMPT.md |
| See shell shortcuts (Mac + VPS) | Aliases.md |
| Understand agent personality & rules | SOUL.md |
| Prompting & tier-selection philosophy | [Best-Practices.md](Best-Practices.md) |
| See how we got here (build timeline) | [HERMES_BUILD_RETROSPECTIVE.md](HERMES_BUILD_RETROSPECTIVE.md) |
| Quick hub + config snapshot | [README.md](README.md) |

---

## Access Matrix

```mermaid
flowchart LR
    subgraph MAC["🖥️ Mac"]
        M1["hermes cmd"]
        M2["tutor"]
        M3["ssh vps"]
        M4["localhost:4861<br/>filebrowser"]
    end

    subgraph NET["🔒 Tailscale"]
        TS["vps-hermes-project:2222"]
    end

    subgraph VPS["☁️ VPS Host"]
        BASH["~/.bashrc<br/>hermes() · projects()"]
    end

    subgraph CTR["🐳 Container /opt/data"]
        GW["gateway"]
        DASH[":4860 dashboard"]
        FB[":4861 filebrowser"]
        DISC["Discord bot"]
    end

    M1 --> MAC
    M2 --> TS --> BASH --> CTR
    M3 --> TS --> BASH
    M4 -.->|"LocalForward 4861"| FB
    BASH --> GW
    GW --> DASH
    GW --> DISC

    style MAC fill:#1a2a4a,color:#fff
    style CTR fill:#2a1a4a,color:#fff
    style NET fill:#1a3a3a,color:#fff
```

| Surface | How to reach | Auth |
|---------|--------------|------|
| **Discord** | Always-on via gateway | Bot token in `/opt/data/.env` |
| **Dashboard** | Tailscale → Traefik `:4860` (host `:32787`) | Basic auth in `.env` |
| **Filebrowser** | `ssh -fN vps` then `http://localhost:4861` | Container credentials |
| **Netdata** | Tailscale → `http://<VPS_TAILSCALE_IP>:19999` | Mac app or Chrome "Install as App" |
| **SSH shell** | `ssh vps` → `<VPS_TAILSCALE_IP>:2222` | `id_ed25519_finder` (see Aliases.md) |
| **Hermes CLI** | Mac local or `ssh vps` → `hermes cmd` | API keys in respective `.env` |
| **Desktop app** | Settings → Gateway → Remote gateway → `http://<VPS_TAILSCALE_IP>:32787` | Dashboard basic auth |
| **Git backup** | Inside container at `Projects/VPS_Hermes_Project/` | `.git-credentials` (not committed) |

> **SSH note:** Mac `~/.ssh/config` uses `HostName <VPS_TAILSCALE_IP>` because **Use Tailscale DNS = OFF**. MagicDNS names like `vps-hermes-project` do not resolve on the Mac without Tailscale DNS enabled.

---

## Timezones

| Location | Zone | Notes |
|----------|------|-------|
| **Jacob's Mac** | `America/Chicago` (CDT, UTC−5 in summer) | Wall-clock for daily work |
| **VPS host** | **UTC** | All `date` output, cron schedules, OpenRouter daily cap reset |

**Rule of thumb:** When it's **3:00 PM** in Chicago, the VPS clock reads **8:00 PM UTC**. Schedule Hermes crons and future n8n flows in UTC, or convert before editing `cron/jobs.json`.

---

## Two-Layer File Model

Hermes reads **flat** paths at `/opt/data/`. The git repo is a **backup mirror** — never the live source of truth.

```mermaid
graph LR
    subgraph LIVE["💾 LIVE /opt/data/"]
        direction TB
        A1["config.yaml"]
        A2[".env 🔒"]
        A3["SOUL.md"]
        A4["state.db · kanban.db"]
        A5["hooks/ · cron/ · logs/"]
        A6["state-snapshots/"]
    end

    subgraph BACKUP["📁 BACKUP Projects/VPS_Hermes_Project/"]
        direction TB
        B1["config/config.yaml"]
        B2["docs/*.md"]
        B3["hooks/ · cron/"]
        B4[".gitignore"]
    end

    LIVE -->|"cp before commit"| BACKUP
    BACKUP -->|"cp on restore"| LIVE

    style LIVE fill:#1a4a2e,color:#fff
    style BACKUP fill:#1a2a4a,color:#fff
```

### Path Reference

| Asset | Live (Hermes reads) | Git backup | Committed? |
|-------|---------------------|------------|------------|
| Main config | `/opt/data/config.yaml` | `config/config.yaml` | ✅ |
| Secrets | `/opt/data/.env` | — | 🚫 never |
| Agent identity | `/opt/data/SOUL.md` | `docs/SOUL.md` | ✅ |
| Auth fingerprints | `/opt/data/auth.json` | `config/auth.json` | ✅ |
| Discord channels | `/opt/data/channel_directory.json` | `config/channel_directory.json` | ✅ |
| Cron jobs | `/opt/data/cron/jobs.json` | `cron/jobs.json` | ✅ |
| Startup hooks | `/opt/data/hooks/` | `hooks/` | ✅ |
| Recovery bundles | `/opt/data/state-snapshots/` | — | 🚫 gitignored |
| Runtime state | `gateway.pid`, `state.db`, caches | — | 🚫 gitignored |
| Bundled skills | `/opt/data/skills/` | `skills/` | 🚫 gitignored |

---

## Documentation Catalog

| File | Lines* | Audience | Summary |
|------|--------|----------|---------|
| [README.md](README.md) | ~200 | Everyone | Hub overview, two-layer model, config snapshot |
| [INDEX.md](INDEX.md) | — | Everyone | **This file** — master navigation |
| [PERFORMANCE.md](PERFORMANCE.md) | — | Ops / cost tuning | Resources, provider economics, tuning |
| [Architecture.md](Architecture.md) | ~400 | Design | Dual-tier, provider chain, subsystems |
| [HERMES_VPS_SETUP.md](HERMES_VPS_SETUP.md) | ~350 | Deploy / recover | Docker, Traefik, firewall, file tree |
| [Workflow.md](Workflow.md) | ~300 | Daily use | Tier selection, commands, routines |
| [BACKUPS.md](BACKUPS.md) | ~200 | Ops | Commit/push, restore, what gets tracked |
| [SECURITY.md](SECURITY.md) | ~250 | Security review | Firewall, secrets model, open risks |
| Aliases.md | ~150 | Shell users | Mac `~/.zshrc` + VPS `~/.bashrc` shortcuts |
| [Best-Practices.md](Best-Practices.md) | ~200 | Prompting | Tier framework, project rules |
| SOUL.md | 150 | Agent context | Identity, collaboration style |
| [HERMES_BUILD_RETROSPECTIVE.md](HERMES_BUILD_RETROSPECTIVE.md) | ~300 | History | Plan vs reality, milestones |
| [DEVELOPMENT.md](DEVELOPMENT.md) | — | Dev | Python/Node tooling, venv packages |
| VPS-Setup.md | ~20 | Redirect | Points to HERMES_VPS_SETUP.md |

*\*Approximate — run `wc -l docs/*.md` for live counts.*

---

## Provider Stack (Current)

```mermaid
flowchart TD
    REQ["Incoming request<br/>Discord · CLI · Cron"]
    PRI["① DeepSeek<br/>deepseek-v4-flash<br/>pool credits · primary"]
    FB1["② OpenRouter<br/>gemini-2.5-flash-lite<br/>$1/day cap · resets UTC midnight"]
    FB2["③ Anthropic direct<br/>claude-haiku-4-5-20251001<br/>prepaid credits"]
    FB3["④ Google direct<br/>gemini-2.5-flash-lite<br/>free-tier quotas"]

    REQ --> PRI
    PRI -->|pool empty / error| FB1
    FB1 -->|$1 cap / 401 / fail| FB2
    FB2 -->|error| FB3

    style PRI fill:#1a4a2e,color:#fff
    style FB1 fill:#3a2a1a,color:#fff
    style FB2 fill:#4a1a1a,color:#fff
    style FB3 fill:#1a2a4a,color:#fff
```

> DeepSeek is primary. OpenRouter is fallback #1 ($1/day cap). When DeepSeek pool runs low, fund or rotate — see [PERFORMANCE.md](PERFORMANCE.md).

---

## Command Cheat Sheet

```bash
# ── Mac ──────────────────────────────────────────
hermes "prompt"              # local CLI
tutor                        # Python course via VPS
ssh vps                      # Tailscale shell
ssh -fN vps                  # background tunnel → filebrowser :4861

# ── VPS host ─────────────────────────────────────
hermes gateway status        # routes to container
hermes chat                  # interactive session
projects                     # ls /opt/data/Projects

# ── Inside container ─────────────────────────────
C=$(docker ps -qf "ancestor=ghcr.io/hostinger/hvps-hermes-agent:latest" | head -1)
docker exec -u hermes -it $C bash
cd /opt/data
hermes gateway restart
hermes cron list

# ── Git backup ───────────────────────────────────
cd /opt/data/Projects/VPS_Hermes_Project
git status && git log -3 --oneline
```

---

## Repo Layout (Git Backup Only)

```
Projects/VPS_Hermes_Project/
├── docs/           ← all .md documentation (this folder)
├── config/         ← backup copies of live config files
├── hooks/          ← startup hook source
├── cron/           ← job definitions
├── backups/        ← timestamped snapshots (gitignored)
├── gateway/        ← legacy nested state (not live path)
├── discord/        ← thread state (gitignored at runtime)
├── logs/           ← log copies (gitignored)
├── memories/       ← USER.md gitignored
├── runtime/        ← history files (gitignored)
└── skills/         ← bundled library (gitignored)
```

---

## Maintenance Checklist

| Frequency | Action | Doc |
|-----------|--------|-----|
| Daily | `hermes gateway status` | [Workflow.md](Workflow.md) |
| After config edit | `cp` live → backup, commit, push | [BACKUPS.md](BACKUPS.md) |
| Weekly | Review [PERFORMANCE.md](PERFORMANCE.md) usage | [PERFORMANCE.md](PERFORMANCE.md) |
| After key rotation | Update `/opt/data/.env`, restart gateway | [SECURITY.md](SECURITY.md) |
| Before wipe/restore | Snapshot to `state-snapshots/` | [BACKUPS.md](BACKUPS.md) |

---

*See also: [README.md](README.md) · [PERFORMANCE.md](PERFORMANCE.md) · [HERMES_VPS_SETUP.md](HERMES_VPS_SETUP.md)*

---


---

## Live Ops Stack

```mermaid
flowchart TB
    subgraph CORE["Core"]
        H["Hermes v0.17.0
(live)"]
        AM["agentmemory
404=OK"]
        CH["Chroma
27+23 chunks
17 source .md files"]
    end

    subgraph OBSERVE["Observability"]
        UK["Uptime Kuma"]
        BZ["Beszel"]
        ND["Netdata
<VPS_TAILSCALE_IP>:19999"]
    end

    subgraph AUTO["Automation"]
        N8N["n8n
:32771"]
        CRON["8 Hermes crons
+ host snapshots"]
    end

    SS["stack_status.sh"] -.-> H
    H --> AM & CH
    H -.-> UK & BZ & ND
    H -.-> CRON
    N8N -.->|"separate Docker net"| H

    style H fill:#2d1b4e,color:#fff
    style CH fill:#1a2a4a,color:#fff
    style AUTO fill:#3a2a1a,color:#fff
```

## Cron & Snapshot Automation

```mermaid
flowchart TB
    subgraph HOST["VPS host crontab (jacob)"]
        H1["*/15 docker_ps.snapshot"]
        H2["*/30 tailscale status snapshot"]
        H3["*/5 fix-hermes-perms.sh"]
        H4["06:00 security_watchdog.sh"]
    end

    subgraph HERMES_CRON["Hermes cron/jobs.json — 8 jobs"]
        C1["Sun 04:00 chroma-reindex"]
        C2["Sun 05:00 stack-familiarization"]
        C3["Daily 06:30 security-watchdog"]
        C4["Daily 09:00 python-course-scan"]
        C5["Mon 08:00 weekly-ops-brief"]
        C6["Daily 11:00 improvement-pulse"]
        C7["Daily 11:10 exposure-audit"]
        C8["Daily 11:20 admin-plane-check"]
    end

    subgraph SCRIPTS["Script layers"]
        SRC[".hermes/scripts/ source"]
        SYNC["sync_cron_scripts.sh"]
        RUN["/opt/data/scripts/ runtime"]
    end

    HOST -->|"feeds admin-plane-check"| HERMES_CRON
    SRC --> SYNC --> RUN --> HERMES_CRON

    style HOST fill:#1a3a3a,color:#fff
    style HERMES_CRON fill:#2d1b4e,color:#fff
```

> **6:00 AM CDT block:** `hermes-improvement-pulse`, `exposure-audit`, and `admin-plane-check` all fire at **11:00 UTC** daily. See [Workflow.md](Workflow.md) for the full gantt.

## Admin Plane (Mac → VPS)

```mermaid
flowchart LR
    MAC["Mac
America/Chicago"]
    NORD["NordVPN
disconnect for VPS"]
    TS["Tailscale
<VPS_TAILSCALE_IP>"]
    SSH["ssh vps :2222
id_ed25519_finder"]
    VPS["VPS host UTC"]

    MAC --> NORD
    NORD -.->|"policy: off during admin"| TS
    TS --> SSH --> VPS

    style NORD fill:#4a1a1a,color:#fff
    style TS fill:#1a3a3a,color:#fff
```

| Check | Command |
|-------|---------|
| Stack health | `/opt/data/bin/stack_status.sh` |
| Paths reference | `skills/vps-ops/references/paths-and-endpoints.md` |
| Re-index docs | `/opt/hermes/.venv/bin/python3 /opt/data/bin/chroma_bootstrap.py` |
| Sync cron scripts | `/opt/data/.hermes/scripts/sync_cron_scripts.sh` |

## Canonical Paths

```
HERMES_HOME=/opt/data
Docs:     /opt/data/Projects/VPS_Hermes_Project/docs/
Config:   /opt/data/config.yaml, /opt/data/.env
Skills:   /opt/data/skills/
Cron:     /opt/data/cron/jobs.json
Ops bin:  /opt/data/bin/              # manual: stack_status.sh, chroma_bootstrap.py
Cron src: /opt/data/.hermes/scripts/  # edit → sync_cron_scripts.sh
Cron run: /opt/data/scripts/          # scheduler script: names
Snapshot: /opt/data/docker_ps.snapshot
Ref:      /opt/data/skills/vps-ops/references/paths-and-endpoints.md
Course:   /opt/data/Projects/Python-Fundamentals/
```

**Endpoints (internal Docker DNS):**

| Service | URL | Healthy |
|---------|-----|---------|
| agentmemory | `http://agentmemory-o72l-agentmemory-1:3111/` | HTTP 404 |
| Chroma | `http://chroma:8000/api/v2/heartbeat` | HTTP 200 |
| Beszel | `http://beszel:8090/api/health` | HTTP 200 |
| Uptime Kuma | `http://uptime-kuma-fl0m-uptime-kuma-1:3001/` | HTTP 302 |
| Netdata | `http://netdata:19999/api/v1/info` (internal) · `http://<VPS_TAILSCALE_IP>:19999` (Mac via Tailscale) | HTTP 200 |
| n8n | `http://n8n-eywu-n8n-1:5678/` (not on Hermes network) · `http://<VPS_TAILSCALE_IP>:32771` (Mac) | Hostinger public URL also live |

---

## Hermes Version

| Field | Value |
|-------|-------|
| **Live (container)** | v0.17.0 (in-container upgrade 2026-06-20; Hostinger image = 0.16.0) |
| **Upstream target** | [v0.17.0 (v2026.6.19)](https://github.com/NousResearch/hermes-agent/releases/tag/v2026.6.19) |
| **Upgrade** | `hermes update -y` inside container, or Hostinger image refresh — see [HERMES_VPS_SETUP.md](HERMES_VPS_SETUP.md) |

---

*Last Updated: June 20, 2026 (v0.17.0 live, port/provider corrections, desktop gateway)*
