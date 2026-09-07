---
title: WireGuard (Wireshield)
description: 45Drives builds encrypted networking on WireGuard, the modern open-source VPN protocol — via a Houston UI module called Wireshield.
---

# WireGuard: the VPN protocol that got adopted, not just praised

[WireGuard](https://www.wireguard.com) is the VPN protocol that broke through where a decade
of alternatives didn't — a fraction of the code size of IPsec or OpenVPN, built with modern
cryptography from the start, and eventually merged directly into the **Linux kernel** itself
(as of 5.6), which is about as strong an open-source endorsement as a networking project can
get.

## How 45Drives uses it

45Drives packages WireGuard-based encrypted network management directly into Houston UI under
the name **Wireshield** — per the package's own description, *"WireGuard-based encrypted
network management for Houston/Cockpit."* Rather than requiring separate VPN configuration
outside the dashboard, encrypted site-to-site or remote-access networking becomes another
managed capability alongside storage, sharing, and monitoring.

## An honest note, same as elsewhere on this site

As with [storage encryption](/storage-encryption/), the `wireshield` integration package
itself doesn't currently have a public source repository in the
[45Drives GitHub org](https://github.com/orgs/45Drives/repositories) — WireGuard the protocol
is open source and in the Linux kernel; the specific Houston UI wrapper around it is not
(publicly) at this time.

## Why that matters if you're evaluating storage

- **Modern, audited cryptography, not a legacy protocol kept alive out of inertia.** WireGuard
  was formally verified and is maintained by a small, focused team rather than carrying decades
  of accumulated protocol cruft.
- **Networking managed from the same dashboard as everything else** — one less separate tool
  to learn and operate for secure remote access to storage infrastructure.
- **Kernel-level performance.** Being merged into Linux itself means WireGuard runs with less
  overhead than userspace VPN implementations.

## Learn more

- [wireguard.com](https://www.wireguard.com) — the official WireGuard project

See [Storage Encryption & Key Management](/storage-encryption/) for the other recently-added
security tooling in this stack.
