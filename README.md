# 한민족 조선민족 — han-minjok.org

A single-page static site. Two names for one people: 남에서는 한민족, 북에서는 조선민족.

No build step, no framework, no JavaScript. One HTML file, one font, one social image.

## Files

| File | Purpose |
| --- | --- |
| `index.html` | The entire site — markup, CSS, and inline SVG artwork |
| `hm-serif.woff2` | Subset display serif, preloaded and `font-display: swap` |
| `og.png` | 1200×630 Open Graph / Twitter card image |
| `vercel.json` | Cache-Control headers for the font (immutable, 1y) and the OG image (1d) |

## Local preview

```sh
python3 -m http.server 8000
```

Then open http://localhost:8000. Use a server rather than opening the file
directly — the font and OG image are referenced by absolute root paths
(`/hm-serif.woff2`), which do not resolve over `file://`.

## Deploy

Vercel, zero config. Import this repository at
https://vercel.com/new and accept the defaults:

- Framework preset: **Other**
- Build command: *(none)*
- Output directory: *(leave empty — the repository root is the site)*

Vercel picks up `vercel.json` automatically and serves `index.html` at `/`.
Every push to the default branch redeploys; other branches get preview URLs.

From the CLI instead:

```sh
npx vercel --prod
```

### Custom domain

`index.html` declares absolute canonical URLs on `han-minjok.org`:

```html
<meta property="og:url"     content="https://han-minjok.org/">
<meta property="og:image"   content="https://han-minjok.org/og.png">
```

Social scrapers fetch `og:image` from that host, not from the deployment
they were given. Until `han-minjok.org` is attached to the project in
Vercel (Project → Settings → Domains) and resolving, link previews on the
`*.vercel.app` URL will render without an image. Either attach the domain,
or point both tags at the deployment host while testing.
