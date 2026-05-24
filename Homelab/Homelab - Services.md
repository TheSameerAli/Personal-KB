---
title: Homelab - Services
date: 2026-05-23
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
| [[Proxmox]] (storage1) | 192.168.2.150 | 8006 | - |
| nitrolab-nfs | 192.168.2.152 | - | - |
| [[PiHole]] | 192.168.2.10 | 80 | pihole.nitro.lab |
| [[Wireguard]] | 192.168.2.20 | 10086 | wireguard.nitro.lab |

## Kubernetes Nodes

| Node | IP Address | Role |
|------|-----------|------|
| kube-cn1 | 192.168.2.3 | Control plane |
| kube-wn1 | 192.168.2.4 | Worker |
| kube-wn2 | 192.168.2.160 | Worker |
| kube-wn3 | 192.168.2.161 | Worker |

> [!NOTE]
> The Ansible inventory uses a different subnet (192.168.30.x) for the K3s cluster internal communication. The 192.168.2.x addresses above are the Proxmox VM management IPs.

## Ingress (192.168.3.4)

All Kubernetes services are exposed through NGINX Ingress at `192.168.3.4:80/443`.

### Internal Domains (*.nitro.lab)

| Domain | Service |
|--------|---------|
| cctv.shop.nitro.lab | [[Frigate]] |
| drive.nitro.lab | [[Nextcloud]] |
| ha.nitro.lab | [[Home Assistant]] |
| code.nitro.lab | [[Code Server]] |
| browserless.nitro.lab | [[Browserless]] |
| n8n.nitro.lab | [[n8n]] |
| torrent.nitro.lab | [[qBittorrent]] |
| prowlarr.nitro.lab | [[Prowlarr]] |
| sonarr.nitro.lab | [[Sonarr]] |
| radarr.nitro.lab | [[Radarr]] |
| jellyseerr.nitro.lab | [[Jellyseerr]] |
| bazarr.nitro.lab | [[Bazarr]] |
| wireguard.nitro.lab | [[Wireguard]] |

### Public Domains (*.nitrolab.cloud)

| Domain | Service |
|--------|---------|
| home.nitrolab.cloud | [[Homarr]] |
| uptime.nitrolab.cloud | [[Uptime Kuma]] |
| v.nitrolab.cloud | [[Vaultwarden]] |
| n8n.nitrolab.cloud | [[n8n]] |
| play.nitrolab.cloud | [[Emby]] |
| request.nitrolab.cloud | [[Jellyseerr]] |
| code.nitrolab.cloud | [[Code Server]] |
| invoice.nitrolab.cloud | [[Invoice Ninja]] |
| k.nitrolab.cloud | [[Kavita]] |
| livesync.nitrolab.cloud | [[Obsidian LiveSync]] |
| cctv.shop.nitrlab.cloud | [[Frigate]] (public) |

### Other Domains

| Domain | Service |
|--------|---------|
| budget.sameerali.co.uk | [[Actual Budget]] |

## See Also

- [[Homelab - Overview]]
- [[Homelab - Kubernetes Apps]]
- [[Homelab - Ansible Automation]]
