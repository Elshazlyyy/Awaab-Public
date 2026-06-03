# Awaab — public site

![Awaab — A quiet companion for the daily prayer](og.png)

Public marketing, support, and legal pages for **Awaab** — a quiet,
offline Islamic companion app for iPhone, iPad, and Android. Qur'an
reader, prayer times, qibla finder. Trilingual (English + Arabic + French).

This repository is the **static website only**. The application itself
lives in a separate, proprietary repository.

The pages here satisfy the support-URL, privacy-policy, accessibility,
and age-suitability requirements for distribution on the Apple App
Store and Google Play.

## Live

| Language | URL |
| --- | --- |
| English | <https://elshazlyyy.github.io/Awaab-Public/> |
| العربية | <https://elshazlyyy.github.io/Awaab-Public/ar/> |
| Français | <https://elshazlyyy.github.io/Awaab-Public/fr/> |

## What's in the repo

```
.
├── awaab.css                  Single source of truth for tokens, type, components
├── apple-touch-icon.png       180×180 home-screen pin (iOS)
├── favicon-16/32/48.png       PNG favicon set (modern browsers)
├── icon-192/512.png           PWA manifest icons
├── og.png                     1200×630 social-share card (English)
├── og-ar.png                  1200×630 social-share card (Arabic, RTL)
├── og-fr.png                  1200×630 social-share card (French, LTR)
├── manifest.webmanifest       PWA manifest for "Add to Home Screen"
├── robots.txt                 Allow all crawlers, points at sitemap.xml
├── sitemap.xml                Fifteen canonical URLs with hreflang alternates
├── .nojekyll                  Tells GitHub Pages to skip Jekyll processing
│
├── index.html                 English home — hero, gallery, features, craft, verse, facts, FAQ, promise
├── privacy.html               English privacy policy
├── accessibility.html         English accessibility statement
├── age-suitability.html       English age-suitability statement
├── licenses.html              English licenses & attribution
├── 404.html                   Not-found page (shared fallback, English copy)
│
├── ar/                        Arabic mirror (right-to-left)
│   ├── index.html
│   ├── privacy.html
│   ├── accessibility.html
│   ├── age-suitability.html
│   └── licenses.html
│
└── fr/                        French mirror (left-to-right)
    ├── index.html
    ├── privacy.html
    ├── accessibility.html
    ├── age-suitability.html
    └── licenses.html
```

## Design system

Tokens mirror the app's `src/theme/colors.ts` and `src/theme/typography.ts`,
canonicalized in `awaab-design/awaab-redesign-v1/spec.md`:

| Token             | Light       | Dark        | Purpose                       |
| ----------------- | ----------- | ----------- | ----------------------------- |
| `bg`              | `#F4EFE3`   | `#0E0F12`   | Canvas                        |
| `surface`         | `#EBE5D6`   | `#16181D`   | Cards, chips, pickers         |
| `surface2`        | `#DFD8C5`   | `#1E2129`   | Pressed, search wells         |
| `ink`             | `#1A1916`   | `#EDE6D2`   | Body & display text           |
| `inkSoft`         | `#4A463E`   | `#A39C87`   | Captions, secondary           |
| `inkFaint`        | `#615C4F`   | `#928C7A`   | Tertiary, eyebrows (WCAG AA)  |
| `hairline`        | `#D9D2BF`   | `#252830`   | 1px dividers                  |
| `hairlineStrong`  | `#C7BFA8`   | `#363A45`   | Stronger separators           |
| `accent`          | `#8A6E3A`   | `#B58447`   | Decorative bronze             |
| `accentText`      | `#76592A`   | `#C9A86A`   | Text bronze — WCAG AA         |

The two-bronze split keeps small-caps bronze legible on cream. `accent`
is decorative (diamonds, dots, ornaments); `accentText` is the
contrast-shifted variant for words — eyebrows, links, captions.

Dark theme is auto-activated via `prefers-color-scheme: dark`. The
full type ramp (displayL 44/52, monoXXL 64/64, eyebrow 10/12 +
0.22em tracking, plus the two Arabic variants — Amiri Quran for
verses, Cairo for UI) lives in `awaab.css`.

Fonts: **Cormorant Garamond** (serif headings, italic-only),
**Inter Tight** (sans body), **Amiri Quran** (Arabic verses only),
**Cairo** (Arabic UI), **JetBrains Mono** (numerals + small caps;
three weights — Light 300 / Regular 400 / Medium 500).

## Local preview

No build step — just open the files. From the repo root:

```bash
# Python (built into macOS)
python3 -m http.server 8000

# Or Node, if you have it
npx serve -p 8000

# Or just double-click index.html (most things work; some paths
# behave slightly differently than under a real server)
```

Then open <http://localhost:8000/>, <http://localhost:8000/ar/>, and <http://localhost:8000/fr/>.

## Deploy

Every push to `main` is auto-deployed by GitHub Pages from the repo
root. No CI, no build, no preview environments. The CDN typically
serves new content within ~30 seconds of push.

To verify a deploy:

```bash
gh api /repos/Elshazlyyy/Awaab-Public/pages/builds/latest --jq '.status'
```

## Quality

Verified at the production URL with Lighthouse (mobile, the harder
target). Site CI re-runs Lighthouse on every PR + push to main via
[`.github/lighthouserc.json`](.github/lighthouserc.json). Accessibility,
Best Practices, and SEO are **hard-gated at 100** — the run fails below
that. Performance is a **warn-only floor at ≥0.70**: Lighthouse mobile
performance swings with CI-runner load and network simulation, so a hard
gate there would only produce false-fail noise.

| | Performance | Accessibility | Best Practices | SEO |
| --- | --- | --- | --- | --- |
| CI gate  | warn ≥70 (advisory) | **error 100** | **error 100** | **error 100** |
| Observed | ≥90 mobile · ≥95 desktop | 100 | 100 | 100 |

Zero failed audits at either viewport. The site is fully responsive
from 320 px (iPhone SE) to wide desktop, supports dark mode via
`prefers-color-scheme`, respects `prefers-reduced-motion`, and ships
**no analytics, no trackers, no third-party scripts, and no
cross-origin font calls**. Fonts (Cormorant Garamond, Inter Tight,
JetBrains Mono, Amiri, Amiri Quran, Cairo) are self-hosted as `woff2`
in [`/fonts`](fonts/) — earlier the site fetched them from
`fonts.googleapis.com` via `@import`, which both leaked visitor IPs
to Google and was render-blocking serial; both are gone. Each page
preloads its three above-the-fold weights with `<link rel="preload"
as="font" crossorigin>` so the hero paints without a FOUT/FOIT flash.

## License

Proprietary. © 2026 Ahmed Elshazly. All rights reserved.

The repository is hosted publicly so that end users and app store
reviewers can access the pages required for the application's
distribution. Public visibility does not grant any rights to copy,
fork, modify, or reuse this repository's contents. See [`LICENSE`](LICENSE)
for the full text.

## Contact

Questions, feedback, or accessibility reports: <ahmed.shazly15@gmail.com>
