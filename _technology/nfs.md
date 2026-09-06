---
title: NFS
description: Why 45Drives serves Linux/Unix file sharing through NFS, the original network filesystem protocol.
---

# NFS: the network filesystem Unix has trusted since 1984

[NFS (Network File System)](https://en.wikipedia.org/wiki/Network_File_System) lets a remote
directory appear as if it were local — `mount` a share from a 45Drives storage server and
every Linux, Unix, or macOS client on the network can read and write to it as though it were
on their own disk. The protocol dates back to Sun Microsystems in 1984 and NFSv4, the current
generation, is maintained as an open IETF standard implemented natively in the Linux kernel.

## Why this matters for a storage server

Compute clusters, research workloads, CI/CD pipelines, and Linux desktops all tend to expect
NFS rather than SMB — it's the native, lowest-friction way for Unix-family systems to share a
common filesystem. For workloads like HPC research computing or render farms, NFS is often the
default expectation, not an alternative.

## How 45Drives uses it

NFS is the other protocol managed directly through [Houston UI](/cockpit/)'s
`cockpit-file-sharing` module, alongside Samba — creating an export and scoping it to a subnet
happens from the same dashboard, backed by the standard Linux `nfs-kernel-server`. Because it's
the in-kernel NFS server rather than a userspace reimplementation, performance is close to
local-disk speeds for compatible workloads.

## Why that matters if you're evaluating storage

- **Native to the Linux/Unix world.** No client software to install — `mount` is already
  built into every Unix-like OS.
- **Simple, well-understood security model.** Exports are scoped by network/host, with
  root-squash and read/write controls that are easy to reason about.
- **A standard, not a vendor feature.** NFSv4 is an IETF standard implemented across every
  major operating system, not a proprietary protocol tied to one vendor's hardware.

## Learn more

- [Linux NFS project documentation](https://linux-nfs.org)
- [45Drives' `cockpit-file-sharing` repository](https://github.com/45Drives/cockpit-file-sharing)

See [Build Your Own](/build-your-own/) for a real walkthrough that includes an NFS-specific
gotcha worth knowing before you rely on it: testing a mount from `localhost` behaves
differently than testing from a real client IP, because export ACLs are scoped by network.
