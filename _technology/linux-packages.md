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

## What's not covered

No Fedora, openSUSE, or Alpine builds exist in this repository as of this writing.

## A note on accuracy

Repository contents and OS release/EOL status both change over time. This page reflects what
was actually published at [repo.45drives.com/enterprise/](https://repo.45drives.com/enterprise/)
and each distribution's real lifecycle status as of the last time it was checked — if
something here looks wrong or out of date, that's the place to verify directly.
