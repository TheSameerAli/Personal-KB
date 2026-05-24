---
title: Homelab - Kubernetes Apps
date: 2026-05-23
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

All apps run on the [[K3s]] cluster. Storage is primarily NFS-backed (`lab-storage-nfs-client`) with some apps using `local-path` for performance-sensitive data.

### Dashboard & Monitoring

| App | Namespace | Image | Domain | Purpose |
|-----|-----------|-------|--------|---------|
| [[Homarr]] | `homarr` | `ghcr.io/homarr-labs/homarr:1.43.1` | `home.nitrolab.cloud` | Home dashboard |
| [[Uptime Kuma]] | `uptimekuma` | `louislam/uptime-kuma:latest` | `uptime.nitrolab.cloud` | Uptime monitoring |

### Productivity

| App | Namespace | Image | Domain | Purpose |
|-----|-----------|-------|--------|---------|
| [[Nextcloud]] | `nextcloud` | `kilrah/nextcloud-ffmpeg:31.0.2` | `drive.nitro.lab` | File sync & cloud storage |
| [[n8n]] | `n8n` | `sam78640/n8n:1.122.5-curl` | `n8n.nitrolab.cloud` | Workflow automation |
| [[Code Server]] | `code-server` | `linuxserver/code-server:4.99.1` | `code.nitro.lab` / `code.nitrolab.cloud` | Remote VS Code |
| [[Invoice Ninja]] | `invoiceninja` | `invoiceninja/invoiceninja:5.12.30` | `invoice.nitrolab.cloud` | Invoicing |
| [[Browserless]] | `browserless` | `ghcr.io/browserless/chromium:latest` | `browserless.nitro.lab` | Headless Chrome API |

### Media

| App | Namespace | Image | Domain | Purpose |
|-----|-----------|-------|--------|---------|
| [[Emby]] | `media-stack` | `emby/embyserver:4.9.1.80` | `play.nitrolab.cloud` | Media server |
| [[qBittorrent]] | `media-stack` | `lscr.io/linuxserver/qbittorrent:5.0.4` | `torrent.nitro.lab` | Torrent client (VPN via Gluetun/AirVPN) |
| [[Prowlarr]] | `media-stack` | - | `prowlarr.nitro.lab` | Indexer manager |
| [[Sonarr]] | `media-stack` | - | `sonarr.nitro.lab` | TV show automation |
| [[Radarr]] | `media-stack` | - | `radarr.nitro.lab` | Movie automation |
| [[Jellyseerr]] | `media-stack` | - | `jellyseerr.nitro.lab` / `request.nitrolab.cloud` | Media requests |
| [[Bazarr]] | `media-stack` | - | `bazarr.nitro.lab` | Subtitle management |

### Security & Passwords

| App | Namespace | Image | Domain | Purpose |
|-----|-----------|-------|--------|---------|
| [[Vaultwarden]] | `vaultwarden` | `vaultwarden/server:1.34.3` | `v.nitrolab.cloud` | Bitwarden-compatible password manager |

### Home & IoT

| App | Namespace | Image | Domain | Purpose |
|-----|-----------|-------|--------|---------|
| [[Home Assistant]] | `default` | `homeassistant/home-assistant:stable` | `ha.nitro.lab` | Smart home automation |
| [[Frigate]] | `cctv` | `ghcr.io/blakeblackshear/frigate:stable` | `cctv.shop.nitro.lab` | NVR / CCTV with AI detection |

### Knowledge & Reading

| App | Namespace | Image | Domain | Purpose |
|-----|-----------|-------|--------|---------|
| [[Kavita]] | `kavita` | `lscr.io/linuxserver/kavita:latest` | `k.nitrolab.cloud` | Ebook / manga reader |
| [[Obsidian LiveSync]] | `obsidian` | `couchdb:3.3` | `livesync.nitrolab.cloud` | CouchDB for Obsidian sync |

### Finance

| App | Namespace | Image | Domain | Purpose |
|-----|-----------|-------|--------|---------|
| [[Actual Budget]] | `actual` | `actualbudget/actual-server:latest` | `budget.sameerali.co.uk` | Budgeting tool |

## Databases

| Database | Namespace | Image | Purpose |
|----------|-----------|-------|---------|
| [[PostgreSQL]] 14 | `databases` | `postgres:14.17` | Shared DB for n8n, Nextcloud, etc. |
| [[MySQL]] 8.4 | `databases` | `mysql:8.4` | Shared DB for Invoice Ninja |

## VPN Sidecar Pattern

Several apps route traffic through a [[Gluetun]] VPN sidecar:

- **qBittorrent** — AirVPN (WireGuard)
- **Frigate** — Custom WireGuard endpoint

> [!NOTE]
> The VPN containers use `NET_ADMIN` capability and the Gluetun image (`qmcgaw/gluetun:v3.36.0`) for provider-agnostic WireGuard tunnelling.

## Resource Allocation

| App | CPU Request | CPU Limit | Memory Request | Memory Limit |
|-----|-------------|-----------|----------------|--------------|
| n8n | 1 | 4 | 4Gi | 8Gi |
| Nextcloud | 1 | 3 | 4Gi | 8Gi |
| Emby | 1 | 2 | 2Gi | 6Gi |
| qBittorrent | 1 | 1 | 3Gi | 6Gi |
| Frigate | - | 1.5 | - | 4Gi |
| Code Server | 1 | 1 | 1Gi | 2Gi |
| Homarr | 100m | 500m | 512Mi | 1Gi |
| Vaultwarden | 100m | 500m | 256Mi | 512Mi |
| Uptime Kuma | 100m | 500m | 256Mi | 512Mi |
| Kavita | 100m | 500m | 256Mi | 512Mi |
| Actual Budget | 100m | 500m | 256Mi | 512Mi |

## See Also

- [[Homelab - Overview]]
- [[Homelab - Network]]
- [[Homelab - Ansible Automation]]
