# Psanctuary Church — Changelog & Library

> A running, public-safe record of what's built for Psanctuary Church on GitHub, plus a library of the
> longer write-ups behind it.

[![License: MIT](https://img.shields.io/badge/license-MIT-lightgrey?style=flat)](LICENSE)
[![Live Site](https://img.shields.io/badge/%F0%9F%8C%90%20Live-changelog--astro--public-E2B45A)](https://psanctuarychurch.github.io/changelog-astro-public/)
[![Built with Claude Code](https://img.shields.io/badge/Built%20with-Claude%20Code%20CLI-blueviolet)](https://code.claude.com/docs/en/overview)

---

<strong><i class="ph ph-door-open"></i> <a href="https://psanctuarychurch.github.io/changelog-astro-public/">Changelog &amp; Library Astro Porch</a></strong>

<a href="https://psanctuarychurch.github.io/"><i class="ph ph-house-simple"></i> Entry portal</a> ·
<a href="https://psanctuarychurch.github.io/changelog-astro-public/changelog"><i class="ph ph-clock-counter-clockwise"></i> Changelog</a> ·
<a href="https://psanctuarychurch.github.io/changelog-astro-public/library"><i class="ph ph-books"></i> Library</a> ·
<a href="https://www.psanctuary.org"><i class="ph ph-globe-simple"></i> psanctuary.org</a>

---

## 👋 What this is

A plain-language, dated record of the technical work done for [Psanctuary](https://www.psanctuary.org),
a sacred mushroom church, on GitHub, plus a `/library` of longer write-ups for anyone curious how
it's built. It's written for someone outside the work who cares about it, not for the people already
inside it.

The church's real home is [psanctuary.org](https://www.psanctuary.org). This is the quieter,
technical side. This README is also the live site's homepage.

## 🗂️ What's here

- `changelog/`: one markdown file per entry, named `YYYY-MM-DD-short-slug.md`.
- `library/`: longer, standalone write-ups that don't belong to one entry.
- `astro/`: the Astro site itself.
- `.github/workflows/sync-public.yml`: on every push to `main`, builds the site and publishes only
  the finished pages (never this source) to
  [`psanctuarychurch/changelog-astro-public`](https://github.com/psanctuarychurch/changelog-astro-public).

Same pattern as the PIR® and Holy Earth Foundation changelogs.

## ✍️ Adding an entry

New file in `changelog/`, named `YYYY-MM-DD-short-slug.md`:

```yaml
---
title: "Plain-language title"
date: "YYYY-MM-DD"
priority: "P2-medium" # P1-high, P2-medium (default), or P3-low
repo: "github" # which project card it belongs to
---
```

Open with a one-line TL;DR, then put the full story inside a `<details>` block. If something can't be
described publicly (member details, ceremonies, legal matters), it doesn't get an entry.

## 💻 Local development

```sh
sh scripts/install-hooks.sh   # once per clone
cd astro
npm install
npm run dev
```

---

*Maintained by [psanctuarychurch](https://github.com/psanctuarychurch) with Anthropic's Claude Code CLI.*
