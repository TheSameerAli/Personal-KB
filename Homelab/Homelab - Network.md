---
title: Homelab - Network
date: 2026-05-24
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
| 192.168.2.x | Infrastructure | Proxmox hosts, Kubernetes nodes, NFS, DNS, VPN |
| 192.168.3.x | Services | Ingress, exposed services |
| 192.168.30.x | Kubernetes | K3s cluster internal networking |

## Key Addresses

| IP | Role |
|----|------|
| 192.168.0.1 | Gateway / Router |
| 192.168.2.10 | [[PiHole]] (DNS, DHCP) |
| 192.168.2.20 | [[Wireguard]] VPN server |
| 192.168.3.4 | NGINX Ingress (LoadBalancer IP) |
| 192.168.30.222 | K3s API Server VIP ([[kube-vip]]) |
| 192.168.30.80-90 | [[MetalLB]] LoadBalancer range |
| 10.43.0.1 | Kubernetes API (cluster-internal) |

## DNS

Local DNS resolution is handled by [[PiHole]] at `192.168.2.10`. Internal domains (`*.nitro.lab`) resolve to cluster ingress IPs via PiHole local DNS records.

For the K3s pod network, CoreDNS resolves internal Kubernetes service names (e.g., `hermes-gateway.hermes-agent.svc.cluster.local`).

## VPN Access

Two VPN layers are available:

| VPN | Endpoint | Purpose |
|-----|----------|---------|
| [[Wireguard]] | `192.168.2.20:10086` | Remote access to LAN |
| [[Tailscale]] | `tailscale` namespace | Mesh VPN for pod-to-pod connectivity |

## Kubernetes Networking

- CNI: Flannel on `eth0`
- Pod CIDR: `10.52.0.0/16`
- Service CIDR: `10.43.0.0/16`
- Service type: ClusterIP (all apps) with NGINX Ingress for external access
- MetalLB provides LoadBalancer IPs in Layer 2 mode (`192.168.30.80-90`)
- Ingress: NGINX controller at `192.168.3.4`

## See Also

- [[Homelab - Overview]]
- [[Homelab - Services]]
- [[Homelab - Kubernetes Apps]]
