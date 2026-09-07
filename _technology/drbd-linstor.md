---
title: DRBD & LINSTOR
description: Why 45Drives partners with LINBIT on DRBD and LINSTOR for synchronous, block-level high availability — a smaller-scale complement to Ceph.
---

# DRBD & LINSTOR: high availability without a full Ceph cluster

[DRBD](https://linbit.com/drbd/) (Distributed Replicated Block Device) is an open-source Linux
kernel module that mirrors a block device between two or more servers in real time — every
write is synchronously replicated, so if one node goes down, the other already has an
identical, up-to-date copy. [LINSTOR](https://linbit.com/linstor/) is LINBIT's open-source
orchestration layer on top of DRBD, managing volumes, snapshots, and failover across a cluster
rather than by hand.

## Why this matters for a storage server

[Ceph](/ceph/) is the right tool for scale-out clusters — many nodes, self-healing, built for
growth. But not every deployment needs that much cluster: a two-node high-availability pair,
common in edge sites, small offices, or budget-constrained deployments, is a different shape
of problem. DRBD/LINSTOR solve HA at that smaller scale without the operational overhead of
running a full Ceph cluster for just two boxes.

## How 45Drives uses it

45Drives has a direct partnership with [LINBIT](https://www.linbit.com), the company behind
DRBD and LINSTOR, to bring synchronous block-level replication and cluster orchestration to
45Drives' [ZFS](/zfs/)-based storage servers — positioned as a complementary high-availability
option alongside Ceph, not a replacement for it. Where Ceph is the answer for distributed,
many-node scale, DRBD/LINSTOR is the answer for "I need two servers to fail over to each other
reliably."

## Why that matters if you're evaluating storage

- **Right-sized HA.** Not every deployment needs Ceph's operational complexity — DRBD gives
  you real synchronous replication for a two-node pair without standing up a cluster.
- **Open source, not a proprietary SAN feature.** Like everything else in this stack,
  DRBD/LINSTOR are auditable, community-supported projects, not a black-box vendor add-on.
- **A genuine partnership, not just a bundled dependency.** 45Drives and LINBIT worked
  together directly on this integration, rather than 45Drives simply packaging an unmodified
  upstream project.

## Learn more

- [linbit.com](https://www.linbit.com) — LINBIT, the company behind DRBD and LINSTOR
- [DRBD documentation](https://linbit.com/drbd-user-guide/)
- [LINSTOR documentation](https://linbit.com/drbd-user-guide/linstor-guide/)

See [Ceph](/ceph/) and [ZFS](/zfs/) for the other two storage foundations this stack is built
on, depending on the scale of the deployment.
