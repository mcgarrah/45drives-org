---
title: 45Flow
description: 45Drives' own open-source file collaboration and video review platform, built for media-heavy workflows.
---

# 45Flow: another 45Drives project given back to the community

Like [`autotier`](/autotier/), [`45Flow`](https://github.com/45Drives/45flow) is a project
45Drives **wrote and open-sourced themselves** (GPL-3.0), not a third-party tool they simply
adopted. It's actively maintained — commits land regularly, not a one-and-done release.

## What it actually does

45Flow is a secure file collaboration and transfer platform built specifically for
media-heavy workflows: an Electron desktop client paired with a Linux server service
(`houston-broadcaster`). It handles expiring, access-controlled share links, upload portals
for external collaborators, client-side video transcoding before upload, and browser-based
video review with frame-accurate, timecoded comments — the kind of workflow a post-production
house or broadcast team needs, not a generic "share a folder" tool.

## Why this matters for a storage server

This isn't an abstract feature — it's aimed squarely at a real customer base. 45Drives'
[public customer spotlights](https://www.45drives.com/community/customer-spotlights/) include
the Toronto International Film Festival, post-production studios, and broadcast/media
companies managing hundreds of terabytes of video. 45Flow is purpose-built for exactly that
workflow: reviewing and moving large media files securely, with the kind of access control and
watermarking a client-facing review process needs.

## Why that matters if you're evaluating storage

- **A second data point that 45Drives contributes, not just consumes.** Combined with
  [`autotier`](/autotier/), this is a real pattern, not a one-off: 45Drives builds and
  open-sources tools to solve problems their own customers actually have.
- **Purpose-built for a specific, real workload** (media review/collaboration) rather than a
  generic file-sharing clone — the kind of thing that comes from actually working with media
  customers, not guessing at requirements.
- **No proprietary lock-in on the collaboration layer**, same as the rest of this stack — the
  server component and protocol are open source and self-hosted.

## Learn more

- [`45Drives/45flow` on GitHub](https://github.com/45Drives/45flow) — source, user guide, and
  releases

See [autotier](/autotier/) for 45Drives' other original open-source contribution.
