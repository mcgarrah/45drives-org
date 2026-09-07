---
title: iSCSI
description: 45Drives' iSCSI support runs on SCST, the open-source Linux SCSI target subsystem — including a clustered, Ceph-backed HA mode.
---

# iSCSI: block storage over the network, open source underneath

[iSCSI](https://en.wikipedia.org/wiki/ISCSI) presents remote storage as if it were a local
block device — a raw disk a client can partition, format, and use exactly like directly
attached storage, just delivered over the network instead of a cable. It's the standard way
to give a hypervisor, a database server, or any workload that expects "a disk" (rather than a
file share) access to centralized storage.

## What runs underneath

45Drives' iSCSI implementation is built on [**SCST**](http://scst.sourceforge.net/) (the
Generic SCSI Target Subsystem for Linux) — confirmed directly in
[`cockpit-file-sharing`'s source](https://github.com/45Drives/cockpit-file-sharing), which
manages SCST targets through `/sys/kernel/scst_tgt/`. That's a deliberate, specific choice,
not a generic wrapper around whatever the kernel ships by default.

## What the Houston UI panel actually does

Through [Houston UI](/cockpit/)'s `cockpit-file-sharing` module: creating and editing virtual
devices (both BlockIO and FileIO-backed), managing targets, portals, and initiator groups,
CHAP authentication, and viewing active sessions — all from the browser.

The more interesting part is **clustered mode**. A separate driver in the same plugin
integrates **Pacemaker/Corosync** (cluster resource management) with **Ceph RBD** (RADOS
Block Device) and LVM to deliver **highly-available iSCSI targets backed by a Ceph cluster** —
not just a single-server iSCSI target, but block storage that survives a node failure, built
entirely from open-source cluster tooling.

## How 45Drives uses it

iSCSI is productized directly: **Storinator SAN** is 45Drives' single-server iSCSI
configuration, described on their own solutions page as offering *"read/write speeds over
3 GB/s"* for moving large amounts of data fast. The same underlying SCST/Houston UI tooling
also scales up to the clustered, Ceph-backed HA configuration described above — automated via
the [`iscsi-ansible`](https://github.com/45Drives/iscsi-ansible) playbooks, which deploy SCST
iSCSI and are actively maintained.

## Why that matters if you're evaluating storage

- **A named, auditable target implementation.** "iSCSI support" on a spec sheet could mean
  anything; SCST is a specific, inspectable open-source project, not a black box.
- **Block storage doesn't require giving up high availability.** The clustered driver means an
  iSCSI target can survive a node failure the same way the rest of this stack does, rather
  than being a single point of failure bolted onto an otherwise resilient cluster.
- **Same open-source guarantees, same management surface.** iSCSI configuration lives in the
  same Houston UI dashboard as Samba, NFS, and S3 — one place to manage every protocol this
  stack speaks.

## Learn more

- [SCST project](http://scst.sourceforge.net/) — the target subsystem this is built on
- [`45Drives/cockpit-file-sharing`](https://github.com/45Drives/cockpit-file-sharing) — includes
  both the single-server and clustered iSCSI drivers
- [`45Drives/iscsi-ansible`](https://github.com/45Drives/iscsi-ansible) — automated SCST iSCSI
  deployment
- [45Drives solutions page](https://www.45drives.com/solutions/) — the Storinator SAN
  description

See [Ceph](/ceph/) for the storage layer the clustered iSCSI mode is built on, or
[Samba](/samba/), [NFS](/nfs/), and [S3 / RADOS Gateway](/rados-gateway/) for the other three
protocols managed from the same Houston UI dashboard.
