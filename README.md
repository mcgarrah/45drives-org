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

Built to scale from today's 6 pages toward 10-20+ without a rewrite:

- `index.md` — home page
- `technology.md` + `_technology/` — the **Technology collection**. `technology.md` is an
  auto-generated index page (loops over `site.technology`); each file in `_technology/`
  (`ceph.md`, `zfs.md`, `cockpit.md` today) is one page. **To add a new technology page**
  (Samba, NFS, S3/RGW, monitoring, etc.), just add a new `.md` file to `_technology/` with
  `title`/`description` front matter — it appears in the Technology index and nav dropdown
  automatically, no other changes needed.
- `_data/nav.yml` — site navigation, grouped by section. A top-level entry can point at a
  collection (`collection: technology`) for an auto-populated dropdown, or a static
  `children:` list for sections not backed by a collection. **Add new top-level sections
  here** as they're created (e.g. a future "Community" section).
- `build-your-own.md` — points to the companion hands-on project,
  [ceph-gateway-45drive](https://github.com/mcgarrah/ceph-gateway-45drive)
- `about.md` — independence disclaimer and background
- `404.html`, `robots.txt` — standard site plumbing
- `CNAME` — GitHub Pages custom domain file (`45drives.org`)

### Plugins

- `jekyll-sitemap`, `jekyll-seo-tag` — standard SEO plumbing
- `jekyll-redirect-from` — installed proactively for when pages get restructured/renamed as
  the site grows; add `redirect_from: [/old-path/]` to a page's front matter when that
  happens, so old links/search results don't 404

### CI

`.github/workflows/jekyll.yml` builds via GitHub Actions (not "deploy from a branch") so any
Jekyll plugin can be used, not just the GitHub-Pages-gem whitelist. Every push runs
`jekyll doctor` and HTML Proofer (broken link/image check) before deploying. `.github/dependabot.yml`
keeps bundler and Actions dependencies current.

## Status

Live at [45drives.org](https://45drives.org) (GitHub Pages, custom domain via Porkbun DNS).
