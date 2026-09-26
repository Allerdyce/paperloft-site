# paperloft-site

The website for **Paperloft** at <https://paperloft.app>: a suite of Mac apps for paperwork, made by EvidencePair LLC. The first app, Paperloft Receipts, is **not released yet**.

The site has three jobs, in this order:

1. Pass Apple's organization enrollment check for EvidencePair LLC. That needs a real, working public site on a domain the organization owns.
2. Serve the App Store URLs for Paperloft Receipts: privacy policy at `/privacy/` and support at `/support/`.
3. Be the suite's home page for the long term, with almost no upkeep.

## Layout

```text
CNAME                     exactly "paperloft.app" (GitHub Pages custom domain; don't remove)
.nojekyll                 empty; tells GitHub Pages to serve files as they are
index.html                /           home page for the suite
receipts/index.html       /receipts/  Paperloft Receipts
privacy/index.html        /privacy/   privacy policy (App Store privacy URL)
support/index.html        /support/   support + FAQ (App Store support URL)
404.html                  served by GitHub Pages for any missing path
assets/site.css           the only stylesheet (colors, light and dark mode)
assets/paperloft-icon-512.png   icon, also the Open Graph image
assets/paperloft-icon-256.png   icon shown on pages
apple-touch-icon.png      180 px
favicon-32.png            32 px
robots.txt                allow all, points to the sitemap
sitemap.xml               the four public pages
```

Every page is a complete HTML file with the same header and footer. There are no templates or includes, so a change to the header or footer has to be made in all five HTML files. All internal links and asset paths are root-absolute (`/assets/site.css`, `/privacy/`) so they also work from `404.html` at any depth.

## Rules

These are the rules for this site. Anyone editing it, human or agent, should keep to them.

**Stack**
- Plain HTML and CSS. No build step, no framework, no `package.json`, no Jekyll, no JavaScript.
- Hosting: GitHub Pages, deploying from `main` at `/ (root)`. Domain `paperloft.app`, which is HTTPS-only (`.app` is on the HSTS preload list).
- Nothing third party: no analytics, trackers, cookies, web fonts, CDNs, embeds or forms. The privacy policy says so, and the site must match it. Outbound *links* are fine (the privacy policy links to Apple and GitHub); nothing may *load* from another domain.
- Clean URLs: one folder per page with an `index.html`.
- Images: resize with macOS `sips` only.

**Honesty (these override everything else)**
- Until Paperloft Receipts is actually live, it is "coming soon". Don't say or imply it's available, don't show a "Download on the Mac App Store" badge, and don't link to an App Store page.
- No invented facts: no user counts, reviews, testimonials, press quotes, awards, ratings, customer logos, or screenshots of an app that doesn't exist yet.
- No prices. "Free to start" is fine.
- Legal details are limited to: EvidencePair LLC, California, and support@paperloft.app. No address, phone number or registration numbers.
- Don't name future apps beyond "more Paperloft apps for your paperwork are on the way."
- Illustrations must be clearly illustrations (like the HTML/CSS before-and-after on `/receipts/`), never fake app-window screenshots.

**Design**
- The home page adds an owner-directed soft leaf-green accent (`#C9DFA8`) and neutral surfaces. Core colors from the icon: deep green `#083920`, tray green `#10462A`, line green `#104A2E`, paper cream `#F8F1E0`, near-black `#1A1F1C`. All are defined as CSS variables at the top of `assets/site.css`, with the contrast ratios noted. Every text pairing must meet WCAG AA.
- Light and dark mode follow `prefers-color-scheme`.
- System font stack only, tabular numbers for figures.
- Product-led home page up to 1160 px wide, with a soft leaf-green hero, bold system typography, clearly labeled workflow illustrations, feature sections, and native FAQ disclosures. Text and support pages remain about 720 px wide. Must work at 375 px with no sideways scrolling.
- Each page's `<head>` has a title, meta description, canonical URL on `https://paperloft.app/...`, Open Graph title/description/image, and `theme-color` `#083920`.

## Preview locally

```sh
python3 -m http.server 8000
```

Then open <http://localhost:8000/>. Check each page at 375 px and 1280 px wide, in light and dark mode.

Before pushing, confirm nothing loads from another domain. The only `http` matches should be canonical/Open Graph URLs on `paperloft.app`, the two policy links in `privacy/index.html`, and the sitemap namespace:

```sh
grep -rn "http" --include="*.html" --include="*.css" .
```

## Who edits what

| What | Who | Where |
| --- | --- | --- |
| Real app screenshots | Codex (final build phase) | Add a new section to `receipts/index.html` with images in `assets/`. Keep the before-and-after illustration or replace it; don't mix real screenshots with drawn illustrations in one figure. Resize with `sips`. |
| Final privacy wording | Codex, then reviewed by Ali | `privacy/index.html`. Check every statement against the shipped app, update "Last updated" (text and `datetime`), and remove the draft comment at the top once reviewed. |
| App Store link | Codex, **only once the app is live** | `receipts/index.html` (the eyebrow line and the status line at the bottom), the Receipts card on `index.html`, and the first FAQ answer on `support/index.html`. Replace "Coming soon" wording in all of them together. A badge is fine then, but it must be a local file in `assets/`, not hot-linked. |
| New Paperloft apps | Ali decides | Add a card on `index.html`, a folder like `/receipts/`, a header nav link in every page, and an entry in `sitemap.xml`. |
| Colors, layout, type | Anyone, rarely | `assets/site.css` only. |
| `CNAME`, `.nojekyll` | Nobody | Leave as they are. |

After any change, update `<lastmod>` in `sitemap.xml` for the pages you touched.

## Icon

The source icon is `app-icon-source.png` (1254×1254), kept outside this repo. To regenerate the sized copies:

```sh
sips -s format png -Z 512 app-icon-source.png --out assets/paperloft-icon-512.png
sips -s format png -Z 256 app-icon-source.png --out assets/paperloft-icon-256.png
sips -s format png -Z 180 app-icon-source.png --out apple-touch-icon.png
sips -s format png -Z 32  app-icon-source.png --out favicon-32.png
```

## GitHub Pages settings

- Source: Deploy from a branch, `main`, `/ (root)`.
- Custom domain: `paperloft.app`.
- **Enforce HTTPS: on**, once GitHub has issued the certificate.
