---
name: deadhop-brand
description: Apply the deadhop brand to a UI, a docs page, a README, slides, a social card or any image. Covers the two faces (quiet checkout vs stamped developer), the colour tokens in tokens.css and the role of each, the three self-hosted typefaces (Schibsted Grotesk, Geist, Martian Mono), the design language rules (dashes mean in transit, boxes mean held, yellow is only a label, one teal accent), which logo file to use and what never to do with the mark, and where copies of the brand files live in other deadhop repositories. Use it whenever someone asks to make something look like deadhop, on-brand, in the deadhop style, to pick deadhop colours or fonts, to place the deadhop logo or favicon, or to change or regenerate a logo, icon, font or token in deadhop/brand.
license: MIT
metadata:
  repository: https://github.com/deadhop/brand
---

# The deadhop brand

deadhop is "dead drop" plus "hop": payments are left in a drop, every message travels relay to
relay, and the service has no listening socket. The identity draws both ideas: the **hop** (a
dashed arc) and the **drop** (a box with a slot). The originals of every brand file live in
[deadhop/brand](https://github.com/deadhop/brand). Its `README.md` is the full reference; read
it when a detail here is not enough.

## 1. Pick the face first

Every deliverable uses one of two faces. Decide which before choosing anything else, and ask the
user if it is not obvious.

| Face | For | Feel |
| --- | --- | --- |
| **Checkout** | pay buttons, invoices, anything a customer sees on a merchant's site | Quiet. Geist sentences, one teal accent, plain words |
| **Developer** | CLI, README, docs, dashboard, npm, slides for operators | Stamped. Martian Mono labels, drop IDs, yellow tags, dark ground |

The wording follows the face. A customer reads "Payment seen. Waiting for 1 confirmation."; an
operator reads `DROP 7F3A · SEEN · 0/1`.

Checkout components are guests on someone else's site: they inherit the merchant's font and use a
graphite button, so they never compete with the shop's own accent. Do not force deadhop fonts or
teal onto a merchant's checkout.

## 2. Colours

Use the custom properties in `tokens.css` rather than retyping hex values. The file defines light
and dark values as **separate variables**; it has no `prefers-color-scheme` switch. If a page
needs a dark mode, map the roles yourself, for example:

```css
:root { --ground: var(--dh-concrete); --ink: var(--dh-graphite); --accent: var(--dh-route); }
@media (prefers-color-scheme: dark) {
  :root { --ground: var(--dh-night); --ink: var(--dh-fog); --accent: var(--dh-route-dark); }
}
```

| Role | Light | Dark | Used for |
| --- | --- | --- | --- |
| ground | `--dh-concrete` `#e9e8e3` | `--dh-night` `#0e1214` | page background |
| surface | `--dh-paper` `#f5f4f0` | `--dh-depot` `#161b1d` | cards, invoices |
| ink | `--dh-graphite` `#16191b` | `--dh-fog` `#e6ebe9` | text, mark, primary buttons |
| secondary | `--dh-steel` `#5b636b` | `--dh-haze` `#8e979b` | secondary text |
| line | `--dh-rule` `#cfcdc6` | `--dh-rule-dark` `#2a3236` | rules and outlines |
| accent | `--dh-route` `#0b6e69` | `--dh-route-dark` `#5fc6b9` | the one accent: mark, links, settled progress |
| tag | `--dh-tag` `#e8c547` | `--dh-tag` `#e8c547` | small status labels only |

`--dh-ok`, `--dh-bad` and their `-dark` variants are semantic colours for success and error
states, not brand colours. Keep them to status messages.

Every listed pair passes WCAG AA for body text (the README lists the ratios). If you combine
colours in a way the README does not list, check the contrast before shipping.

## 3. Type

| Role | Face | Token | Where |
| --- | --- | --- | --- |
| Display | Schibsted Grotesk 700–800 | `--dh-font-display` | wordmark, headings, amounts |
| Body | Geist 400–600 | `--dh-font-body` | sentences |
| Mono | Martian Mono 400–500 | `--dh-font-mono` | addresses, keys, IDs, code, stamped labels |

The fonts are self-hosted from `fonts/` (variable WOFF2, subset to Latin, SIL Open Font
License). `tokens.css` declares the `@font-face` rules with paths relative to itself. Never load
them from a font CDN: self-hosting lets a strict Content-Security-Policy stay at
`font-src 'self'`. Ship the `OFL-*.txt` licence files with any copy of the fonts.

## 4. Design language

- **Dashes mean "in transit".** Pending progress, loading and the relay arc are dashed
  (`--dh-dash: 6px 10px`). Settled things are solid.
- **Boxes mean "held".** Containers get 4 px corners (`--dh-radius`) and 1.5 px outlines
  (`--dh-outline`), and no drop shadows.
- **Yellow is a label, never a fill.** Tag yellow appears only on small stamped labels such as
  `SEEN` or `ports open: 0`. Never a button, a background or a large area.
- **One accent.** Route teal. Everything else is graphite, steel and concrete (or their dark
  counterparts).
- **Mono for what you copy.** Addresses, IDs, keys and CLI output use Martian Mono.

When reviewing a design, check each of these rules in turn and name the one a change breaks.

## 5. Logo

The mark is a dot (you), a dashed hop and a slotted drop box, drawn on a 32-unit grid in one
colour. It holds at 16 px.

| File | Use |
| --- | --- |
| `logo/lockup.svg` | default: route-teal mark, graphite wordmark, on light grounds |
| `logo/lockup-dark.svg` | on dark grounds |
| `logo/lockup-mono.svg`, `logo/lockup-mono-fog.svg` | one colour (print, embossing) |
| `logo/mark.svg`, `logo/mark-fog.svg`, `logo/mark-route.svg` | the mark alone |
| `logo/mark-currentcolor.svg` | the mark, inheriting the surrounding text colour |
| `logo/wordmark.svg`, `logo/wordmark-fog.svg` | the wordmark alone |
| `icons/icon.svg`, `icons/icon-192.png`, `icons/icon-512.png`, `icons/apple-touch-icon.png` | app, avatar and touch icons (mark on a night tile) |
| `icons/favicon.svg`, `icons/favicon.ico` | favicons; the SVG switches to fog in dark browser UI |
| `og.png` | 1200 × 630 social card |

For a README or a page that has both themes, use a `<picture>` with `lockup-dark.svg` (or
`mark-fog.svg`) as the `prefers-color-scheme: dark` source.

Rules:
- Clear space of at least the height of the mark's box on every side. Minimum size: mark 16 px,
  lockup 18 px tall.
- The wordmark is Schibsted Grotesk 800, tracked −3.5%, always lowercase, and already outlined in
  the SVGs. Do not retype it in a live font; use the file.

Never:
- recolour the mark outside the palette, or use bitcoin orange;
- make the dashes solid (they mean "in transit");
- put the lockup on busy imagery, add effects, or set the wordmark in another font;
- add a ₿ glyph, an onion, an ostrich or a padlock to the mark.

## 6. Changing a brand file

The files in this repository are the originals. Other deadhop repositories hold **copies**, not a
submodule, so each builds from its own clone:

- [deadhop/relay](https://github.com/deadhop/relay) and [deadhop/lnaddr](https://github.com/deadhop/lnaddr):
  `assets/brand/` (fonts, `icons/favicon.svg`, `logo/mark-currentcolor.svg`), compiled into their
  status and admin pages.
- [deadhop/js](https://github.com/deadhop/js): `brand/` (fonts, `icons/favicon.svg`) for the
  dashboard and console.
- [deadhop/site](https://github.com/deadhop/site): `brand/` (fonts, icons, mark, social card),
  plus the served copies in `public/` and `docs/public/`. The site's check fails if a served copy
  differs from its own `brand/`.
- The mark is also inlined as SVG in deadhop/js `dashboard/src/components/App.svelte` and
  deadhop/site `src/pages/index.astro`. Nothing checks those; update them by hand.

So a change goes in this order: change the original here and open a pull request; once it is
merged, copy the changed files into each repository above, one pull request per repository.
Never edit only a copy, because the next sync from this repository would undo it.

## 7. Regenerating

Run these from the repository root. The README says all three reproduce the committed files byte
for byte, so after a run `git status` should show only the files you meant to change.

1. `tools/make_fonts.sh` rebuilds `fonts/` from google/fonts (needs `fonttools`, `brotli`).
2. `python3 tools/make_logos.py <SchibstedGrotesk[wght].ttf> .` rebuilds `logo/` and the SVG icons
   (needs `fonttools`, `uharfbuzz`). The mark's geometry and the palette are defined once, at the
   top of this script; change the mark there, never by hand-editing an SVG.
3. `tools/render_rasters.sh` renders the PNG icons, `favicon.ico` and `og.png` (from
   `tools/og.html`) with headless Chrome (needs Pillow; set `CHROME` to point at another Chrome
   binary).

Run step 3 after step 2 whenever the mark changes, since the rasters come from the SVGs.

## Verify

- Every colour in the result is a token from `tokens.css` (or a role mapped onto one).
- Teal is the only accent; yellow appears only on small labels.
- Copyable strings are in Martian Mono; no font loads from a CDN.
- The logo file matches the ground (light or dark), with clear space and above minimum size.
- For a change to this repository: the regeneration tools leave no unexpected diff, and the
  copies in the repositories listed above are updated.
