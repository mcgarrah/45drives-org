---
title: Houston UI
description: 45Drives' Houston UI is a web dashboard built on the open-source Cockpit project — the easiest way to see the open-source storage stack in action.
permalink: /cockpit/
---

# Houston UI: the easiest way to see it working

If Ceph and ZFS are the engine, **Houston UI** is the dashboard — and it's the fastest way to
actually *see* what "open source storage" means, rather than just read about it. Houston UI is
45Drives' name for a set of modules built on [Cockpit](https://cockpit-project.org), Red
Hat's open-source web-based server admin console, extended with 45Drives' own open-source
plugins for storage-specific tasks.

<div class="screenshot-placeholder">
  Screenshot: Houston UI / Cockpit dashboard overview<br>
  <em>(placeholder — add a real screenshot from your own deployment)</em>
</div>

## What you actually get in the dashboard

- **File and block sharing** — configure SMB, NFS, [iSCSI](/iscsi/), and (on supported
  backends) S3 shares from a browser, without hand-editing `smb.conf` or `/etc/exports`.
- **ZFS management** — create pools, manage datasets, schedule snapshots, and watch pool
  health, all visually.
- **Ceph deployment** — bring up single-node or multi-node Ceph clusters through a guided
  Ansible-backed workflow instead of raw `ceph` CLI commands.
- **User and identity management, hardware health, benchmarking** — day-to-day server
  administration without needing to live in a terminal.

## It's a real, actively-maintained open-source project

This isn't a proprietary UI 45Drives built once and stopped touching — it's a family of
public GitHub repositories, actively maintained, that anyone can inspect, fork, or contribute
to:

| Module | What it does |
| --- | --- |
| [`cockpit-file-sharing`](https://github.com/45Drives/cockpit-file-sharing) | Manages Samba, NFS, iSCSI, and (for supported backends like Ceph RGW, MinIO) S3 shares |
| [`cockpit-navigator`](https://github.com/45Drives/cockpit-navigator) | A full-featured web file browser |
| [`cockpit-zfs-manager`](https://github.com/45Drives/cockpit-zfs-manager) | Interactive ZFS pool/dataset administration |
| [`cockpit-identities`](https://github.com/45Drives/cockpit-identities) | User and group management |
| [`cockpit-hardware`](https://github.com/45Drives/cockpit-hardware) | Hardware health monitoring for 45Drives storage servers |
| [`cockpit-benchmark`](https://github.com/45Drives/cockpit-benchmark) | Built-in storage benchmarking |
| [`cockpit-ceph-deploy`](https://github.com/45Drives/cockpit-ceph-deploy) | Guided Ceph cluster deployment via Ansible |
| [`cockpit-scheduler`](https://github.com/45Drives/cockpit-scheduler) | All-in-one task scheduling module |
| [`cockpit-2FA`](https://github.com/45Drives/cockpit-2FA) | Two-factor authentication for Houston UI logins |
| [`cockpit-autotier-status`](https://github.com/45Drives/cockpit-autotier-status) | Displays live status for [`autotier`](/autotier/) |
| [`houston-common`](https://github.com/45Drives/houston-common) | The shared library the other modules are built on — actively updated, not a one-off |

## Why the "quick and easy show" matters

Distributed storage and copy-on-write filesystems are genuinely complex under the hood — that
complexity is exactly *why* the industry has historically sold them wrapped in expensive,
proprietary management consoles you had to trust blindly. Houston UI makes the case that you
don't have to trade transparency for usability: the dashboard is friendly enough for day-to-day
administration, and every module behind it is public source you (or anyone) can read.

## See it yourself

Nothing here beats seeing it live. Head to [Build Your Own](/build-your-own/) for a real,
documented walkthrough of standing up this exact stack — Ceph or ZFS, Samba/NFS/S3, and
Houston UI on top — using 45Drives' own open-source tooling.
