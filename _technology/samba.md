---
title: Samba
description: Why 45Drives serves Windows/macOS file sharing through Samba, the open-source implementation of the SMB protocol.
---

# Samba: open-source SMB, without the Windows Server tax

[Samba](https://www.samba.org) is the open-source implementation of Microsoft's SMB/CIFS
protocol — the same protocol Windows file sharing has used for decades. It's been developed
in the open since 1992 and is the reason a Linux server can show up in a Windows "Network"
browser, serve authenticated shares, and even act as an Active Directory domain controller,
all without a Windows Server license.

## Why this matters for a storage server

Most organizations still run Windows or macOS on the desktop, even when their storage backend
is Linux. Samba is the bridge: it lets 45Drives' Linux-based storage servers present shares
that Windows and Mac clients mount and use exactly like a native file server, with real
user/group permissions, no client-side software required.

## How 45Drives uses it

Samba is one of the two protocols managed directly through
[Houston UI](/cockpit/)'s `cockpit-file-sharing` module — creating a share, setting access
control, and managing users happens from the web dashboard rather than hand-editing
`smb.conf`. It's also architecturally significant for [SnapShield](https://www.45drives.com/software/snapshield-ransomware-protection/):
the ransomware-detection product hooks into Samba's **VFS (Virtual File System) module**
layer, meaning it inspects file activity as it passes through Samba itself, independent of
whether the underlying filesystem is ZFS or Ceph.

## Why that matters if you're evaluating storage

- **No per-client licensing.** Samba shares are free to stand up and free to scale — no
  Windows Server CAL math.
- **Battle-tested at real enterprise scale.** Samba has been powering mixed Windows/Linux
  environments for over three decades; it's not a hobbyist reimplementation.
- **Auditable.** Like everything else in this stack, the code is public — if you need to know
  exactly how authentication or permissions are enforced, it's there to read.

## Learn more

- [samba.org](https://www.samba.org) — the official Samba project
- [45Drives' `cockpit-file-sharing` repository](https://github.com/45Drives/cockpit-file-sharing)

See [Build Your Own](/build-your-own/) for a real walkthrough of configuring Samba shares
through Houston UI — including a couple of real gotchas around Samba's dual config-backend
system that the guide found the hard way.
