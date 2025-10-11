---
title: MoonPocket Static Site
version: v1.0.1
updated: 2025-10-03
owner: moonpacket-core
---

Production-ready Astro + TypeScript + Tailwind v4 static site. 34 locales (incl. RTL), SEO-first with SSR metric fallbacks and client enhancement. No GitHub Actions required.

## Install
```bash
pnpm i
pnpm dev
```

## Build
```bash
# default: ./dist
pnpm build
# or output to /docs for GitHub Pages main branch
OUT_DIR=docs pnpm build
```

## Publish (No Actions)
- Option A: gh-pages branch
```bash
pnpm build
git subtree push --prefix dist origin gh-pages
```
- Option B: /docs on main
```bash
OUT_DIR=docs pnpm build
# commit ./docs and push main
```

## Configure SITE/base
Update `SITE` in `astro.config.mjs` to your GitHub Pages URL, e.g. `https://<user>.github.io/<repo>`. Internal links honor `BASE_URL` automatically.

## Live Metrics API
- SSR fallback from `public/data/placeholders.json`
- Client island `src/islands/LiveMetrics.tsx` fetches `data-api-src` every 60s
- Swap to your API by replacing `data-api-src` on each `MetricCard`

## Brand Assets
- Put OG image at `public/images/og-default.png`
- Logo SVG at `public/icons/logo.svg`

## Tests
- Unit: `pnpm test:unit`
- E2E: `pnpm build && pnpm preview & wait-on http://localhost:4321 && pnpm test:e2e`

## See also
- Contributing: `CONTRIBUTING.md`
- Code style: `CODESTYLE.md` (aligns with `.prettierrc`, `.eslint.config.mjs`)
- UI guidelines: `UI-GUIDELINES.md`
- Directory & naming: `DIRECTORY-NAMING.md`
- Tests overview: `tests/README.md`
- Metrics data shape: `public/data/README.md`
- i18n rule: No hardcoded copy (incl. SEO title/description) — read from `messages.*` only

## Backup & Private Repo (Safe by design)
- Policy: `docs/PUBLICATION-POLICY.md` (v1.3.0)
- Operations (backup/push/publish) run ONLY when explicitly instructed by the owner.

### Quick Commands
```bash
# Quick local backup (264MB, seconds) ⭐
pnpm run backup:simple

# Quick private repo backup (one-way push) ⭐
pnpm run backup:private-simple

# Full backup with git history
pnpm run backup:safe

# Push to private repo (may be slow)
pnpm run backup:moonpocket

# Deploy to GitHub Pages
pnpm run publish:public
```

### Advanced: Use "git bundle + temp clone + dry-run" to push to private repo without touching the working tree.
```bash
cd /Users/yichen/Downloads/cursor/moonpocket
mkdir -p backups
ts=$(date +%Y%m%d-%H%M%S)
bundle="backups/moonpocket-$ts.bundle"
git bundle create "$bundle" --all --tags && git bundle verify "$bundle"
tmpdir=$(mktemp -d)
git clone "$bundle" "$tmpdir/moonpocket-publish"
cd "$tmpdir/moonpocket-publish"
git remote add private git@github.com:devcrealize/moonpacket.git
git ls-remote --heads private && git fetch private --prune
git push --dry-run private --all && git push --dry-run private --tags
git push private --all && git push private --tags
```
Config hardening (in your main repo):
```bash
git config --local pull.ff only
git config --local fetch.prune false
```
