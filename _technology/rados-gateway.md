---
title: S3 / RADOS Gateway
description: Why 45Drives offers S3-compatible object storage through Ceph's RADOS Gateway, without a cloud subscription.
---

# S3-compatible storage, running on hardware you own

Modern applications increasingly expect **S3** — the object storage API Amazon popularized
with AWS — as their storage interface, not a traditional file share. [Ceph](/ceph/) makes
that available on-prem through its **RADOS Gateway (RGW)**: an open-source component of Ceph
that speaks the S3 (and Swift) API natively, backed by the same distributed, self-healing
storage cluster described on the Ceph page.

## Why this matters for a storage server

An application written against the S3 API — a backup tool, a CI artifact store, a data
pipeline — doesn't need to know or care whether it's talking to AWS or to a 45Drives cluster
in your own datacenter. RGW gives you that portability: the same API, the same client
libraries and tools (including the official AWS CLI), running entirely on infrastructure you
control, with no per-request cloud egress or storage fees.

## How 45Drives uses it

RGW runs as a service against a Ceph cluster built on 45Drives hardware — either alongside
the cluster or on a dedicated gateway node. [Houston UI](/cockpit/)'s `cockpit-file-sharing`
module lists Ceph RGW as a supported S3 backend for bucket and user management, alongside
Samba and NFS in the same dashboard.

## A real client tool in the same repository

45Drives' own package repository also carries [`rclone`](https://rclone.org) — the
widely-used, genuinely popular open-source "rsync for cloud storage" — right alongside the
RGW-related packages. It's not a 45Drives creation, just a real, well-regarded third-party
tool bundled for convenience, and a practical way to actually move data into or out of an RGW
bucket from the command line once it's running.

## Why that matters if you're evaluating storage

- **No cloud lock-in for S3-native tooling.** If your backup software, data pipeline, or CI
  system already speaks S3, RGW lets you point it at your own hardware instead of a monthly
  cloud bill.
- **No egress fees.** Object storage in the cloud gets expensive fast once you're moving data
  out of it regularly — RGW eliminates that cost entirely.
- **Same open-source guarantees as the rest of the stack.** RGW is part of Ceph itself, not a
  bolted-on proprietary gateway — auditable, community-maintained, and not tied to one vendor.

## Learn more

- [Ceph RADOS Gateway documentation](https://docs.ceph.com/en/latest/radosgw/)
- [45Drives' `cockpit-ceph-deploy` repository](https://github.com/45Drives/cockpit-ceph-deploy)

See [Build Your Own](/build-your-own/) for a real, documented RGW deployment — including
setting up scoped credentials, verifying the service with the actual AWS CLI, and adding TLS
with a self-signed certificate.
