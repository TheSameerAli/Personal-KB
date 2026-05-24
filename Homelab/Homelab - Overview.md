---
title: Homelab - Overview
date: 2026-05-24
tags:
  - homelab
  - infrastructure
  - kubernetes
  - self-hosted
type: project
status: active
related:
  - "[[Homelab - Network]]"
  - "[[Homelab - Services]]"
  - "[[Homelab - Kubernetes Apps]]"
  - "[[Homelab - Ansible Automation]]"
---

## Summary

Self-hosted homelab running on [[Proxmox]] virtualisation with a [[K3s]] Kubernetes cluster as the primary workload orchestrator. Infrastructure is managed as code using [[Ansible]] for provisioning and raw Kubernetes manifests for application deployments. An AI assistant ([[Hermes Agent]]) runs inside the cluster with full admin access for self-diagnosis and management. Backups are handled via CronJobs inside Kubernetes with per-app backup scripts.

## Architecture

```
┌─────────────────────────────────────────────────────────┐
│                    Home Network                          │
│                  192.168.0.0/16                          │
├─────────────────────────────────────────────────────────┤
│                                                         │
│  ┌──────────┐  ┌──────────┐  ┌──────────────────────┐  │
│  │ Proxmox  │  │ Proxmox  │  │ Proxmox (storage1)   │  │
│  │  cn1     │  │  wn1     │  │  NFS / Storage        │  │
│  │ .2.1     │  │  .2.2    │  │  .2.150               │  │
│  └────┬─────┘  └────┬─────┘  └──────────────────────┘  │
│       │              │                                   │
│  ┌────┴──────────────┴────────────────────────────┐     │
│  │         K3s Cluster (v1.30.2)                  │     │
│  │  API Server VIP: 192.168.30.222                │     │
│  │                                                │     │
│  │  Control Plane: kube-cn1 (.2.3)               │     │
│  │  Workers: kube-wn1 (.2.4)                     │     │
│  │           kube-wn2 (.2.160)                    │     │
│  │           kube-wn3 (.2.161)                    │     │
│  │                                                │     │
│  │  MetalLB Range: 192.168.30.80 - .30.90        │     │
│  │  Ingress IP: 192.168.3.4                       │     │
│  └────────────────────────────────────────────────┘     │
│                                                         │
│  ┌──────────┐  ┌──────────┐                             │
│  │ PiHole   │  │Wireguard │                             │
│  │ .2.10    │  │ .2.20    │                             │
│  └──────────┘  └──────────┘                             │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

## Hardware

| Node | Role | IP | Notes |
|------|------|-----|-------|
| cn1 | Proxmox host | 192.168.2.1 | Control node |
| wn1 | Proxmox host | 192.168.2.2 | Worker node |
| storage1 | Proxmox host / NFS | 192.168.2.150 | Storage server |

## Kubernetes Cluster

- Distribution: [[K3s]] v1.30.2
- HA: Single control-plane with [[kube-vip]] (ARP mode) for API server VIP
- API Server VIP: `192.168.30.222`
- Nodes: 1 control-plane + 3 workers (Ubuntu 24.04)
- Networking: Flannel CNI
- Load Balancer: [[MetalLB]] (Layer 2, range `192.168.30.80-90`)
- Ingress: NGINX Ingress Controller at `192.168.3.4`
- Storage: NFS client provisioner + local-path provisioner + [[Longhorn]]
- TLS: [[cert-manager]] for automated certificate management
- GitOps: [[ArgoCD]] for declarative deployments
- Dashboard: [[Kubernetes Dashboard]] for web-based cluster management
- AI Assistant: [[Hermes Agent]] with cluster-admin RBAC for self-diagnosis and management

## Namespaces (Active)

| Namespace | Purpose |
|---|---|
| `argocd` | GitOps continuous delivery |
| `cctv` | Frigate NVR |
| `cert-manager` | TLS certificate automation |
| `coding-kitty` | Coding Kitty project |
| `databases` | MySQL 8.4 + PostgreSQL 14 |
| `default` | NGINX ingress controller |
| `hermes-agent` | Hermes AI assistant |
| `kube-system` | CoreDNS, metrics-server, local-path |
| `kubernetes-dashboard` | Web-based cluster dashboard |
| `lab-storage` | Lab storage NFS provisioner |
| `longhorn-system` | Distributed block storage |
| `media-stack` | Emby, Sonarr, Radarr, Prowlarr, Bazarr, Jellyseerr, qBittorrent, FlareSolverr |
| `metallb-system` | MetalLB load balancer |
| `n8n` | Workflow automation |
| `nextcloud` | File sync & cloud storage |
| `tailscale` | Mesh VPN networking |
| `uptimekuma` | Uptime monitoring |
| `vaultwarden` | Password manager |

## Domains

Two domain tiers are in use:

| Tier | Domain | Access |
|------|--------|--------|
| Internal | `*.nitro.lab` | LAN only (via [[PiHole]] DNS) |
| Public | `*.nitrolab.cloud` | Internet-facing (via [[Wireguard]] or direct) |

## Repository

Infrastructure-as-code lives at `github.com/TheSameerAli/homelab`:

```
kubernetes/
├── 01-namespaces/        # Namespace definitions
├── 02-security/          # cert-manager + hermes-agent RBAC
├── 03-apps/              # Application deployments
│   ├── frigate/
│   ├── hermes-agent/     # Gateway + bootstrap + cronjob
│   ├── media-stack/
│   ├── n8n/
│   ├── nextcloud/
│   ├── uptime-kuma/
│   └── vault-warden/
├── 04-ingress/           # NGINX ingress + domain configs
│   └── controller/nginx/features/domains/
└── 05-databases/         # PostgreSQL 14, MySQL 8.4
ansible/
└── playbooks/
    ├── management/       # IP setup, general management
    └── deployments/      # K3s cluster, Postgres, NFS (WIP)
backups/                  # Backup scripts
```

## See Also

- [[Homelab - Network]]
- [[Homelab - Services]]
- [[Homelab - Kubernetes Apps]]
- [[Homelab - Ansible Automation]]
