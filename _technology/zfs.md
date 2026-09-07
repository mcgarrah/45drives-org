---
title: ZFS
description: Why 45Drives builds single-server storage on ZFS, the open-source filesystem built to never silently lose your data.
permalink: /zfs/
---

# ZFS: a filesystem that assumes hardware lies to it

[ZFS](https://openzfs.org) was originally developed at Sun Microsystems and lives on today as
[OpenZFS](https://openzfs.org), a fully open-source project maintained by a broad community
across Linux, FreeBSD, and other platforms. It was designed around an uncomfortable but
accurate assumption: disks, controllers, and cables occasionally corrupt data silently, and
most filesystems have no way to even notice.

## What it actually does differently

- **Checksums everything.** Every block of data gets a checksum, verified on every read. If a
  disk returns corrupted data, ZFS knows immediately — most filesystems would have no idea.
- **Copy-on-write snapshots.** Snapshots are near-instant and cheap, which makes "restore to
  five minutes ago" a real, fast operation rather than a lengthy restore-from-backup process.
- **Self-healing on redundant pools.** If ZFS detects corruption and has a redundant copy (via
  mirroring or RAID-Z), it repairs the bad copy automatically, using the checksum to know
  which copy was actually correct.
- **No RAID controller required.** ZFS manages redundancy in software, across plain disks —
  no proprietary hardware RAID card, and no controller-specific format you're locked into.

## How 45Drives uses it

ZFS is the filesystem underneath 45Drives' single-server storage systems (the Storinator HDD
and hybrid lines), managed through the open-source `cockpit-zfs-manager` module in Houston UI
— pool creation, dataset management, snapshot scheduling, and health monitoring, all from a
web dashboard rather than the command line (though the command line is always still there if
you want it). For off-box backup and replication, 45Drives also tracks
[`znapzend`](https://github.com/45Drives/znapzend) — an established open-source ZFS
backup/replication tool (originally by [Oetiker+Partner](https://github.com/oetiker/znapzend),
not a 45Drives creation) with remote-target and `mbuffer` support for efficient transfer.

## Why that matters if you're evaluating storage

- **Data integrity you can actually verify.** ZFS's checksumming isn't a marketing claim — the
  mechanism is public, documented, and has been battle-tested for two decades across
  enterprises, research institutions, and hobbyists alike.
- **Recovery that's fast because it's simple.** Snapshot rollback doesn't require restoring
  from a separate backup system — the previous state is already sitting on disk.
- **Runs on hardware you actually own.** No proprietary RAID controller format means your data
  isn't hostage to one company's hardware if you ever need to move it.

## Learn more

- [openzfs.org](https://openzfs.org) — the official OpenZFS project site
- [OpenZFS documentation](https://openzfs.github.io/openzfs-docs/)
- [45Drives' `cockpit-zfs-manager` repository](https://github.com/45Drives/cockpit-zfs-manager)

See [Houston UI](/cockpit/) for what managing ZFS actually looks like day to day, or
[Build Your Own](/build-your-own/) for a hands-on walkthrough.
