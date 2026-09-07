---
title: Proxmox VE
description: Why 45Drives' Proxinator virtualization line is built on Proxmox VE, the open-source hypervisor platform.
---

# Proxmox VE: virtualization without the vSphere license

[Proxmox VE](https://www.proxmox.com/en/proxmox-virtual-environment/overview) is an
open-source virtualization platform combining KVM (kernel-based virtual machines) and LXC
(Linux containers) under one web-managed cluster, built on Debian. It's a direct, fully
open-source alternative to proprietary hypervisor platforms like VMware vSphere — clustering,
live migration, and software-defined storage integration included, with no per-socket
licensing.

## Why this matters for a storage server

Compute and storage have converged for a lot of workloads: rather than a separate SAN feeding
a separate virtualization cluster, hyperconverged infrastructure runs both on the same nodes.
Proxmox VE is 45Drives' open-source answer to that pattern — and because it integrates
natively with Ceph, the storage layer and the compute layer can be genuinely the same
open-source stack, not two vendors' products duct-taped together.

## How 45Drives uses it

45Drives' **Proxinator** hyperconverged infrastructure line is built directly on Proxmox VE,
paired with Ceph for the underlying storage — VM and container workloads run on the same
cluster that also serves as a self-healing, distributed storage pool. That's the same Ceph
described on the [Ceph page](/ceph/), just consumed by Proxmox's virtualization layer instead
of (or in addition to) file/object sharing. For cluster-wide visibility, 45Drives also tracks
[Pulse](https://github.com/45Drives/Pulse) — an open-source, real-time monitoring dashboard
for Proxmox VE (originally created by [rcourtman](https://github.com/rcourtman/Pulse)) that
shows metrics across every node in a cluster from one screen.

## Why that matters if you're evaluating storage

- **No per-socket or per-core licensing.** Proxmox VE's core platform is free and open source;
  paid support subscriptions are optional, not a requirement to use it in production.
- **One open-source stack, not a vendor stack plus a storage vendor's stack.** Compute and
  storage sharing the same Ceph foundation means one thing to learn, one community to lean on.
- **A real alternative to proprietary hypervisors**, not a toy — Proxmox VE runs production
  workloads at real scale across a large user base independent of any one hardware vendor.

## Learn more

- [proxmox.com](https://www.proxmox.com) — the official Proxmox project
- [Proxmox VE documentation](https://pve.proxmox.com/pve-docs/)

See [Ceph](/ceph/) for the storage layer Proxinator builds on, or
[Build Your Own](/build-your-own/) for a hands-on Ceph deployment you can try on your own
Proxmox cluster.
