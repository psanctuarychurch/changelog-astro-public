# 🌱 Sustainability: what keeps these pages running

<div class="tldr"><i class="ph ph-plant"></i> <span><strong>TL;DR:</strong> Nothing here costs money. What keeps it alive is a short list of things that quietly expire or drift: one access token, two scheduled jobs, a vendored kit, cached link previews, and (later) a wallet allowlist. Each is written down below, with what to do when it needs attention.</span></div>

Psanctuary Church's GitHub home is two public sites built from three repositories. This page lists what
each one needs, so none of it depends on anyone's memory. It's also the repository's
`SUSTAINABILITY.md`, so it reads the same on GitHub.

## The pieces

| Repository | What it is | How it goes live |
| --- | --- | --- |
| [`psanctuarychurch.github.io`](https://github.com/psanctuarychurch/psanctuarychurch.github.io) (public) | The entry portal: `/`, `/donate/`, `/map/`, the 404, and the **arrival kit** every page loads | GitHub Pages, straight from `main` |
| `changelog-astro` (private) | The source of this AstroLog: entries, write-ups, the Astro site | A workflow builds it and publishes **only the finished pages** |
| [`changelog-astro-public`](https://github.com/psanctuarychurch/changelog-astro-public) (public) | The built AstroLog you're reading | GitHub Pages, from what the workflow pushes |

## What can quietly stop, and what to do

### 🔑 The publishing token (`PUBLIC_REPO_TOKEN`)

The AstroLog is published by a workflow in the private source repository, which pushes the finished
pages into `changelog-astro-public` with a **classic personal access token** stored as the secret
`PUBLIC_REPO_TOKEN`. Classic tokens expire. When it does, **the site stops updating without any
visible sign on the site itself**: new entries, the daily ledger and every fix simply stop arriving,
and the pages keep showing the last good build. The same token also lets the ledger read the
private repositories it shows masked; without it, those rows carry over from the last snapshot.

*What to do:* before the expiry date (GitHub → Settings → Developer settings → Personal access
tokens), make a new classic token with the `repo` and `workflow` scopes, paste it into
`changelog-astro` → Settings → Secrets → Actions → `PUBLIC_REPO_TOKEN`, then run the workflow by
hand once (Actions → Sync Public Changelog → Run workflow) and check this page's footer date.

### 🗓️ The daily ledger rebuild

The [ledger](https://psanctuarychurch.github.io/changelog-astro-public/ledger) lists every public change in plain words. So it stays fresh on days nothing
else changes, the publishing workflow also runs **every day at 10:29 UTC**. It uses a few minutes of
the organization's Actions allowance a day.

*What to do:* if the ledger's newest day looks old, open the workflow's run history. A failed
scheduled run usually means the token above, or GitHub's API having a bad moment (the build then
falls back to a committed snapshot and still publishes).

### 🪞 The hourly README mirror

The portal's `README.md` is a copy of the organization profile (`psanctuarychurch/.github`), refreshed
**every hour** by a workflow in the portal repository. GitHub switches off scheduled workflows in a
public repository after 60 days without activity.

*What to do:* edit the profile, never the portal's README. If the copy stops following, re-enable
the "Mirror org profile README" workflow in the portal's Actions tab.

### 🧰 The arrival kit is vendored

The navbar, the light and motion controls, search, the welcome guide, the contact and community
windows, the donate page and the map's cards all come from the **arrival kit**: `arrival.js` and
`arrival.css`, copied into the portal's `arrival/` folder, plus this org's own `org.js` (facts and
links) and `org.css` (the look). The canonical copy lives in
[`drasticstatic/my-template`](https://github.com/drasticstatic/my-template) under
`changelog-template/arrival/`. Every org keeps its own copy on purpose, so one org's change can't
break another's.

*What to do:* to upgrade, copy the two kit files unchanged from the template, read its porting
notes, and push the portal **before** this AstroLog, which loads the kit from the portal.

### 🖼️ Cached link previews

Every page has its own share image, built with the site. Telegram, iMessage, Slack, Discord and
LinkedIn keep their own copy of a link's preview for days or weeks, so after a share image changes,
old previews linger. Cards never carry counts, so a cached one doesn't go stale on its own.

*What to do:* after changing a card, refresh the important links (for Telegram, send each link to
@WebpageBot; LinkedIn and Facebook have their own inspectors).

### 👛 Wallet giving, later

The "give with crypto" slot on [`/donate/`](https://psanctuarychurch.github.io/donate/) is empty on
purpose. The wallet-connect module that will fill it (Reown AppKit, with Web3Auth for email
sign-in) only loads on domains its projects allow.

*What to do:* when it's switched on, add `https://psanctuarychurch.github.io` to the Reown project's
domain allowlist and to Web3Auth's, with a real client ID (never a demo one).

### 🔗 Links that leave GitHub

The navbar, the contact and community windows and the [map](https://psanctuarychurch.github.io/changelog-astro-public/map) point at psanctuary.org and the
church's member network on Mighty Networks.

*What to do:* if one of those addresses changes, update the portal's `arrival/org.js` (one place
for the navbar and windows) and this AstroLog's map (`src/data/map.ts`).

### 📦 Dependencies

Astro, Satori and sharp are pinned by `package-lock.json`, and the workflow installs exactly those
(`npm ci`). The share-card fonts (Young Serif, Figtree, both OFL) are committed, so a build needs no
font download. Upgrades are deliberate: update, build locally, check the pages and cards, then push.

## Who looks after it

Christopher W, with Alfred (Claude Code). Questions go through [Reach out](https://www.psanctuary.org/contact).

*Last reviewed 2026-10-13.*
