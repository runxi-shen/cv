# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Single-page static CV website for Runxi Shen. Two files only: `index.html` and `style.css` in the repo root. No frameworks, no build tools, no dependencies. Deploys to GitHub Pages from `main` branch root.

## Development

No build step. To preview locally:
```
open index.html
# or
python3 -m http.server 8000
```

No tests. Verify manually: dark/light mode toggle, print preview (Cmd+P), mobile viewport.

## Architecture

- `index.html` — all CV content in semantic HTML, Inter font via Google Fonts, inline `<script>` at bottom for dark/light theme toggle only
- `style.css` — all styling via CSS custom properties on `[data-theme]` selectors, responsive at 640px breakpoint, print media query

## Constraints

- No JavaScript except the theme toggle (~10 lines)
- No external CSS frameworks
- No images (unless the CV has a headshot URL)
- Total site size under 20KB excluding font loading
- Files in repo root only — no `docs/` or `dist/` subfolder

## Design System — Cool Blue Futuristic

### Colors

| Token | Light Mode | Dark Mode (default) |
|-------|-----------|-----------|
| Background | `#F6F8FA` | `#0D1117` |
| Surface | `#EAEEF2` | `#161B22` |
| Text primary | `#1F2937` | `#E6EDF3` |
| Text secondary | `#656D76` | `#8B949E` |
| Accent | `#2D5F8F` | `#5E89B4` |
| Accent hover | `#1D4B75` | `#7299C1` |
| Border | `#D0D7DE` | `#30363D` |

### Typography

- **Font**: `Inter` via Google Fonts. Fallback: `-apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif`
- **Name/h1**: 2.25rem, weight 600, letter-spacing -0.02em
- **Section headers/h2**: 1.05rem, weight 600, uppercase, letter-spacing 0.12em, text-primary color, 1px solid border-bottom
- **Body**: 1rem, weight 400, line-height 1.7, text-align justify
- **Dates/metadata**: 0.9rem, weight 400, text-secondary

### Layout

- Max-width: 800px, centered
- Vertical padding: 4rem top, 3rem bottom
- Section spacing: 2.5rem between sections
- Entry spacing: 1.5rem between items within a section

### Visual Rules

- **No borders on cards.** Whitespace and typography hierarchy only.
- **Accent line**: 2px solid accent color under the name, width ~40px
- **Links**: accent color, no underline, underline on hover
- **Bullet points**: no native bullets; custom accent-colored dashes via `li::before` pseudo-element (7px wide, 2px tall). Always use whole pixel values for height to avoid subpixel rendering inconsistency.
- **Tags/badges**: pill-shaped — `border: 1px solid` border-color, `border-radius: 9999px`, `padding: 0.15rem 0.6rem`, `font-size: 0.8rem`, text-secondary color
- **Section dividers**: 1px solid border-bottom under each h2
- **Dark mode by default.** Small sun/moon toggle in top-right corner (16px, text-secondary). CSS custom properties with `data-theme` attribute on `<html>`. Respect `prefers-color-scheme` as default. Persist choice in `localStorage`.

### Print

`@media print`: force light mode colors, hide toggle, A4/Letter margins, `page-break-inside: avoid` on entries.

### Responsive

Below 640px: reduce horizontal padding to 1.5rem, stack entry headers vertically.

## HTML Structure

```
<html data-theme="dark">
  <head> ... Inter font, style.css, meta viewport, title, og tags </head>
  <body>
    <header>
      name, tagline, contact row (email · LinkedIn, separated by ·)
    </header>
    <main>
      <section> About </section>
      <section> Experience — each entry: title, org, dates (right-aligned), sub-entries with project names, bullet descriptions </section>
      <section> Education </section>
      <section> Publications — three subsections (peer-reviewed, under review, in preparation), unordered lists with accent-dash bullets, journal names linked to papers </section>
      <section> Invited Talks </section>
      <section> Poster Presentations </section>
      <section> Skills — inline pills grouped by category </section>
      <section> Journal Reviewer </section>
    </main>
    <script> dark/light toggle logic </script>
  </body>
</html>
```

### Formatting Rules

- Experience entries: flex layout — org on left, dates on right
- CV owner's name must be `<strong>` in all publication author lists
- Italicize journal names and organism names (e.g., *Drosophila*, *Wolbachia*)
- Journal names in publications should link to the paper URL where available
- Mark co-first authorship with accent-colored italic annotation
- Awards (e.g., Best Talk Award, Golden Buffalo) styled in accent-colored italic
- Talks with links (e.g., YouTube) should be clickable
