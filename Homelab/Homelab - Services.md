---
title: Homelab - Services
date: 2026-05-24
tags:
  - homelab
  - services
  - self-hosted
type: note
status: active
related:
  - "[[Homelab - Overview]]"
  - "[[Homelab - Network]]"
  - "[[Homelab - Kubernetes Apps]]"
---

## Infrastructure Services

| Service | IP Address | Port | Domain |
|---------|-----------|------|--------|
| [[Proxmox]] (cn1) | 192.168.2.1 | 8006 | cn1.nitro.lab |
| [[Proxmox]] (wn1) | 192.168.2.2 | 8006 | wn1.nitro.lab |
| [[Proxmox]] (storage1) | 192.168.2.150 | 8006 | — |
| [[PiHole]] | 192.168.2.10 | 80 | pihole.nitro.lab |
| [[Wireguard]] | 192.168.2.20 | 10086 | wireguard.nitro.lab |

## Kubernetes Nodes

| Node | IP Address | OS | Role |
|------|-----------|-----|------|
| kube-cn1 | 192.168.2.3 | Ubuntu 24.04.1 | Control plane |
| kube-wn1 | 192.168.2.4 | Ubuntu 24.04.1 | Worker |
| kube-wn2 | 192.168.2.160 | Ubuntu 24.04.1 | Worker |
| kube-wn3 | 192.168.2.161 | Ubuntu 24.04.2 | Worker |

> [!NOTE]
> The Ansible inventory uses a different subnet (192.168.30.x) for the K3s cluster internal communication. The 192.168.2.x addresses above are the Proxmox VM management IPs.

## Ingress (192.168.3.4)

All Kubernetes services are exposed through NGINX Ingress at `192.168.3.4:80/443`.

### Internal Domains (*.nitro.lab)

| Domain | Service |
|--------|---------|
| cctv.shop.nitro.lab | [[Frigate]] |
| drive.nitro.lab | [[Nextcloud]] |
| torrent.nitro.lab | [[qBittorrent]] |
| prowlarr.nitro.lab | [[Prowlarr]] |
| sonarr.nitro.lab | [[Sonarr]] |
| radarr.nitro.lab | [[Radarr]] |
| jellyseerr.nitro.lab | [[Jellyseerr]] |
| bazarr.nitro.lab | [[Bazarr]] |
| flaresolver.nitro.lab | [[FlareSolverr]] |
| n8n.nitro.lab | [[n8n]] |
| wireguard.nitro.lab | [[Wireguard]] |

### Public Domains (*.nitrolab.cloud)

| Domain | Service |
|--------|---------|
| uptime.nitrolab.cloud | [[Uptime Kuma]] |
| v.nitrolab.cloud | [[Vaultwarden]] |
| n8n.nitrolab.cloud | [[n8n]] |
| play.nitrolab.cloud | [[Emby]] |
| request.nitrolab.cloud | [[Jellyseerr]] |
| cctv.shop.nitrlab.cloud | [[Frigate]] (public) |

## See Also

- [[Homelab - Overview]]
- [[Homelab - Kubernetes Apps]]
- [[Homelab - Ansible Automation]]
