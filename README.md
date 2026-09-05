# 45drives-org

Source for [45drives.org](https://45drives.org) — an independent, community-built Jekyll site
showcasing the open-source stack (Ceph, ZFS, Cockpit/Houston UI) that 45Drives builds its
storage hardware and support business on.

**This is not an official 45Drives property.** See the site's [About](/about/) page for the
full disclaimer and context.

## Local development

```sh
bundle install
bundle exec jekyll serve
```

Then visit `http://localhost:4000`.

## Structure

- `index.md` — home page
- `ceph.md`, `zfs.md`, `cockpit.md` — the three technology pages
- `build-your-own.md` — points to the companion hands-on project,
  [ceph-gateway-45drive](https://github.com/mcgarrah/ceph-gateway-45drive)
- `about.md` — independence disclaimer and background
- `CNAME` — GitHub Pages custom domain file (`45drives.org`)

## Status

Not yet published. Built as a draft for review before creating a public GitHub repo, enabling
GitHub Pages, and pointing the `45drives.org` DNS at it.
