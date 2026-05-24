---
title: Homelab - Overview
date: 2026-05-23
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

Self-hosted homelab running on [[Proxmox]] virtualisation with a [[K3s]] Kubernetes cluster as the primary workload orchestrator. Infrastructure is managed as code using [[Ansible]] for provisioning and raw Kubernetes manifests for application deployments. Backups are handled via [[Restic]] to [[Backblaze B2]].

## Architecture

```
┌─────────────────────────────────────────────────────────┐
│                    Home Network                          │
│                  192.168.0.0/16                          │
├─────────────────────────────────────────────────────────┤
│                                                         │
│  ┌──────────┐  ┌──────────┐  ┌──────────────────────┐  │
│  │ Proxmox  │  │ Proxmox  │  │ Proxmox (storage1)   │  │
│  │  cn1     │  │  wn1     │  │  NFS Server          │  │
│  │ .2.1     │  │  .2.2    │  │  .2.150 / .2.152     │  │
│  └────┬─────┘  └────┬─────┘  └──────────────────────┘  │
│       │              │                                   │
│  ┌────┴──────────────┴────────────────────────────┐     │
│  │         K3s Cluster (v1.30.2)                  │     │
│  │  VIP: 192.168.30.222                           │     │
│  │                                                │     │
│  │  Masters: .30.38, .30.39, .30.40              │     │
│  │  Workers: .30.41, .30.42                      │     │
│  │                                                │     │
│  │  MetalLB Range: 192.168.30.80 - .30.90        │     │
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
- HA: 3 master nodes with [[kube-vip]] (ARP mode)
- API Server VIP: `192.168.30.222`
- Networking: Flannel CNI (eth0)
- Load Balancer: [[MetalLB]] v0.14.8 (Layer 2, range `192.168.30.80-90`)
- Ingress: NGINX Ingress Controller
- Storage: NFS client provisioner (`lab-storage-nfs-client`) + local-path
- TLS: [[cert-manager]] for certificate management

## Domains

Two domain tiers are in use:

| Tier | Domain | Access |
|------|--------|--------|
| Internal | `*.nitro.lab` | LAN only (via [[PiHole]] DNS) |
| Public | `*.nitrolab.cloud` | Internet-facing (via [[Wireguard]] or direct) |
| Public | `*.sameerali.co.uk` | Personal domain |

## Backup Strategy

- Tool: [[Restic]]
- Destination: [[Backblaze B2]]
- Scope: Per-app backup CronJobs running inside Kubernetes
- Apps with backups: Nextcloud, Vaultwarden, Uptime Kuma, Homarr, Kavita, n8n, Obsidian LiveSync, main Postgres, main MySQL

## Repository Structure

```
~/code/homelab/
├── ansible/
│   ├── collections/          # Ansible Galaxy requirements
│   └── playbooks/
│       ├── management/       # IP setup, general management
│       └── deployments/      # K3s cluster, Postgres, NFS
├── kubernetes/
│   ├── 01-namespaces/        # Namespace definitions
│   ├── 02-security/          # cert-manager
│   ├── 03-apps/              # Application deployments
│   ├── 04-ingress/           # NGINX ingress + domain configs
│   └── 05-databases/         # PostgreSQL 14, MySQL 8.4
└── backups/                  # Restic initialisation scripts
```

## See Also

- [[Homelab - Network]]
- [[Homelab - Services]]
- [[Homelab - Kubernetes Apps]]
- [[Homelab - Ansible Automation]]
