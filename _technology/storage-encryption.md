---
title: Storage Encryption & Key Management
description: 45Drives manages LUKS disk encryption keys via OpenBao, the open-source Vault fork — with an optional path to a commercial hardware security module for higher assurance needs.
---

# Encryption at rest, with a real choice of where the keys live

Encrypting a disk is the easy part — Linux's built-in [LUKS](https://gitlab.com/cryptsetup/cryptsetup)
(Linux Unified Key Setup) has handled that well for years. The harder problem is **key
management**: where the decryption key actually lives, how it's protected, and what happens
when a server reboots and needs that key back without a human typing a passphrase in. 45Drives
manages that through Houston UI's `cockpit-storage-encryption` module, backed by a small
cluster of purpose-built tools: `proxmox-kms-bridge` and `vault-dmkey`.

## Where the keys actually live: a real choice

45Drives' own package metadata is specific about this: `vault-dmkey` fetches `dmcrypt` keys
"from OpenBao and QxVault" — two backends, not one, with meaningfully different profiles:

- **[OpenBao](https://openbao.org)** — a genuinely open-source secrets-management platform,
  governed under the Linux Foundation. It exists because HashiCorp re-licensed the original
  Vault project away from an open-source license (MPL 2.0) to the non-open-source Business
  Source License — the same pattern OpenTofu followed after Terraform's relicensing. OpenBao
  is the community's continuation of the original open, MPL-licensed codebase.
- **[QxVault](https://crypto4a.com/products/blade-modules/qx-vault)** — a commercial,
  quantum-safe secrets platform from Crypto4A Technologies, built around a FIPS 140-3 Level 3
  hardware security module (HSM). **This one is not open source** — it's a proprietary
  hardware product for organizations that need certified, hardware-backed key protection
  (defense, regulated industries) beyond what a software-only vault provides.

`proxmox-kms-bridge` specifically wires this key-management layer into **LUKS encryption for
Proxmox VE**, so encrypted VM/container storage can pull its keys from either backend rather
than storing them locally on the hypervisor.

## An honest note on this specific corner of the stack

Unlike most of the technology covered on this site, **the 45Drives-authored integration
packages here — `cockpit-storage-encryption`, `proxmox-kms-bridge`, and `vault-dmkey` — don't
have a public source repository** as of this writing (no matching repo exists in the public
[45Drives GitHub org](https://github.com/orgs/45Drives/repositories)). The underlying
primitives they orchestrate (LUKS, OpenBao) are open source and independently auditable; the
specific glue code tying them into Houston UI currently is not. Worth knowing if "open source"
specifically (not just "works with open source tools") matters for your evaluation of this
particular piece.

## Why that matters if you're evaluating storage

- **A real choice, not a lock-in to one vendor's key vault.** Software-only (OpenBao) or
  hardware-HSM-backed (QxVault) — pick based on what your compliance requirements actually
  demand, rather than being stuck with whatever one backend a vendor bundled.
- **The open-source option exists specifically because a popular tool went proprietary.**
  OpenBao's own origin story is a useful data point on why open licensing matters in
  practice, not just in principle.
- **Verify claims yourself.** This page is a good example of the broader point this whole
  site makes: check what's actually public before taking "open source" at face value —
  including here.

## Learn more

- [openbao.org](https://openbao.org) — the open-source Vault fork
- [Crypto4A QxVault](https://crypto4a.com/products/blade-modules/qx-vault) — the commercial,
  HSM-backed alternative
- [LUKS / cryptsetup](https://gitlab.com/cryptsetup/cryptsetup) — the underlying Linux disk
  encryption layer

See [STIG Hardening](/stig-hardening/) for the other security/compliance-adjacent tooling in
this stack.
