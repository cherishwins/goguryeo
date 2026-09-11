# 한민족 조선민족, han-minjok.org

A single-page static site. Two names for one people: 남에서는 한민족, 북에서는 조선민족.

No build step and no framework. One HTML file carries all markup, CSS and inline
SVG artwork; the only other runtime requests are the subset font, the icons and
the analytics tag.

## Files

| File | Purpose |
| --- | --- |
| `index.html` | The whole site: markup, CSS, inline SVG ridgeline and glyph diagrams |
| `404.html` | Matching not-found page. Vercel serves it for unmatched paths with no config |
| `hm-serif.woff2` | Noto Serif KR SemiBold, subset to the glyphs used in serif elements |
| `og.png` | 1200×630 link-preview card |
| `favicon.svg` | The 천지인 mark, with a dark-scheme swap |
| `favicon.ico`, `apple-touch-icon.png` | Legacy and iOS home-screen icons |
| `icon-192.png`, `icon-512.png`, `icon-maskable-512.png` | PWA icons named by `site.webmanifest` |
| `site.webmanifest` | Install metadata |
| `robots.txt`, `sitemap.xml`, `llms.txt` | Crawler and AI-agent files |
| `vercel.json` | Cache-Control: font immutable for a year, OG image for a day |

## Local preview

```sh
python3 -m http.server 8000
```

Open http://localhost:8000. Use a server rather than opening the file directly:
assets are referenced by absolute root paths (`/hm-serif.woff2`), which do not
resolve over `file://`.

## Deploy

Vercel, zero config. Import the repository at https://vercel.com/new and accept
the defaults:

- Framework preset: **Other**
- Build command: *(none)*
- Output directory: *(leave empty, the repository root is the site)*

`vercel.json` is picked up automatically. Pushes to the default branch redeploy;
other branches get preview URLs.

### Custom domain

`index.html` declares absolute canonical URLs on `han-minjok.org`, and so do
`robots.txt`, `sitemap.xml` and `llms.txt`. Social scrapers fetch `og:image`
from that host, not from the deployment they were handed, so until the domain is
attached in Vercel (Project → Settings → Domains) and resolving, link previews
on the `*.vercel.app` URL render without an image.

## Submitting the sitemap

`sitemap.xml` lists the one canonical URL and is referenced from `robots.txt`,
so any crawler that reads robots finds it unprompted. Submit it directly to
speed up first indexing. Each needs domain ownership verified once, usually by a
DNS TXT record or by an HTML meta tag in `index.html`.

| Where | URL | Notes |
| --- | --- | --- |
| Google | https://search.google.com/search-console | Add property, then Sitemaps → `sitemap.xml` |
| Bing | https://www.bing.com/webmasters | Can import the Google property wholesale |
| Yandex | https://webmaster.yandex.com | Sitemap files section |
| Naver | https://searchadvisor.naver.com | Worth it here: Korean-language search |
| Daum | https://register.search.daum.net/index.daum | Korean, separate index from Naver |

Bing, Yandex and Seznam also accept **IndexNow**, which pushes a URL the moment
it changes instead of waiting for a crawl. It needs a key file at the site root
and a single GET to submit. Not wired up: say the word and it is a key file plus
four lines of documentation.

`llms.txt` states the licence as `[set by the author before launch]`. Worth
settling before you announce the site, since it is the line an AI agent quotes.

## Analytics

Umami, loaded deferred from `cloud.umami.is` in `index.html`. Cookieless, so no
consent banner is required for it. It is the only third-party request the page
makes and the only JavaScript on it, and it reverts in one commit if you would
rather the page stayed inert.

## Footer signature slot

The footer has a commented `<img>` ready for the rose mark, beside
"Made by Jesse James". `.credit img` already carries the alignment, so dropping
the file in and uncommenting one line is the whole job.

- **Format:** WebP, 40px tall on screen, supplied at 80px tall for 2x
- **Weight:** 6 KB or less
- **Attributes:** explicit `width` and `height` (set `width` to the real
  intrinsic ratio at 40px tall) so it cannot shift layout; `alt=""` because the
  credit line beside it already carries the meaning; `decoding="async"`

## Changing serif Korean text

`hm-serif.woff2` contains only the glyphs used inside serif elements (`h1`,
`h2`, `.sf`, `.seal`, `.coda`, `figcaption`). Adding a Korean character to any
of those without regenerating the subset makes it fall back to a different
serif, silently, on the one character. Regenerate from Noto Serif KR SemiBold
(SIL OFL, Google Fonts):

```sh
pyftsubset NotoSerifKR-SemiBold.otf --text-file=chars.txt --flavor=woff2 \
  --layout-features='*' --output-file=hm-serif.woff2
```

`chars.txt` must list every character used in serif elements, plus digits and
basic punctuation.

The 404 heading (`길을 잃었다`) contains three glyphs the subset does not carry,
so that page sets its heading in a system myeongjo on purpose rather than
regenerating the font for one error page.
