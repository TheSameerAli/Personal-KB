---
title: Homelab - Kubernetes Apps
date: 2026-05-24
tags:
  - homelab
  - kubernetes
  - self-hosted
  - docker
type: note
status: active
related:
  - "[[Homelab - Overview]]"
  - "[[Homelab - Services]]"
  - "[[Homelab - Network]]"
---

## Deployed Applications

All apps run on the [[K3s]] cluster. Storage is primarily NFS-backed (`lab-storage-nfs-client`) with some apps using `local-path` for performance-sensitive data. [[Longhorn]] provides distributed block storage.

### AI & Automation

| App | Namespace | Domain | Purpose |
|-----|-----------|--------|---------|
| [[Hermes Agent]] | `hermes-agent` | — (internal) | AI assistant with cluster-admin, Proxmox access, and GitHub integration |
| [[n8n]] | `n8n` | `n8n.nitrolab.cloud` / `n8n.nitro.lab` | Workflow automation |

### Infrastructure

| App | Namespace | Purpose |
|-----|-----------|---------|
| [[ArgoCD]] | `argocd` | GitOps continuous delivery |
| [[Kubernetes Dashboard]] | `kubernetes-dashboard` | Web-based cluster management |
| [[Longhorn]] | `longhorn-system` | Distributed block storage |
| [[cert-manager]] | `cert-manager` | TLS certificate automation |
| [[Tailscale]] | `tailscale` | Mesh VPN networking |

### Monitoring

| App | Namespace | Domain | Purpose |
|-----|-----------|--------|---------|
| [[Uptime Kuma]] | `uptimekuma` | `uptime.nitrolab.cloud` | Uptime monitoring |

### Productivity

| App | Namespace | Domain | Purpose |
|-----|-----------|--------|---------|
| [[Nextcloud]] | `nextcloud` | `drive.nitro.lab` | File sync & cloud storage |

### Media

| App | Namespace | Domain | Purpose |
|-----|-----------|--------|---------|
| [[Emby]] | `media-stack` | `play.nitrolab.cloud` | Media server |
| [[qBittorrent]] | `media-stack` | `torrent.nitro.lab` | Torrent client (VPN via Gluetun/AirVPN) |
| [[Prowlarr]] | `media-stack` | `prowlarr.nitro.lab` | Indexer manager |
| [[Sonarr]] | `media-stack` | `sonarr.nitro.lab` | TV show automation |
| [[Radarr]] | `media-stack` | `radarr.nitro.lab` | Movie automation |
| [[Jellyseerr]] | `media-stack` | `jellyseerr.nitro.lab` / `request.nitrolab.cloud` | Media requests |
| [[Bazarr]] | `media-stack` | `bazarr.nitro.lab` | Subtitle management |
| [[FlareSolverr]] | `media-stack` | `flaresolver.nitro.lab` | Cloudflare bypass for indexers |

### Security & Passwords

| App | Namespace | Domain | Purpose |
|-----|-----------|--------|---------|
| [[Vaultwarden]] | `vaultwarden` | `v.nitrolab.cloud` | Bitwarden-compatible password manager |

### Home & IoT

| App | Namespace | Domain | Purpose |
|-----|-----------|--------|---------|
| [[Frigate]] | `cctv` | `cctv.shop.nitro.lab` / `cctv.shop.nitrlab.cloud` | NVR / CCTV with AI detection |

### Projects

| App | Namespace | Purpose |
|-----|-----------|---------|
| [[Coding Kitty]] | `coding-kitty` | Coding Kitty project workloads |

## Databases

| Database | Namespace | Image | Purpose |
|----------|-----------|-------|---------|
| [[PostgreSQL]] 14 | `databases` | `postgres:14` | Shared DB for n8n, Nextcloud, etc. |
| [[MySQL]] 8.4 | `databases` | `mysql:8.4` | Shared DB for various apps |

## VPN Sidecar Pattern

Several apps route traffic through a [[Gluetun]] VPN sidecar:

- **qBittorrent** — AirVPN (WireGuard)

> [!NOTE]
> The VPN containers use `NET_ADMIN` capability and the Gluetun image for provider-agnostic WireGuard tunnelling.

## Hermes Agent

The [[Hermes Agent]] (namespace `hermes-agent`) is an AI assistant running inside the cluster with:

- **Cluster-admin RBAC** — Full Kubernetes API access for self-diagnosis and pod management
- **Proxmox API access** — VM/CT management across the 3-node Proxmox cluster
- **GitHub access** — Push/pull to `TheSameerAli/homelab` for infrastructure-as-code sync
- **kubectl** — CLI tool for direct cluster operations
- **gh CLI** — GitHub CLI for repo management

The agent maintains the homelab repo as a single source of truth and updates the [[Personal-KB|Obsidian knowledge base]] whenever changes are made.

### Deployment

- **Gateway**: `hermes-gateway` deployment
- **Bootstrap**: Init job for first-run setup
- **Daily Brief**: CronJob for scheduled summaries
- **RBAC**: `kubernetes/02-security/hermes-agent/rbac.yaml` in the homelab repo

## See Also

- [[Homelab - Overview]]
- [[Homelab - Network]]
- [[Homelab - Ansible Automation]]
