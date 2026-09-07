---
title: STIG Hardening
description: 45Drives publishes real STIG security hardening overlays for Rocky Linux — relevant to government and defense customers under CMMC.
---

# Security hardening, published, not just claimed

A [STIG](https://public.cyber.mil/stigs/) (Security Technical Implementation Guide) is a
formal, DoD-published configuration standard for locking down a system — every setting an
auditor will check, documented and testable. 45Drives publishes its own
[STIG overlay repository](https://github.com/45Drives/45drives-stig): specific hardening
profiles for **Rocky Linux 8 and Rocky Linux 9**, layered on top of the baseline STIG using
[InSpec](https://www.chef.io/products/chef-inspec)-based automated compliance checks, plus a
full recorded audit run as evidence.

## Why this matters for a storage server

Government agencies, defense contractors, and regulated industries can't just take a vendor's
word that a system is "secure" — they need an auditable configuration standard and evidence it
was actually applied. This is directly relevant to the CMMC (Cybersecurity Maturity Model
Certification) self-assessment requirements now facing government and defense contractors:
having a published, testable hardening overlay is the kind of artifact a compliance review
actually asks for.

## How 45Drives uses it

The repository contains overlay profiles for both Rocky Linux 8 and 9 — 45Drives' hardening
additions on top of the baseline OS STIG — along with a dated, full audit report showing the
overlay actually being run and evaluated, not just a checklist that's never been executed.

## Related tools worth knowing about

45Drives also tracks [`sedutil`](https://github.com/45Drives/sedutil), an open-source utility
for managing **Self-Encrypting Drives (SEDs)** — hardware-level drive encryption, a common
requirement alongside OS-level hardening in regulated environments. Separately, 45Drives
publishes `45drives-audit-tool`, described in its own package metadata as running "a
collection of system-tuning checks" and writing results to a TSV file — a lighter-weight,
general system-health audit rather than a formal STIG check. Unlike the STIG overlay repo and
`sedutil`, this one doesn't have a public source repository — and unlike most of the
undocumented packages elsewhere on this site, this one genuinely is opaque: extracting the
actual `.deb` shows a stripped, compiled ELF binary with no accompanying source, not a
readable script.

## Why that matters if you're evaluating storage

- **Auditable, not just asserted.** The STIG overlay and its audit evidence are both public —
  nothing to take on faith for the core hardening claim.
- **Built for the standard regulated customers actually have to meet**, not a generic
  "security-hardened" marketing claim.
- **Mostly, but not entirely, open.** The formal STIG tooling is public; a general-purpose
  audit utility alongside it currently isn't — worth knowing which is which if "open source"
  specifically is a requirement, not just "security-focused."

## Learn more

- [`45Drives/45drives-stig` on GitHub](https://github.com/45Drives/45drives-stig)
- [public.cyber.mil/stigs](https://public.cyber.mil/stigs/) — the official DoD STIG program
- [`45Drives/sedutil` on GitHub](https://github.com/45Drives/sedutil)
