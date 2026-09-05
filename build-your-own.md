---
title: Build Your Own
description: A real, documented walkthrough of standing up 45Drives' open-source storage stack yourself — Ceph or ZFS, Samba/NFS/S3, and Houston UI.
permalink: /build-your-own/
---

# Build it yourself

The best way to understand this stack isn't reading a marketing page — it's standing it up.
This site is paired with a real, working, openly-documented project:

<p style="margin: 2rem 0;">
  <a class="button primary" href="https://github.com/mcgarrah/ceph-gateway-45drive">
    github.com/mcgarrah/ceph-gateway-45drive
  </a>
</p>

That project documents building a protocol gateway — a Linux VM that mounts a CephFS pool and
re-exposes it over SMB, NFSv4, and S3, using 45Drives' actual open-source Houston UI stack
(Cockpit + the modules described on the [Houston UI page](/cockpit/)) plus native Ceph RGW for
object storage. It's not a sanitized demo — it's a real build log, including the parts that
didn't work on the first try.

## What's actually in there

- Step-by-step setup: mounting CephFS with scoped credentials, installing the 45Drives Cockpit
  stack, configuring SMB/NFS shares, and standing up an S3-compatible endpoint via Ceph's
  RADOS Gateway.
- A running list of real gotchas found along the way — the kind of thing you only discover by
  actually doing the install, not by reading documentation. A few examples: Samba's registry
  backend (what the Houston UI writes to) and the flat `smb.conf` file are two independent
  config stores that don't talk to each other unless you explicitly wire them together; a
  self-signed TLS certificate needs its file ownership set correctly for the service that
  actually reads it; and a hostname longer than 15 characters will silently break NetBIOS
  naming.
- Security considerations worth thinking about before you expose any share — least-privilege
  Ceph credentials, network-scoped exports, and not pointing a guest-accessible share at more
  than you mean to.

## Why this matters beyond "it's a cool homelab project"

Every one of those findings came from actually running the open-source stack 45Drives builds
its hardware on — which is a pretty direct demonstration of the whole point of this site: it's
*real, runnable, and inspectable*, not a black box you have to take on faith. If something
doesn't work as documented, you can find out why, because the entire stack — Ceph, ZFS,
Cockpit, and 45Drives' own plugins on top — is open source, end to end.

If you build your own and find something worth documenting, the repository is open to
[issues and pull requests](https://github.com/mcgarrah/ceph-gateway-45drive) — that's the
whole idea of an open-source ecosystem.
