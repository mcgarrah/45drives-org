---
title: Ceph
description: Why 45Drives builds scale-out storage on Ceph, the open-source distributed storage system.
permalink: /ceph/
---

# Ceph: storage that heals itself

[Ceph](https://ceph.io) is an open-source, distributed storage system originally created by
Sage Weil and developed as a community project under the Ceph Foundation (part of the Linux
Foundation) since. It's used by some of the largest storage deployments in the world, and it's
entirely free and open to inspect, modify, and run yourself.

## What makes it different from a traditional storage array

A conventional storage array is one (or two, for redundancy) controller managing a shelf of
disks — if the controller fails or the vendor stops supporting it, you have a real problem.
Ceph works differently: it spreads data across many nodes and many disks, using algorithms
(CRUSH) to decide where copies live, so there's no single controller to fail. Lose a disk, a
node, even a whole rack, and a healthy Ceph cluster keeps serving data and automatically
rebuilds the missing copies elsewhere — all without anyone racing to swap hardware before a
second failure happens.

## How 45Drives uses it

45Drives builds Ceph clusters on its Storinator storage servers, and provides open-source
tooling (`ceph-ansible` playbooks and a Cockpit deployment module) to make standing up and
managing a cluster more approachable than wrestling with Ceph's configuration by hand. The
result is multi-node, self-healing, scale-out storage — the kind of architecture usually
associated with hyperscale cloud providers — built from hardware you own and software anyone
can read the source code for.

## Why that matters if you're evaluating storage

- **No vendor lock-in on the software.** Ceph runs on 45Drives hardware, but it isn't tied to
  it — the same open-source project runs at massive scale elsewhere, which means the skills,
  documentation, and community troubleshooting you'll find apply broadly, not just to one
  vendor's product.
- **Auditable, not a black box.** Every line of Ceph is public. If you need to know exactly how
  your data is placed, replicated, or recovered, you can go read it — not take a vendor's word
  for it.
- **A real community, not just a support contract.** Bugs get found and fixed by a large,
  active upstream project, not just whatever engineering team one company can afford to staff.

## Learn more

- [ceph.io](https://ceph.io) — the official Ceph project site
- [Ceph documentation](https://docs.ceph.com)
- [45Drives' `ceph-ansible` repository](https://github.com/45Drives/ceph-ansible)
- [45Drives' `cockpit-ceph-deploy` repository](https://github.com/45Drives/cockpit-ceph-deploy)

See [Houston UI](/cockpit/) for what managing a Ceph cluster actually looks like day to day,
or [Build Your Own](/build-your-own/) for a hands-on walkthrough of standing one up yourself.
