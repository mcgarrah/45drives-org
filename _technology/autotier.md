---
title: autotier
description: 45Drives doesn't just build on open source — autotier is their own open-source contribution back to the community.
---

# autotier: 45Drives' own contribution back to open source

Every other page in this section covers open-source projects 45Drives *builds on* — Ceph,
ZFS, Samba, NFS, Cockpit. [`autotier`](https://github.com/45Drives/autotier) is different:
it's a project 45Drives **wrote and open-sourced themselves**, released under the GPL for
anyone to use, audit, or contribute to — not just customers.

## What it actually does

`autotier` is a FUSE (Filesystem in Userspace) passthrough filesystem that automatically
moves files between storage tiers based on how often and how recently they're accessed. Point
it at a fast tier (SSD/NVMe) and a slower, cheaper tier (spinning HDD), and it presents a
single unified mount point — hot files migrate to the fast tier automatically, cold files sink
to the cheap tier, without anyone manually deciding where anything lives.

## Why that's worth building yourself instead of buying

Storage tiering is normally a feature enterprise storage vendors charge a premium for, bundled
into proprietary arrays as a differentiator. 45Drives' approach was to solve the same problem
as an independent, standalone, open-source tool — usable with any Linux storage setup, not
gated behind a specific 45Drives hardware purchase, and inspectable by anyone who wants to
verify exactly how the tiering decisions get made.

## Why this matters beyond the tool itself

This is the clearest evidence on this site that 45Drives' open-source commitment isn't just
"we chose free software to cut costs" — it's a two-way relationship. They consume Ceph, ZFS,
Samba, Cockpit, and Proxmox from their respective communities, and they give back tools of
their own under the same open terms — [`45Flow`](/45flow/) is the other clear example. That's
a meaningfully different posture than a vendor who only *uses* open source internally while
keeping their own value-add proprietary.

## A related approach worth knowing about

`autotier` moves whole files between tiers based on access patterns. A different technique —
block-level caching, where a fast tier transparently accelerates a slower one without moving
files wholesale — is handled by [Open CAS](https://github.com/Open-CAS/open-cas-linux), which
45Drives also tracks via an [Ansible deployment role](https://github.com/45Drives/open-cas-ansible)
(Open CAS itself is an Intel-originated project, not a 45Drives creation).

## Learn more

- [`45Drives/autotier` on GitHub](https://github.com/45Drives/autotier) — source, issues, and
  releases, open for anyone to use or contribute to

See [Ceph](/ceph/) and [ZFS](/zfs/) for the storage foundations `autotier` typically sits on
top of, or [45Flow](/45flow/) for 45Drives' other original open-source project.
