# Hermes Multi-Agent Architecture Plan

*Generated: June 1, 2026*

Full audit of the Kubernetes cluster, Hermes deployment, and available tools — architecture plan for adding a dedicated coding agent.

---

## Current State

```
┌─────────────────────────────────────────────────────┐
│               k3s Cluster (4 nodes)                  │
│  ┌───────────────────────────────────────────────┐  │
│  │          hermes-agent namespace               │  │
│  │                                               │  │
│  │  ┌──────────┐  ┌──────────┐  ┌────────────┐  │  │
│  │  │ Hermes   │  │Browserless│  │Investment  │  │  │
│  │  │ Gateway  │  │ (Chrome)  │  │Dashboard   │  │  │
│  │  │          │  │           │  │(Streamlit) │  │  │
│  │  │ Model:   │  └──────────┘  └────────────┘  │  │
│  │  │ GPT-5.5  │                                │  │
│  │  │ +DS fallbk│   Claude Code ✅ installed     │  │
│  │  └──────────┘   Codex CLI ✅ installed        │  │
│  │       │                                       │  │
│  │  nginx ingress @ 192.168.3.4                  │  │
│  │  invest.nitro.lab → Investment Dashboard      │  │
│  └───────────────────────────────────────────────┘  │
│  ┌───────────────────────────────────────────────┐  │
│  │     kubernetes-dashboard (no ingress)          │  │
│  └───────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────┘
```

**Already installed:**
- Claude Code v2.1.159 (Claude Sonnet 4 — best coding model)
- Codex CLI v0.135.0
- Cluster-admin RBAC
- nginx ingress for exposing services
- Hermes Kanban system for multi-agent task queues

---

## Target Architecture

```
┌──────────────────────────────────────────────────────────┐
│                   k3s Cluster                            │
│                                                          │
│  ┌────────────────────────────────────────────────────┐  │
│  │  hermes-agent namespace                            │  │
│  │                                                    │  │
│  │  ┌─────────────┐    Kanban Board     ┌──────────┐  │  │
│  │  │  Hermes     │◄───────────────────►│  Hermes  │  │  │
│  │  │  Gateway    │   (shared SQLite)   │  Coder   │  │  │
│  │  │  (Orch.)    │                     │          │  │  │
│  │  │             │                     │  Model:  │  │  │
│  │  │  Model:     │                     │  Claude  │  │  │
│  │  │  GPT-5.5    │                     │  Sonnet 4│  │  │
│  │  └──────┬──────┘                     └──────────┘  │  │
│  │         │                                          │  │
│  │         │ Quick sync tasks                         │  │
│  │         ▼                                          │  │
│  │  ┌──────────────┐                                  │  │
│  │  │ delegate_task│  (uses Claude for code)          │  │
│  │  └──────────────┘                                  │  │
│  │                                                    │  │
│  │  ┌──────────────┐  ┌──────────────┐                │  │
│  │  │ Agent        │  │ Kubernetes   │                │  │
│  │  │ Dashboard    │  │ Dashboard    │                │  │
│  │  │ (custom)     │  │ (existing)   │                │  │
│  │  └──────────────┘  └──────────────┘                │  │
│  │       │                   │                        │  │
│  └───────┼───────────────────┼────────────────────────┘  │
│          │                   │                           │
│     hermes.nitro.lab    k8s.nitro.lab                    │
└──────────────────────────────────────────────────────────┘
```

---

## The Plan: 3 Layers, 4 Steps

### Layer 1 — Coding Agent (Claude Sonnet 4)

Two parallel approaches:

**1a. Configure `delegate_task` to use Claude** *(immediate, no new pods)*
```yaml
# config.yaml
delegation:
  provider: anthropic
  model: claude-sonnet-4-6
```
Every `delegate_task()` call spawns a Claude-powered subagent for coding — the quick/sync path.

**1b. Deploy a dedicated "Hermes Coder" pod** *(for long-running coding)*
A second Hermes Deployment with a `coder` profile:
- Uses Claude Sonnet 4 exclusively
- Listens on the Kanban board for coding tasks
- Has separate sessions, context, and skills
- Can run lengthy coding missions autonomously

### Layer 2 — Orchestration

```
Sameer → Hermes Gateway (me)
              │
              │ Is this a coding task?
              │
        ┌─────┴─────┐
        │ YES       │ NO
        ▼           ▼
   Plan it      Handle it
        │       myself
   ┌────┴────┐
   │ Quick?  │ Long?
   ▼         ▼
delegate_task   Kanban task
(Claude sync)   → Hermes Coder picks up
                → Works autonomously
                → Reports back via board
                → I review & relay to you
```

### Layer 3 — Web Dashboard

**Quick win (Phase 1):** Expose existing Kubernetes Dashboard
- Already deployed in `kubernetes-dashboard` namespace
- Add ingress → `k8s.nitro.lab`
- Shows all pods, logs, resource usage, events
- < 30 minutes

**Custom dashboard (Phase 2):** Agent-specific UI
- Shows Hermes Gateway + Hermes Coder status
- Active coding tasks, sessions, token usage
- Kanban board viewer
- Deploy as another Streamlit pod (same pattern as investment dashboard)
- Ingress at `hermes.nitro.lab`

---

## Implementation Steps

| # | Step | Effort | What you get |
|---|------|--------|-------------|
| 1 | Set `delegation.provider: anthropic` + `delegation.model: claude-sonnet-4-6` in config | 5 min | All subagents use Claude for code |
| 2 | Create `coder` profile + K8s Deployment manifest | 30 min | Dedicated coding agent pod |
| 3 | Expose K8s dashboard ingress | 15 min | `k8s.nitro.lab` — pod management |
| 4 | Build + deploy agent dashboard | 1-2 hr | `hermes.nitro.lab` — agent management |

---

## Key Decision Needed

**Do you have an Anthropic API key?** Claude Code is installed but needs auth. If not, alternatives: Codex CLI with a stronger model, or OpenRouter to access Claude.
