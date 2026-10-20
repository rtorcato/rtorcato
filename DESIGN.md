# DESIGN.md: profile README banner

Spec for the header banner on the GitHub profile README (issue #7). rtorcato picked direction A, a static hero. The banner replaces the plain `# Richard Torcato` h1.

## Goal

A quiet, static banner in the torcato.dev visual language that reads well in GitHub light and dark, on desktop and phone. Nothing on it moves, and it never needs updating.

## Files

Commit four hand-written SVGs under `banner/`:

| File | viewBox | Used when |
|---|---|---|
| `banner/wide-dark.svg` | `0 0 1200 360` | dark theme, viewport wider than 600px |
| `banner/wide-light.svg` | `0 0 1200 360` | light theme, viewport wider than 600px |
| `banner/narrow-dark.svg` | `0 0 600 300` | dark theme, viewport 600px or narrower |
| `banner/narrow-light.svg` | `0 0 600 300` | light theme, viewport 600px or narrower |

Don't add a build step, a third-party service (readme-typing-svg, capsule-render) or a PNG export. The README links the files from the default branch, the same way the snake links its SVGs.

## Markup

Replace the h1 with this, keeping the bold role line under it:

```html
<picture>
  <source media="(max-width: 600px) and (prefers-color-scheme: dark)" srcset="banner/narrow-dark.svg" />
  <source media="(max-width: 600px)" srcset="banner/narrow-light.svg" />
  <source media="(prefers-color-scheme: dark)" srcset="banner/wide-dark.svg" />
  <img alt="Richard Torcato, senior full-stack developer" src="banner/wide-light.svg" width="100%" />
</picture>
```

- Source order matters: the browser takes the first match, so the narrow sources come first.
- A client that ignores the width query (the GitHub mobile app may) falls back to the wide banner. That's acceptable.
- Each SVG also carries `role="img"` and a `<title>` that matches the alt text.

## Copy

- Name: `Richard Torcato`
- Role: `Senior full-stack developer · since 1997`. On the narrow banner the role and `since 1997` sit on separate lines.
- Prompt: `~/rtorcato`
- Chip (wide only): `$ npx @rtorcato/repo-tooling`
- The banner keeps "since 1997" because it never goes out of date. The README bio line drops its "30+ years" count in the same PR, so the two can't disagree. It becomes **Senior Full Stack Application Developer · Shipping web and interactive applications**.

## Layout

All values are in SVG user units.

**Wide (1200 × 360)**

| Element | x | Baseline y | Font | Size | Colour |
|---|---|---|---|---|---|
| `~/rtorcato` | 72 | 92 | mono | 26 | accent |
| Name | 68 | 184 | sans 600, letter-spacing −3 | 96 | heading |
| Role + `since 1997` | 72 | 240 | sans 400 | 36 | muted, with `since 1997` in accent |
| Chip box | 72, y 270 | 474 × 56, rx 12 | | | chip fill + chip border |
| Chip text | 96 | 307 | mono | 24 | text, with `$` in accent |
| 3×3 mark | top-left dot centre 1010, 120 | gap 34, r 9 | | | the nine repo accents |

**Narrow (600 × 300)**

| Element | x | Baseline y | Font | Size | Colour |
|---|---|---|---|---|---|
| `~/rtorcato` | 40 | 68 | mono | 22 | accent |
| Name | 37 | 150 | sans 600, letter-spacing −2 | 60 | heading |
| Role | 40 | 202 | sans 400 | 30 | muted |
| `since 1997` | 40 | 246 | sans 400 | 30 | accent |
| 3×3 mark | top-left dot centre 504, 44 | gap 22, r 6 | | | the nine repo accents |

The narrow banner has no chip. At a 390px phone width the role renders at about 17px and the name at about 33px.

**Both:** a full-bleed background rect, then a 24px dot grid (r 1.4, grid colour), clipped to an rx 20 rounded rect, with a 1px border stroke inset 0.5.

## Tokens

These are the torcato.dev values.

| Token | Dark | Light |
|---|---|---|
| background | `#0a0e16` | `#ffffff` |
| heading | `#ffffff` | `#0a0e16` |
| text | `#dfe5ee` | `#374253` |
| muted | `#9aa5b5` | `#5b6675` |
| accent | `#25c2a0` | `#2e8555` |
| chip fill | `#111824` | `#f3f6f9` |
| chip / frame border | `rgba(255,255,255,.12)` / `.10` | `rgba(12,18,28,.12)` |
| grid dots | `rgba(255,255,255,.06)` | `rgba(12,18,28,.07)` |

The 3×3 mark uses these colours in reading order: `#58a6ff #25c2a0 #3ecf8e #a3e635 #f2cc60 #a78bfa #e879f9 #22d3ee #818cf8`.

Contrast on the banner background:

- muted is 7.75:1 dark and 5.83:1 light
- accent is 8.56:1 dark and 4.56:1 light

Every text element passes WCAG 2.1 AA at the sizes it renders at.

## Type

- Sans: Inter (400, 600).
- Mono: Geist Mono, the torcato.dev mono.
- **Convert all text to outlines (paths) before committing.** GitHub serves README SVGs through `<img>`, which can't load web fonts, so live text would fall back to whatever system font the viewer has. Keep the `<title>` as real text for screen readers.

## Brand

This is a personal profile banner, not a repo brand asset, so it doesn't come from `@rtorcato/brand-kit`. It reuses that kit's tokens and the 3×3 torcato.dev mark so it stays consistent with the docs sites.

## Rules

- No motion. If the banner ever animates, it needs a new design pass and a `prefers-reduced-motion` fallback.
- Don't use cream or terracotta.
- Keep the wide aspect ratio at 10:3 and the narrow one at 2:1 so the bio line below doesn't jump between themes.

## Mockups

Approved mockups are posted in the Buzz project channel. They aren't stored in this repo.
