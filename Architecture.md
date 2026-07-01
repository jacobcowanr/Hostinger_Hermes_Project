# Hermes VPS Architecture

**Project:** `VPS_Hermes_Project`  

**Author:** Jacob Cowan
**Last Updated:** June 19, 2026 (final pass)
**Status:** Live — path audit verified

> Architecture of a **self-operating AI stack**. Not just containers on a VPS — a deliberate design where memory, knowledge, monitoring, and automation reinforce each other.

---

## Design Philosophy

```mermaid
flowchart TB
    subgraph PRINCIPLES["Design Principles"]
        P1["Separate memory from search"]
        P2["Agent operates — doesn't delegate to human"]
        P3["Health semantics over naive checks"]
        P4["Documented paths beat ad-hoc discovery"]
        P5["Security: least privilege in container"]
    end

    P1 --> AM["agentmemory"]
    P1 --> CH["Chroma"]
    P2 --> CRON["8 crons + skills"]
    P3 --> SS["stack_status.sh
404 = healthy"]
    P4 --> PATHS["paths-and-endpoints.md"]
    P5 --> SNAP["docker_ps.snapshot
no docker.sock"]

    style PRINCIPLES fill:#2d1b4e,color:#fff
```

| Principle | Implementation | Why elite |
|-----------|----------------|-----------|
| Memory ≠ search | agentmemory + Chroma as separate MCPs | No conflation — most agents dump everything into one RAG bucket |
| Self-operating | Crons, skills, familiarization | Hermes maintains awareness without Jacob re-prompting |
| Correct health | 404=OK for agentmemory | Monitors match reality — false alarms don't train ignore |
| Canonical paths | Three-tier script layout + snapshot | Hermes runs ops from inside container safely |
| Least privilege | No docker.sock in Hermes | Compromised agent can't start/stop arbitrary containers |

---

## Full Stack Diagram

```mermaid
flowchart TB
    subgraph MAC["🖥️ Mac"]
        M1["hermes · tutor · ssh vps"]
        M2["LocalForward :4861"]
    end

    subgraph TS["🔒 Tailscale vps-hermes-project:2222"]
        TSOVL["Admin overlay only\n(not agent egress)"]
    end

    subgraph HOST["☁️ VPS Host — 96 GB disk · 8 GB RAM"]
        FW["Hostinger firewall + UFW + fail2ban"]
        DOCKER["Docker Engine"]
        HCRON["Host cron
docker snapshot /15m
tailscale snapshot /30m
perms /5m"]
    end

    subgraph CONTAINERS["Docker Containers"]
        HERMES["hermes-agent-0qzm-hermes-agent-1"]
        AM["agentmemory-o72l-agentmemory-1"]
        CH["chroma Chroma"]
        UK["uptime-kuma-fl0m-uptime-kuma-1"]
        BZH["beszel-kmwv-beszel-1"]
        BZA["beszel-agent"]
        ND["netdata
Tailscale :19999"]
        N8N["n8n-eywu-n8n-1
:32771"]
    end

    subgraph DATA["Persistent Data /opt/data"]
        CONFIG["config.yaml · .env · SOUL.md"]
        OPS["bin/ · scripts/ · .hermes/scripts/"]
        PROJ["Projects/ · skills/ · embeddings/"]
        SNAP["docker_ps.snapshot"]
    end

    MAC --> TS --> FW --> DOCKER
    M2 -.-> HERMES
    HCRON --> SNAP
    DOCKER --> CONTAINERS
    HERMES --> DATA
    HERMES <-->|MCP| AM
    HERMES <-->|MCP| CH
    UK -.-> AM & CH
    BZA --> BZH
    BZH -.-> HERMES

    style HERMES fill:#2d1b4e,color:#fff
    style DATA fill:#1a3a3a,color:#fff
```

---

## Service Inventory

| Service | Container | Endpoint | Healthy | Purpose |
|---------|-----------|----------|---------|---------|
| Hermes | `hermes-agent-0qzm-hermes-agent-1` | gateway | running | AI operator — Discord, cron, MCP |
| agentmemory | `agentmemory-o72l-agentmemory-1` | `:3111/` | **404** | Persistent memory |
| Chroma | `chroma` | `:8000/heartbeat` | **200** | Document RAG |
| Beszel | `beszel-kmwv-beszel-1` | `beszel:8090/health` | **200** | Host metrics |
| Uptime Kuma | `uptime-kuma-fl0m-uptime-kuma-1` | `:3001/` | **302** | HTTP monitors |
| Beszel agent | `beszel-agent` | host network | — | Stats collector |
| Netdata | `netdata` | `netdata:19999` / Tailscale `:19999` | **200** | Deep metrics + Discord alerts |
| n8n | `n8n-eywu-n8n-1` | `http://<VPS_TAILSCALE_IP>:32771` (Mac) | — | Workflow automation (separate Docker network) |

---

## MCP Data Flow

```mermaid
sequenceDiagram
    autonumber
    participant J as Jacob
    participant H as Hermes Gateway
    participant AM as agentmemory
    participant CH as Chroma
    participant DB as chroma

    J->>H: "How's the stack?"
    H->>AM: recall stack facts
    AM-->>H: container names, cron schedule
    H->>H: run stack_status.sh
    H->>CH: query vps_knowledge
    CH->>DB: vector search
    DB-->>CH: APPLICATIONS.md chunks
    CH-->>H: grounded doc excerpts
    H-->>J: brief with live + remembered + cited facts
```

