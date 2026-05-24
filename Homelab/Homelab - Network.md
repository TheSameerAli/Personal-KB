---
title: Homelab - Network
date: 2026-05-23
tags:
  - homelab
  - networking
  - infrastructure
type: note
status: active
related:
  - "[[Homelab - Overview]]"
  - "[[Homelab - Services]]"
---

## Network Topology

| Subnet | CIDR | Purpose |
|--------|------|---------|
| 192.168.0.0/16 | Full range | Home network |
| 192.168.2.x | Infrastructure | Proxmox hosts, NFS, DNS, VPN |
| 192.168.3.x | Services | Ingress, exposed services |
| 192.168.30.x | Kubernetes | K3s nodes and cluster networking |

## Key Addresses

| IP | Role |
|----|------|
| 192.168.0.1 | Gateway / Router |
| 192.168.2.10 | [[PiHole]] (DNS) |
| 192.168.30.222 | K3s API Server VIP ([[kube-vip]]) |
| 192.168.30.80-90 | [[MetalLB]] LoadBalancer range |

## DNS

Local DNS resolution is handled by [[PiHole]] at `192.168.2.10`. Internal domains (`*.nitro.lab`) resolve to cluster ingress IPs via PiHole local DNS records.

## VPN Access

[[Wireguard]] server at `192.168.2.20:10086` provides remote access to the LAN. Public domain: `wireguard.nitro.lab`.

## Kubernetes Networking

- CNI: Flannel on `eth0`
- Pod CIDR: `10.52.0.0/16`
- Service type: ClusterIP (all apps) with NGINX Ingress for external access
- MetalLB provides LoadBalancer IPs in Layer 2 mode

## See Also

- [[Homelab - Overview]]
- [[Homelab - Services]]
- [[Homelab - Kubernetes Apps]]
