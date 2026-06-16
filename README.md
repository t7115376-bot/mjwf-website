# Murarijyoti Welfare Foundation — Website

Static marketing/informational site for **Murarijyoti Welfare Foundation (MJWF)**, a Section 8
non-profit working on preventive and menstrual health across rural Bihar.

**Status:** 🟢 Launch candidate — frozen. Audit passed 10/10. No feature work or redesign pending.
Remaining tasks are **operational only** (see [Go-Live Checklist](#go-live-checklist)).

---

## Stack

Plain static HTML + one shared CSS file. **No build step, no dependencies, no JavaScript framework.**
Inline `<script>` blocks handle only the mobile nav toggle and scroll-reveal animations.
Deploy by serving the folder as-is from any static host (Netlify, Vercel, Cloudflare Pages, GitHub Pages, S3, nginx).

## Structure

```
/
├── index.html          Home
├── about.html          About — origin story + team (6 cards)
├── programs.html       Programmes — 4 programme areas (anchors: #menstrual #cervical #camps #school)
├── partners.html       Partners
├── gallery.html        Gallery — 23 photos, no duplicates
├── get-involved.html   Volunteer / partner / donate routes (anchor: #ways)
├── donate.html         Donation page — UPI QR + 80G tax info
├── contact.html        Contact — email, phone, registered office
├── styles.css          Single shared stylesheet (design tokens at :root)
├── logo.svg            Logo + favicon (referenced as rel="icon")
├── photos/             Site imagery
│   ├── team-*.jpg      5 team portraits
│   ├── upi-qr.png      Donation QR
│   └── gallery/        23 gallery images
└── README.md           This file
```

Every file in the repo is referenced by the live site — there are no orphaned assets.

## Conventions

- **Design tokens** live in `:root` in `styles.css` (colors, fonts, spacing). Change there, not inline.
- **Headings:** exactly one `<h1>` per page; section heads use `.dhead` (class-based, heading-tag-agnostic).
- **Spelling:** British — "Programmes", "programme" — used consistently in UI. Keep it.
- **Images:** all use `loading="lazy"` except above-the-fold heroes; all have descriptive `alt`
  (decorative scrims use `alt=""`). Team portraits are real `<img>` tags.
- **Khushi Kumari's team card is an intentional privacy placeholder** (`.tmember__private`,
  "Photo withheld for privacy") — this is by design, do **not** replace it with a photo.
- **Registration number** `U88900BR2023NPL066704` appears in every footer — keep in sync if it ever changes.

---

## Go-Live Checklist

Operational only. The codebase itself is frozen.

### 1. Connect the production domain
Point the domain at the static host and confirm HTTPS is issued. Decide canonical host
(recommend `https://www.mjwf.org` **or** the apex — pick one and 301-redirect the other).

### 2. Update canonical + Open Graph URLs to the live domain
These are currently **relative** (correct for staging; must become **absolute** for production so
social previews and canonical signals resolve). On each of the 8 pages, update the `<head>`:

| Tag | Now (relative) | After go-live (absolute — replace `DOMAIN`) |
|---|---|---|
| `<link rel="canonical">` | `href="about.html"` | `href="https://DOMAIN/about.html"` |
| `<meta property="og:image">` | `content="photos/IMG_0752.jpeg"` | `content="https://DOMAIN/photos/IMG_0752.jpeg"` |
| add `<meta property="og:url">` | *(absent)* | `content="https://DOMAIN/about.html"` |

> Tip for Claude Code: this is a mechanical find-and-replace across the 8 HTML files —
> prefix every `canonical` href and every `og:image` content with `https://DOMAIN/`, and add a
> matching `og:url`. Per-page `og:image` values are already chosen correctly; just absolutize them.

### 3. Recommended at deploy (nice-to-have, not blocking)
- Add `robots.txt` and a `sitemap.xml` listing the 8 absolute page URLs.
- Add a `twitter:card` = `summary_large_image` meta if Twitter/X previews matter.
- Set long cache headers on `/photos` and `styles.css`; short/no-cache on `*.html`.

### 4. Final real-device check (post-deploy)
On a physical phone over the live domain, confirm on **every page**:
- no horizontal scroll, hamburger nav opens/closes, links work
- donate page UPI QR scans, gallery images load, contact email/phone links work
- social preview renders (paste a URL into a chat app / OG debugger)

---

## Verified before freeze (2026-06-16)

- ✅ No broken image / CSS / JS / link references; all internal anchors resolve
- ✅ 23 gallery images, zero duplicates; no orphaned files
- ✅ 5 team photos present + Khushi privacy card; 100% image `alt` coverage
- ✅ Mobile (375px): 0px horizontal overflow on all 8 pages
- ✅ Accessibility: skip link, visible focus, `lang`, keyboard nav on every page
- ✅ SEO: unique title + description + canonical + OG + favicon + exactly one H1 per page
