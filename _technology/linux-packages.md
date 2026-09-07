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
Several packages — `45drives-audit-tool`, `cockpit-alerts`, `cockpit-storage-encryption`,
`cockpit-super-simple-setup`, `proxmox-kms-bridge`, `vault-dmkey`, `wireshield`, and the
branding/hardware-variant packages — currently have no matching GitHub repo. But "no repo"
turned out not to mean "unreadable": actually extracting several of these `.deb` files
directly shows plain, readable JavaScript, Python, Perl, and shell scripts inside — no formal
version history or stated license, but not a black box either. **Only two, checked directly,
turned out to be genuinely compiled and unreadable**: `vault-dmkey` and `45drives-audit-tool`,
both real ELF binaries. See [Storage Encryption & Key Management](/storage-encryption/),
[WireGuard](/wireguard/), and [STIG Hardening](/stig-hardening/) for the specifics on each.

*(`45drives-tools` was initially miscategorized as having no public repo — see the full
mapping below for the correction.)*

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

## Mapping every package to where its source actually lives

Combining both audits and checking every distinct package (across both Debian and RPM) against
the [45Drives GitHub org](https://github.com/orgs/45Drives/repositories) — including, this
time, reading fork metadata and actual repo contents, not just matching names — sorts cleanly
into four groups.

**1. Original 45Drives code, with its own public repository:**

| Package(s) | Repository |
| --- | --- |
| `cockpit-2fa` | [`cockpit-2FA`](https://github.com/45Drives/cockpit-2FA) |
| `cockpit-benchmark` | [`cockpit-benchmark`](https://github.com/45Drives/cockpit-benchmark) |
| `cockpit-file-sharing` | [`cockpit-file-sharing`](https://github.com/45Drives/cockpit-file-sharing) |
| `cockpit-identities` | [`cockpit-identities`](https://github.com/45Drives/cockpit-identities) |
| `cockpit-navigator` | [`cockpit-navigator`](https://github.com/45Drives/cockpit-navigator) |
| `cockpit-scheduler` | [`cockpit-scheduler`](https://github.com/45Drives/cockpit-scheduler) |
| `cockpit-zfs` | [`cockpit-zfs`](https://github.com/45Drives/cockpit-zfs) |
| `cockpit-45drives-hardware` | [`cockpit-hardware`](https://github.com/45Drives/cockpit-hardware) |
| `45drives-tools` | [`tools`](https://github.com/45Drives/tools) — its own `manifest.json` names itself `45drives-tools`, GPL-3.0+ |
| `cockpit-s3-browser` | [`cockpit-S3ObjectBroswer`](https://github.com/45Drives/cockpit-S3ObjectBroswer) (repo name has a typo; its README correctly says `cockpit-s3-browser`) |
| `haproxy-ansible`, `iscsi-ansible`, `nfs-ansible`, `samba-ansible` | matching repos of the same name |
| `serial45d`, `python3-libzfs` | matching repos of the same name |

**2. Built from a 45Drives *fork* of an upstream project** — publicly viewable, including
45Drives' own changes, but not original 45Drives IP:

| Package(s) | Fork | Upstream |
| --- | --- | --- |
| `cockpit`, `cockpit-bridge`, `cockpit-ws`, `cockpit-system`, `cockpit-storaged`, `cockpit-packagekit`, `cockpit-doc`, `cockpit-tests` | [`45Drives/cockpit`](https://github.com/45Drives/cockpit) | [`cockpit-project/cockpit`](https://github.com/cockpit-project/cockpit) |
| `cephfs-shell`, `cockpit-ceph` | [`45Drives/ceph`](https://github.com/45Drives/ceph) | [`ceph/ceph`](https://github.com/ceph/ceph) |
| `ceph-ansible2` | presumably [`45Drives/ceph-ansible`](https://github.com/45Drives/ceph-ansible) | [`ceph/ceph-ansible`](https://github.com/ceph/ceph-ansible) — though no branch actually named "2" was found; the newer internal rewrite this package describes may not be pushed publicly yet |

**3. Pure upstream repackaging** — correctly has no 45Drives repository, because the real
source lives at the upstream project's own home, not because it's hidden:

| Package family | Real upstream home |
| --- | --- |
| `samba`, `samba-client`, `samba-common`, `samba-libs`, `samba-winbind`, `python3-samba`, `libtalloc`, `libtdb`, `libtevent`, `libwbclient`, `libnetapi`, `libldb`/`python3-ldb`, `ctdb`, `samba-vfs-cephfs`, `samba-vfs-iouring` | [`samba-team/samba`](https://github.com/samba-team/samba) |
| `zfs`, `zfs-dkms`, `zfs-dracut`, `libzfs5`, `libnvpair3`, `libuutil3`, `libzpool5`, `python3-pyzfs` | [`openzfs/zfs`](https://github.com/openzfs/zfs) |
| `scst-dkms`, `scstadmin` | [scst.sourceforge.net](http://scst.sourceforge.net/) (predates GitHub-centric hosting) |
| `rclone` | [`rclone/rclone`](https://github.com/rclone/rclone) |
| `smartmontools` | [smartmontools.org](https://www.smartmontools.org) |
| `mpi3mr-dkms` | mainlined directly in the Linux kernel itself |
| `python3-google-auth-oauthlib` | Google's own OAuth library, just a dependency |

**4. No public source found anywhere** — checked against the GitHub org, and where relevant
against the specific upstream project's own tree:

`45drives-audit-tool`, `vault-dmkey` (both confirmed compiled ELF binaries), `proxmox-kms-bridge`,
`wireshield`, `cockpit-alerts`, `cockpit-storage-encryption`, `cockpit-super-simple-setup`,
`cockpit-45drives-branding`, `cockpit-45drives-audit`, `houston-broadcaster`, and
`samba-vfs-snapshield` (a compiled `.so` with no matching source in Samba's own tree either —
see [Samba](/samba/)). `niccli`, a Broadcom NIC-management CLI, is a hardware vendor's own tool
and was never expected to be open regardless.

The honest summary: most of what makes this stack *distinctive* — the Houston UI modules, the
Ansible automation, the storage tooling — really is open, and the handful of exceptions are
consistently small, single-purpose utilities, not core functionality.

## What's not covered

No Fedora, openSUSE, or Alpine builds exist in this repository as of this writing.

## A note on accuracy

Repository contents and OS release/EOL status both change over time. This page reflects what
was actually published at [repo.45drives.com/enterprise/](https://repo.45drives.com/enterprise/)
and each distribution's real lifecycle status as of the last time it was checked — if
something here looks wrong or out of date, that's the place to verify directly.
