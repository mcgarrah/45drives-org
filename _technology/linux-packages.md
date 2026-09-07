---
title: Linux Package Repository
description: 45Drives publishes real Debian and RHEL-family package repositories — here's exactly what's supported, checked directly against the live repo.
---

# The actual package repository, not just marketing copy

Everything on this site describes open-source technology — but open source only matters if
you can actually install it. 45Drives publishes real APT and YUM/DNF repositories at
[repo.45drives.com](https://repo.45drives.com), covering the Houston UI/Cockpit stack across
multiple Linux distributions. This page lists exactly what's currently supported, checked
directly against the live repository directory structure rather than assumed from
documentation.

## Debian family (APT)

| Distribution | Codename | Version | Status |
| --- | --- | --- | --- |
| Ubuntu | `focal` | 20.04 LTS | ⚠️ Past general EOL — standard support ended May 2025, now ESM-only (paid Ubuntu Pro) |
| Ubuntu | `jammy` | 22.04 LTS | Supported |
| Ubuntu | `noble` | 24.04 LTS | Supported |
| Debian | `bookworm` | Debian 12 (also the base of Proxmox VE 8) | Supported |
| Debian | `trixie` | Debian 13 (also the base of Proxmox VE 9) | Supported — current stable Debian release |

## RHEL family (YUM/DNF)

| Distribution | Codename | Version | Status |
| --- | --- | --- | --- |
| RHEL / Rocky Linux / AlmaLinux / CentOS Stream | `el7` | Version 7 | ⚠️ Past general EOL — maintenance ended June 2024, now ELS-only (paid extended support) |
| RHEL / Rocky Linux / AlmaLinux / CentOS Stream | `el8` | Version 8 | Supported |
| RHEL / Rocky Linux / AlmaLinux / CentOS Stream | `el9` | Version 9 | Supported |

RHEL-compatible builds are served from a single `rocky/` path — the package builds are
identical across RHEL, Rocky Linux, AlmaLinux, and CentOS Stream, so there's no separate
per-distribution build to choose between.

## Get the repository set up

45Drives publishes a setup script — [repo.45drives.com/setup](https://repo.45drives.com/setup) —
that detects your distribution and configures the correct repository and signing key
automatically:

```bash
curl -sSL https://repo.45drives.com/setup | sudo bash
```

You can also browse the repository directly at
[repo.45drives.com](https://repo.45drives.com) if you'd rather inspect what's there before
trusting a `curl | sudo bash` one-liner — a reasonable instinct, and the whole reason this
page links the raw directory structure above instead of just describing it.

See the [official 45Drives knowledge base article](https://knowledgebase.45drives.com/kb/kb45035-updating-to-newest-45drives-repositories/)
for the full details, or [Build Your Own](/build-your-own/) for this exact step used in a real,
documented deployment.

## How current is this, really?

Not fully. Checked directly against each distribution's own release history:

- **Debian is fully current** — trixie (Debian 13) is the actual current stable release, not
  a lagging one.
- **Ubuntu is about one LTS behind.** Ubuntu 26.04 LTS shipped in April 2026 and isn't in this
  repository yet — the newest supported release is still 24.04.
- **RHEL-family is about one major version behind.** RHEL 10, Rocky Linux 10, and AlmaLinux 10
  all shipped in 2025; this repository still tops out at el9.
- **Both families are still serving an EOL'd release.** `focal` (Ubuntu 20.04) and `el7`
  passed their general end-of-life dates in 2025 and 2024 respectively — both are now in
  paid-extended-support-only territory upstream, yet still listed here as a build target.

None of this means the software doesn't work — it means don't assume "in the repo" equals
"on the newest release," and don't build new production deployments on `focal` or `el7`
without knowing you're relying on an EOL'd base OS.

## What's actually inside the repository — and a real find worth mentioning

Reading the trixie repository's own package list directly (not just the module table on the
[Houston UI page](/cockpit/)) turned up 22 distinct packages, including one worth calling out:
[`cockpit-super-simple-setup`](https://repo.45drives.com/enterprise/debian/dists/trixie/main/binary-amd64/Packages),
described in its own metadata as *"a cockpit module for simplifying 45Drives server setup."*
If you've read the [Build Your Own](/build-your-own/) page, you'll know standing up this stack
from scratch is real, documented friction — a purpose-built setup-simplification module is a
direct, encouraging response to that problem, whatever its current adoption looks like.

**Not everything in this "open source" repository has a public repository, though** — checked
package-by-package against the [45Drives GitHub org](https://github.com/orgs/45Drives/repositories).
Several packages — `45drives-audit-tool`, `45drives-tools`, `cockpit-alerts`,
`cockpit-storage-encryption`, `cockpit-super-simple-setup`, `proxmox-kms-bridge`,
`vault-dmkey`, `wireshield`, and the branding/hardware-variant packages — currently have no
matching GitHub repo. But "no repo" turned out not to mean "unreadable": actually extracting
several of these `.deb` files directly shows plain, readable JavaScript, Python, Perl, and
shell scripts inside — no formal version history or stated license, but not a black box
either. **Only two, checked directly, turned out to be genuinely compiled and unreadable**:
`vault-dmkey` and `45drives-audit-tool`, both real ELF binaries. See
[Storage Encryption & Key Management](/storage-encryption/), [WireGuard](/wireguard/), and
[STIG Hardening](/stig-hardening/) for the specifics on each.

## Cross-referencing the RPM repository against the Debian one

The Debian audit above only tells half the story. Pulling the same data from the RHEL-family
side — parsing `repodata/primary.xml` from `el9/stable` the same way the Debian `Packages`
file was parsed — turns up **150 distinct packages**, nearly seven times the 22 found in
trixie. That gap is worth explaining rather than just noting, because it's not a sign the RPM
build is more actively developed — it's structural:

- **RHEL-family distributions don't ship ZFS in their own repositories at all**, because ZFS's
  CDDL license is considered incompatible with the GPL-licensed Linux kernel by Red Hat and the
  major RHEL derivatives. Debian and Ubuntu, by contrast, already carry current native ZFS,
  Ceph, and Samba packages (via `contrib`/`main`) that 45Drives' Debian build can simply depend
  on. The RPM build has no equivalent to lean on, so 45Drives packages the entire stack itself
  — ZFS, Ceph, Samba (including its Active Directory/DC components), and their dependencies —
  directly in its own repository.
- Cross-referencing package **names** confirms this: of Debian trixie's 22 packages, all but
  two — `cockpit-45drives-branding` and `proxmox-kms-bridge` — also exist on el9. (The
  `proxmox-kms-bridge` gap makes sense on its own: Proxmox VE is Debian-based, so there's no
  RHEL build to ship it for.) The 45Drives-authored package set is essentially the same across
  both families; el9 is simply carrying a lot more upstream software alongside it.

**New, real finds surfaced only by checking the RPM side directly:**

- **`samba-vfs-snapshield`** — described in its own metadata as *"Samba VFS module for
  Snapshield integration."* Direct confirmation, from the package repository itself, of how
  [SnapShield](/samba/) actually integrates with Samba. Extracting the actual package shows a
  stripped, compiled `.so` with no matching source in either the 45Drives GitHub org or Samba's
  own upstream tree — genuinely closed, unlike its neighbors below. See
  [Honeypot-Based Ransomware Detection](/ransomware-samba-tools/) for more.
- **`samba-vfs-cephfs`** and **`samba-vfs-iouring`** — additional Samba VFS modules for direct
  CephFS integration and `io_uring`-based async I/O. Also compiled `.so` files, but checked
  directly against `samba-team/samba` on GitHub — both `vfs_ceph.c` and `vfs_io_uring.c` exist
  in Samba's own public source tree, so these two genuinely are open. See [Samba](/samba/).
- **`ctdb`** — clustered Samba, the same high-availability story [iSCSI](/iscsi/) already tells
  with its Pacemaker/Corosync mode. See [Samba](/samba/).
- **`scst-dkms` and `scstadmin`** — independent confirmation of the [SCST](/iscsi/) subsystem
  claim already made on the iSCSI page, this time straight from the kernel-module package
  itself rather than inferred from `cockpit-file-sharing`'s source.
- **`rclone`** — a real, well-known open-source cloud-storage sync tool, bundled for
  convenience alongside the [S3 / RADOS Gateway](/rados-gateway/) tooling. Not a 45Drives
  creation, just a genuinely useful inclusion.
- A previously-undocumented **`stable`/`testing` channel split** exists under each RHEL version
  directory (e.g. `el9/stable/`, `el9/testing/`) — this page and its setup instructions track
  the `stable` channel, which is also what the official setup script defaults to.

**One assumption this check disproved:** it would be reasonable to guess the RPM build lags
the Debian build in package versions, the same way the OS versions themselves lag (see below).
Checked directly, that's not true — `repodata/primary.xml` retains every historically-published
build (unlike Debian's `Packages` file, which only exposes the current one), and comparing the
*newest* el9 build against trixie shows them landing on the same versions: `cockpit-alerts`
is `4.0.25` on both, `45drives-audit-tool` is `2.2.7` on both. The RPM repository's metadata
just happens to expose more history, not less currency.

## What's not covered

No Fedora, openSUSE, or Alpine builds exist in this repository as of this writing.

## A note on accuracy

Repository contents and OS release/EOL status both change over time. This page reflects what
was actually published at [repo.45drives.com/enterprise/](https://repo.45drives.com/enterprise/)
and each distribution's real lifecycle status as of the last time it was checked — if
something here looks wrong or out of date, that's the place to verify directly.
