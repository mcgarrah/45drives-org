---
title: Home
description: An independent showcase of the open-source stack behind 45Drives storage — Ceph, ZFS, and the Cockpit-based Houston UI.
---

<div class="hero">
  <h1>45Drives builds on open source, all the way down.</h1>
  <p class="lead">
    Ceph. ZFS. A web dashboard called Houston UI, built on the open-source Cockpit project.
    No proprietary lock-in, no black-box firmware you can't inspect — just well-known,
    widely-used open technology, assembled and supported by a hardware and services company.
    This site is an independent look at that stack: what it is, why it matters, and how to
    try it yourself.
  </p>
  <div class="cta">
    <a class="button primary" href="{{ '/cockpit/' | relative_url }}">See Houston UI</a>
    <a class="button secondary" href="{{ '/build-your-own/' | relative_url }}">Build your own</a>
  </div>
</div>

<div class="disclaimer-banner">
  <strong>This is an independent, community-built site</strong> — not an official 45Drives
  property. It exists because the open-source stack underneath 45Drives' hardware is
  genuinely interesting and, in our opinion, underexplained on 45Drives' own marketing pages.
  See <a href="{{ '/about/' | relative_url }}">About</a> for the full story.
</div>

## Why "open source" is the actual value-add

Most enterprise storage vendors sell you a box and a proprietary control plane you can't see
inside. If the vendor changes strategy, raises prices, or discontinues a product line, you're
stuck. 45Drives' pitch is structurally different: the software doing the real work — the
filesystem, the clustering, the management UI — is open source. You can read the code,
audit it, run it on other hardware if you ever needed to, and lean on an entire upstream
community (not just one vendor) for long-term viability.

What you're actually paying 45Drives for isn't the software — it's the hardware engineering,
the integration work that makes all these open pieces function as one coherent system, and a
support relationship with people who know the stack cold. That's a genuinely different value
proposition than most of the storage industry, and it's worth understanding on its own terms.

<div class="grid">
  <div class="card">
    <h3><a href="{{ '/ceph/' | relative_url }}">Ceph</a></h3>
    <p>The distributed storage engine behind 45Drives' scale-out clusters — self-healing,
    no single point of failure, and entirely open source.</p>
  </div>
  <div class="card">
    <h3><a href="{{ '/zfs/' | relative_url }}">ZFS</a></h3>
    <p>The filesystem behind 45Drives' single-server storage — checksummed, snapshot-capable,
    and built to catch data corruption before you ever notice it.</p>
  </div>
  <div class="card">
    <h3><a href="{{ '/cockpit/' | relative_url }}">Houston UI</a></h3>
    <p>The easiest way to see this stack in action. A web dashboard built on Red Hat's
    open-source Cockpit project, extended by 45Drives' own open-source plugins.</p>
  </div>
</div>
