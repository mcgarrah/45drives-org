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

| Distribution | Codename | Version |
| --- | --- | --- |
| Ubuntu | `focal` | 20.04 LTS |
| Ubuntu | `jammy` | 22.04 LTS |
| Ubuntu | `noble` | 24.04 LTS |
| Debian | `bookworm` | Debian 12 (also the base of Proxmox VE 8) |
| Debian | `trixie` | Debian 13 (also the base of Proxmox VE 9) |

## RHEL family (YUM/DNF)

| Distribution | Codename | Version |
| --- | --- | --- |
| RHEL / Rocky Linux / AlmaLinux / CentOS Stream | `el7` | Version 7 |
| RHEL / Rocky Linux / AlmaLinux / CentOS Stream | `el8` | Version 8 |
| RHEL / Rocky Linux / AlmaLinux / CentOS Stream | `el9` | Version 9 |

RHEL-compatible builds are served from a single `rocky/` path — the package builds are
identical across RHEL, Rocky Linux, AlmaLinux, and CentOS Stream, so there's no separate
per-distribution build to choose between.

## Get the repository set up

45Drives publishes a setup script that configures the correct repository and signing key for
your distribution automatically:

```bash
curl -sSL https://repo.45drives.com/setup | sudo bash
```

See the [official 45Drives knowledge base article](https://knowledgebase.45drives.com/kb/kb45035-updating-to-newest-45drives-repositories/)
for the full details, or [Build Your Own](/build-your-own/) for this exact step used in a real,
documented deployment.

## What's not covered

No Fedora, openSUSE, or Alpine builds exist in this repository as of this writing. `el7`'s
repository metadata is noticeably older than the others — worth verifying it still receives
updates before relying on it for anything new.

## A note on accuracy

Repository contents can change. This page reflects what's actually published at
[repo.45drives.com/enterprise/](https://repo.45drives.com/enterprise/) as of the last time it
was checked — if something here looks wrong, that's the place to verify directly.
