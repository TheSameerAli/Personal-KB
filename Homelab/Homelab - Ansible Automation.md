---
title: Homelab - Ansible Automation
date: 2026-05-24
tags:
  - homelab
  - ansible
  - automation
  - infrastructure-as-code
type: note
status: active
related:
  - "[[Homelab - Overview]]"
  - "[[Homelab - Network]]"
---

## Overview

[[Ansible]] handles all bare-metal and VM provisioning for the homelab. Playbooks are organised into two categories: **management** (ongoing ops) and **deployments** (one-time setup). All playbooks live in the homelab Git repo at `github.com/TheSameerAli/homelab`.

## Playbook Structure

```
ansible/
├── collections/
│   └── requirements.yml        # Galaxy dependencies
└── playbooks/
    ├── management/
    │   └── 01-ips-setup/       # Static IP assignment via Netplan
    └── deployments/
        ├── kubernetes/
        │   ├── 01-disable-swaps/   # Pre-req: disable swap on nodes
        │   └── 02-kubernetes-cluster/  # Full K3s cluster deployment
        ├── postgres/               # Standalone PostgreSQL 16 on bare metal
        └── nfs - IMCOMPLETE/       # NFS server setup (WIP)
```

## Management Playbooks

### Static IP Setup

Configures Ubuntu 24.04 nodes with static IPs via [[Netplan]].

- Template: `netplan_static_ip.yaml.j2`
- Default interface: `ens33`
- Default subnet: `255.255.0.0`
- Includes rollback on failure

## Deployment Playbooks

### K3s Cluster (02-kubernetes-cluster)

Full [[K3s]] cluster deployment based on the community k3s-ansible project. Deploys a single control-plane node with 3 workers behind [[kube-vip]] for API server HA.

**Roles:**
- `prereq` — System prerequisites
- `download` — K3s binary download
- `k3s_server` — Control-plane node setup with kube-vip
- `k3s_agent` — Worker node join
- `k3s_server_post` — Post-install (MetalLB, Calico, or Cilium)
- `raspberrypi` — Raspberry Pi-specific tweaks
- `lxc` / `proxmox_lxc` — LXC container support
- `reset` — Cluster teardown

**Key Configuration:**
- K3s version: `v1.30.2+k3s2`
- SSH user: `ansibleuser`
- CNI: Flannel (default), with Calico/Cilium options
- VIP: kube-vip with ARP broadcasts (API VIP: `192.168.30.222`)
- LB: MetalLB native mode, Layer 2
- Traefik and ServiceLB disabled (using NGINX ingress instead)

**Cluster Topology:**
- 1 control-plane: `kube-cn1` (192.168.2.3)
- 3 workers: `kube-wn1` (192.168.2.4), `kube-wn2` (192.168.2.160), `kube-wn3` (192.168.2.161)

**Utilities:**
- `deploy.sh` — Run the cluster deployment
- `reset.sh` — Tear down the cluster
- `reboot.sh` / `reboot.yml` — Rolling reboot

### Standalone PostgreSQL

Deploys [[PostgreSQL]] 16 on a bare-metal node at `192.168.2.100`.

- Adds official PostgreSQL APT repo
- Creates superuser and database
- Configures remote access (`listen_addresses = '*'`)

### NFS Server (Incomplete)

> [!WARNING]
> This playbook is marked as incomplete and should not be used in its current state.

Intended to set up an NFS server for shared cluster storage. Current NFS storage is provisioned via the `lab-storage-nfs-client` provisioner and `nfs-subdir-external-provisioner` in the `databases` namespace.

## Running Playbooks

```bash
# Deploy K3s cluster
cd ansible/playbooks/deployments/kubernetes/02-kubernetes-cluster
ansible-playbook site.yml -i inventory/my-cluster/hosts.ini

# Reset cluster
ansible-playbook reset.yml -i inventory/my-cluster/hosts.ini

# Setup static IPs
cd ansible/playbooks/management/01-ips-setup
ansible-playbook setup-ips.yml -i hosts.ini
```

## See Also

- [[Homelab - Overview]]
- [[Homelab - Kubernetes Apps]]
- [[Homelab - Network]]