---

## Memory vs Knowledge — The Core Split

```mermaid
flowchart LR
    subgraph INPUT["Jacob provides"]
        SAY["'Remember I work on CareConnectLite'"]
        WRITE["Writes APPLICATIONS.md"]
    end

    SAY --> AM["agentmemory
user facts"]
    WRITE --> DOC["docs/"]
    DOC --> BOOT["chroma_bootstrap.py"]
    BOOT --> CH["Chroma
vps_knowledge"]

    H["Hermes query"] --> AM
    H --> CH

    style AM fill:#1a4a2e,color:#fff
    style CH fill:#1a2a4a,color:#fff
```

---

## Canonical Path Model

```mermaid
flowchart TD
    EDIT["Edit cron scripts
.hermes/scripts/"]
    SYNC["sync_cron_scripts.sh"]
    RUN["Cron runs
/opt/data/scripts/"]
    MANUAL["Manual ops
/opt/data/bin/"]
    DOCS["Documentation
Projects/VPS_Hermes_Project/docs/"]
    INDEX["chroma_bootstrap.py"]
    CHROMA["vps_knowledge"]

    EDIT --> SYNC
    SYNC --> RUN
    SYNC --> MANUAL
    DOCS --> INDEX --> CHROMA

    style EDIT fill:#3a2a1a,color:#fff
    style CHROMA fill:#1a2a4a,color:#fff
```

| Path | Role |
|------|------|
| `/opt/data/` | HERMES_HOME |
| `/opt/data/bin/` | Manual ops: `stack_status.sh`, `chroma_bootstrap.py` |
| `/opt/data/scripts/` | Cron runtime |
| `/opt/data/.hermes/scripts/` | Cron source + sync |
| `/opt/data/skills/vps-ops/references/paths-and-endpoints.md` | Canonical reference |
| `/opt/data/docker_ps.snapshot` | Container list (host-refreshed) |

---

## Provider Resilience

```mermaid
flowchart TD
    REQ["Request"] --> DS["① DeepSeek
deepseek-v4-flash (primary)"]
    DS -->|fail / 401| OR["② OpenRouter
gemini-2.5-flash-lite"]
    OR -->|cap/fail| ANT["③ Anthropic Haiku"]
    ANT -->|fail| GOOG["④ Google Gemini"]

    style DS fill:#1a4a2e,color:#fff
```

Four-provider fallback means Hermes stays online when any single API has quota issues — another ops maturity signal.

---

## Container Security Model

| Constraint | Reason |
|------------|--------|
| No docker.sock | Agent cannot control host containers |
| Non-root uid 10000 | Reduced blast radius |
| `.env` mode 600 | Secrets not world-readable |
| Filebrowser tunnel-only | No public file port |
| Secrets never in Chroma | Index contains docs only |

---

## Egress Model (live)

| Traffic | Path | VPN |
|---------|------|-----|
| Jacob → VPS | Tailscale → SSH `:2222` via `<VPS_TAILSCALE_IP>` | Tailscale (admin) |
| Hermes → APIs / web | Docker → host routing → internet | **None** (Hostinger IP) |
| NordVPN (Mac) | Reinstalled — **disconnect for VPS admin** | Breaks Tailscale when connected |

Inbound security is strong; outbound is **not anonymized**. See [SECURITY.md](SECURITY.md#egress--vpn-posture-live-june-19-2026).

---

## Timezones

| Location | Zone | Impact |
|----------|------|--------|
| VPS host | **UTC** | Cron schedules, logs, OpenRouter $1/day cap reset at UTC midnight |
| Jacob's Mac | `America/Chicago` (CDT) | Wall-clock for daily work |

Convert before editing `cron/jobs.json`: **CDT + 5 hours = UTC** (summer).

---

## Version & Upgrade Path

| | |
|--|--|
| **Live** | Hermes v0.16.0 in `hermes-agent-0qzm-hermes-agent-1` |
| **Target** | [v0.17.0 (v2026.6.19)](https://github.com/NousResearch/hermes-agent/releases/tag/v2026.6.19) |
| **Procedure** | [HERMES_VPS_SETUP.md](HERMES_VPS_SETUP.md#version--upgrade) |

---

## Automation Layers

```mermaid
flowchart TB
    subgraph HOST["VPS host"]
        DS["docker_ps.snapshot /15m"]
        TS["tailscale snapshot /30m"]
        FP["fix-hermes-perms /5m"]
    end

    subgraph HERMES["Hermes container"]
        JOBS["cron/jobs.json — 8 jobs"]
        SRC[".hermes/scripts/"]
        RUN["scripts/ runtime"]
    end

    subgraph DAILY6["6:00–6:20 AM CDT (11:00/11:10/11:20 UTC — staggered)"]
        I["improvement-pulse"]
        E["exposure-audit"]
        A["admin-plane-check"]
    end

    SRC --> RUN --> JOBS
    DS & TS --> A
    JOBS --> DAILY6

    style DAILY6 fill:#2d1b4e,color:#fff
```

---

## Health Check

```bash
/opt/data/bin/stack_status.sh
```

---

*Last audited: June 19, 2026 (final pass — 8 crons)*
*See also: [APPLICATIONS.md](APPLICATIONS.md) · [README.md](README.md)*
