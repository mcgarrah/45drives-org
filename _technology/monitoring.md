---
title: Monitoring (Prometheus, Grafana, Alertmanager)
description: Why 45Drives builds storage monitoring on Prometheus, Grafana, and Alertmanager instead of a proprietary dashboard.
---

# Knowing what your storage is doing, in the open

Storage that fails silently is worse than storage that fails loudly. 45Drives builds
monitoring on the same open-source stack most of the infrastructure world already trusts:
[Prometheus](https://prometheus.io) for metrics collection, [Grafana](https://grafana.com)
for dashboards and visualization, and [Alertmanager](https://prometheus.io/docs/alerting/latest/alertmanager/)
for routing alerts to the people who need to see them.

## Why this matters for a storage server

A proprietary monitoring dashboard locks your operational visibility to one vendor's UI and
one vendor's alerting rules. Prometheus/Grafana/Alertmanager is the de facto standard
observability stack across the rest of the infrastructure world — the same tools monitoring
your storage servers likely already watch your applications, your network, and everything
else in a modern operations team's stack. One set of dashboards, one alerting pipeline, one
skill set to maintain, instead of a separate silo just for storage.

## How 45Drives uses it

45Drives publishes a set of open-source [Ansible roles](https://github.com/45Drives/monitoring-stack)
that deploy Prometheus, Alertmanager, and Grafana together, pre-wired to collect metrics from
[Ceph](/ceph/) and [ZFS](/zfs/) storage nodes. Rather than building a proprietary dashboard
from scratch, 45Drives automated the deployment of tools the broader industry already
standardized on.

There's also a lighter-weight `cockpit-alerts` module built directly into [Houston UI](/cockpit/)
for surfacing alerts inside the dashboard itself — worth noting this one doesn't currently
have a public source repository, unlike the Ansible roles and exporters below.

That's backed by purpose-built Prometheus exporters 45Drives publishes for the parts of the
stack that don't have off-the-shelf metrics already: [`zfs_exporter`](https://github.com/45Drives/zfs_exporter),
[`autotier_exporter`](https://github.com/45Drives/autotier_exporter) for [autotier](/autotier/)
tiering activity, [`radosgw_usage_exporter`](https://github.com/45Drives/radosgw_usage_exporter)
for [Ceph RGW/S3](/rados-gateway/) usage, and [`cephgeorep_exporter`](https://github.com/45Drives/cephgeorep_exporter)
for CephFS remote-backup status.

## Why that matters if you're evaluating storage

- **No dashboard lock-in.** Metrics live in Prometheus's standard format — queryable,
  exportable, and usable with any tool that speaks that ecosystem, not just 45Drives' own UI.
- **One observability stack for everything**, not a separate proprietary tool just for
  storage — if your team already runs Prometheus/Grafana elsewhere, storage metrics slot
  right in.
- **Alerting you control.** Alertmanager routes to whatever you already use — email, Slack,
  PagerDuty, webhook — rather than being limited to whatever notification channels one
  vendor's product happens to support.

## Learn more

- [prometheus.io](https://prometheus.io) — the official Prometheus project
- [grafana.com](https://grafana.com) — the official Grafana project
- [45Drives' `monitoring-stack` repository](https://github.com/45Drives/monitoring-stack)

See [Ceph](/ceph/) and [ZFS](/zfs/) for the storage layers this monitoring stack is built to
watch.
