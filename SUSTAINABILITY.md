# 🌱 Sustainability: what keeps these pages running

<div class="tldr"><i class="ph ph-plant"></i> <span><strong>TL;DR:</strong> Nothing here costs money. What keeps it alive is a short list of things that quietly expire or drift: one access token, two scheduled jobs, a vendored kit, cached link previews, and a shared wallet-connect project. Each is written down below, with what to do when it needs attention. And today one person holds most of the keys, so there's a plan to change that.</span></div>

Psanctuary Church's GitHub home is two public sites built from three repositories. This page lists what
each one needs, so none of it depends on anyone's memory. It's also the repository's
`SUSTAINABILITY.md`, so it reads the same on GitHub.

## The pieces

| Repository | What it is | How it goes live |
| --- | --- | --- |
| [`psanctuarychurch.github.io`](https://github.com/psanctuarychurch/psanctuarychurch.github.io) (public) | The entry portal: `/`, `/donate/`, `/pledge/`, `/map/`, the 404, and the **arrival kit** every page loads | GitHub Pages, straight from `main` |
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

### 🔍 The ledger's read-only token (`LEDGER_TOKEN`)

The ledger reads the organization's private repositories, shown masked, with a separate read-only
`LEDGER_TOKEN` when one is set, so it never needs the write-capable publishing token.

**Set since 2026-10-26:**
- **The token:** the fine-grained `ledger-read-psanctuarychurch`, resource owner `psanctuarychurch`.
- **Reach:** all of its repositories, with read-only "Contents".
- **No expiration.**

*To replace it:* make a new read-only token the same way. Set it as `LEDGER_TOKEN` on `changelog-astro`
(`pbpaste | gh secret set LEDGER_TOKEN -R psanctuarychurch/changelog-astro`), and run the workflow once. If no
token can read a repository, its masked rows carry over from the committed snapshot.

### 🗓️ The daily ledger rebuild

The [ledger](https://psanctuarychurch.github.io/changelog-astro-public/ledger) lists every public change in plain words. So it stays fresh on days nothing
else changes, the publishing workflow also runs **every day at 10:29 UTC**. It uses a few minutes of
the organization's Actions allowance a day. The workflow builds on **Node 24** (since 2026-10-26): Astro 7.4 needs
Node 22.23.4 or newer, and a bare `22` resolved to 22.23.3.

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
windows, the donate and pledge pages and the map's cards all come from the **arrival kit** (1.4.0):
`arrival.js`, `arrival.css`, `give/give.js` and `pledge/pledge.js`, copied into the portal's
`arrival/` folder, plus this org's own `org.js` (facts and links) and `org.css` (the look). The
ledger's generator (`astro/src/ledger/ledger.mjs`) is vendored the same way. The canonical copy lives in
[`drasticstatic/my-template`](https://github.com/drasticstatic/my-template) under
`changelog-template/arrival/`. Every org keeps its own copy on purpose, so one org's change can't
break another's. The portal's `sporefield.js` is the church's own.

*What to do:* to upgrade, copy the kit files unchanged from the template, read its porting notes, and
push the portal **before** this AstroLog, which loads the kit from the portal. The pledge pins its
signature checker (`@noble/curves@1.9.7`, `@noble/hashes@1.8.0`, from jsDelivr) and the wallet window
is pinned too (`@reown/appkit-cdn@1.8.24`); upgrade those in the kit, not here.

### 🖼️ Cached link previews

Every page has its own share image, built with the site. Telegram, iMessage, Slack, Discord and
LinkedIn keep their own copy of a link's preview for days or weeks, so after a share image changes,
old previews linger. Cards never carry counts, so a cached one doesn't go stale on its own.

*What to do:* after changing a card, refresh the important links: the steps for each app, and the
list of pages with their own card, are in
[`SOCIAL-PREVIEWS.md`](https://github.com/psanctuarychurch/.github/blob/main/SOCIAL-PREVIEWS.md) (for
Telegram, send each link to @WebpageBot).

### 👛 Wallet giving and the pledge

[`/donate/`](https://psanctuarychurch.github.io/donate/) has the kit's wallet module (`arrival/give/give.js`):
one Connect button (Reown AppKit) for browser wallets, phone wallets by QR code, and email or social
sign-in. [`/pledge/`](https://psanctuarychurch.github.io/pledge/) uses the same connection to sign a pledge
of support: a message, not a transaction, so it's free, sends nothing and records nothing, and the
page shows no counts. No wallet address is published anywhere: `give.addresses` stays `null` until
the church designates a real treasury address, never a placeholder, and never one put on a page by
hand.

*What to do:* keep `https://psanctuarychurch.github.io` on the Reown project's allowlist (it is, checked
2026-10-22); without it, both pages calmly say wallet connect isn't switched on for this site.

### 🔌 Wallet connect (Reown project)

Every org's wallet features (giving with crypto, and the pledge of support) connect through **one
shared Reown project today: `00b6a1cd528ff580f1fd845ed08b0187`**, made for the O-R-G dApp and held in
Christopher W's Reown account. Its allowlist lists all five org origins plus
`https://drasticstatic.github.io`. If that project is deleted, suspended or runs past its free-plan
limit, wallet connect stops on every site at once (each page then says so calmly; nothing breaks).

*What to do:* the church gets its own project with a **one-line `org.js` change**: create a free
project at dashboard.reown.com under an account the church controls, add
`https://psanctuarychurch.github.io` to its allowlist, and set `give.reownProjectId` in the portal's
`arrival/org.js` to the new ID (the pledge uses it too). Do it before the church publishes a treasury
address.

### 🧾 Receipts and taxes

The donate page's "Receipts and taxes" section quotes the church's own words and links them: its
[legal notice](https://www.psanctuary.org/legal-notice) ("a lawfully incorporated 508(c)(1)(a) Faith
Based Organization", the page's own summary) and its
[donation page](https://ministries.psanctuary.org/donate) ("All donations to Psanctuary (as a
508(c)(1)(a)) are tax deductible", and its CPA line). Receipts are on request at info@psanctuary.org,
and the kit adds "This isn't tax advice."

*What to do:* if either page changes its wording, change the quote in `donate.tax` (portal,
`arrival/org.js`) to match, word for word; never restate it as more. If the church stops saying it,
remove the line.

### 🔗 Links that leave GitHub

The navbar, the contact and community windows and the [map](https://psanctuarychurch.github.io/changelog-astro-public/map) point at psanctuary.org (its
contact page and legal notice), ministries.psanctuary.org's donation page, the church's member
network on Mighty Networks (one link, for joining and for signing in) and
[`CONTACTS.md`](https://github.com/psanctuarychurch/.github/blob/main/CONTACTS.md).

*What to do:* if one of those addresses changes, update the portal's `arrival/org.js` (one place
for the navbar and windows), `CONTACTS.md` and this AstroLog's map (`src/data/map.ts`).

### 📦 Dependencies

Astro, Satori and sharp are pinned by `package-lock.json`, and the workflow installs exactly those
(`npm ci`). The share-card fonts (Young Serif, Figtree, both OFL) are committed, so a build needs no
font download. Upgrades are deliberate: update, build locally, check the pages and cards, then push.

### 📄 Licenses

Psanctuary Church's GitHub home is licensed: code under MIT (`LICENSE`), written content under CC BY-SA 4.0
(`LICENSE-CONTENT`), and the Psanctuary name and marks belong to the church (`TRADEMARKS.md`), in the
organization's `.github`, the portal and this AstroLog.

*What to do:* keep the three files identical across the repositories; a new public repository gets
the same three.

## 🤝 Stewardship and succession

**Today, one person holds most of the keys.** drasticstatic (Christopher W) owns or administers the
church's GitHub organization, and holds the personal access token that publishes these pages
(`PUBLIC_REPO_TOKEN`, and a `LEDGER_TOKEN` if one is set) and the Reown account above. That's a
single point of failure: if he stepped away, the token would expire on schedule and the AstroLog
would quietly stop updating, with nobody else able to renew it. The plan, so these pages outlive any
one person, Christopher included:

1. **At least two owners for the GitHub organization**, from the church's own people, each with
   two-factor sign-in. Owners can do everything; everyone else gets the narrowest role that works.
2. **Org-owned automation, not personal tokens.** Publishing moves to a GitHub App installed on the
   organization (short-lived tokens, scoped to the repositories it needs), or failing that a
   dedicated bot account whose credentials the owners share. No workflow should depend on one
   person's account.
3. **A token register**: every token, key and secret the sites use, with what it does, which secret
   holds it, who owns it, its scopes and its expiry date, kept where every owner can read it (never
   the values themselves).
4. **A renewal runbook**: for each item in the register, how to renew it, step by step, and a
   reminder a month before each expiry date.
5. **Documented hand-over steps**: how to add an owner, transfer a repository, transfer the Reown
   project (or create the church's own), and remove someone who's leaving.
6. **Exports and backups**: the repositories (with their issues and history) cloned or exported
   somewhere the church controls, on a schedule; the same for what lives outside GitHub (the member
   network's admin roles, the church's own sites and forms).

Progress on each step is on the
[roadmap](https://psanctuarychurch.github.io/changelog-astro-public/roadmap).

## Who looks after it

Christopher W, with Alfred (Claude Code), until the steps above are done. Questions go through
[Reach out](https://github.com/psanctuarychurch/.github/blob/main/CONTACTS.md).

*Last reviewed 2026-10-22.*
