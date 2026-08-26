# Site Aesthetic Style Guide

A portable spec for reproducing this warm, editorial aesthetic in other projects.

## Vibe

Warm, editorial, paper-like. Cream/rust palette evoking old book pages, not a typical tech-blue SaaS site. Generous whitespace, centered narrow content column, soft rounded corners, no shadows except a subtle one on floating scroll buttons.

## Fonts

Loaded via Google Fonts:

```html
<link rel="preconnect" href="https://fonts.googleapis.com" />
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
<link
  href="https://fonts.googleapis.com/css2?family=Fragment+Mono&family=National+Park:wght@400;500;700&family=Playfair+Display:wght@400&display=swap"
  rel="stylesheet"
/>
```

| Role | Font | Usage |
|---|---|---|
| Body/UI | `National Park` (400/500/700), fallback `sans-serif` | All body text, nav, headings |
| Code/mono | `Fragment Mono`, fallback `monospace` | Inline `code`, code blocks, venue tags, copy button |
| Reserved | `Playfair Display` (400) | Loaded but currently unused — available for a serif accent (e.g. a pull-quote or hero word) |

Base body text: `font-size: 1.125rem; line-height: 1.5;` with `-webkit-font-smoothing: antialiased;`.

## Color tokens

Defined as CSS custom properties, with a `[data-theme="dark"]` override block for dark mode (toggled via a `<html data-theme="dark">` attribute, not `prefers-color-scheme` alone — see Theming below).

```css
:root {
  --cream: #f1e9df;           /* page background */
  --cream-75: rgba(241, 233, 223, 0.75);
  --dark-brown: #483a31;      /* headings, strong emphasis, primary text */
  --rust: #89553e;            /* links, accent, brand color */
  --body-text: #7b6f65;       /* paragraph/body copy */
  --card-bg: #f4efe9;         /* inline code bg, card surfaces */
  --code-text: #fcfcfa;       /* code block text */
  --code-bg: #2d2a2e;         /* code block background (dark, both themes) */
  --code-tab: #444344;        /* copy-button background on code blocks */
  --radius: 6px;
  --radius-sm: 3px;
  --content-width: 65ch;
  --border-light: rgba(137, 85, 62, 0.1);
  --border-medium: rgba(137, 85, 62, 0.15);
  --border-strong: rgba(137, 85, 62, 0.2);
}

:root[data-theme="dark"] {
  --cream: #1a1714;
  --cream-75: rgba(26, 23, 20, 0.75);
  --dark-brown: #e8ddd0;
  --rust: #c4836a;
  --body-text: #a89e94;
  --card-bg: #242019;
  --code-text: #fcfcfa;
  --code-bg: #2a2628;
  --code-tab: #3a3839;
  --border-light: rgba(200, 180, 160, 0.1);
  --border-medium: rgba(200, 180, 160, 0.15);
  --border-strong: rgba(200, 180, 160, 0.2);
}
```

`--code-bg`/`--code-text` stay dark in both themes — code blocks are always "dark mode" (Monokai-style), even on the light page.

Images inside prose auto-invert in dark mode to stay readable against the dark background:

```css
:root[data-theme="dark"] .article-body img,
:root[data-theme="dark"] .page-content img {
  filter: invert(1) hue-rotate(180deg);
}
```
Opt out per-image with a `.no-invert` wrapper class (used for emoji/logos that shouldn't invert).

## Layout

- Reset: `* { margin:0; padding:0; box-sizing:border-box; }`
- Body is a flex column (`min-height:100vh; display:flex; flex-direction:column;`) so the footer sticks to the bottom (`main { flex: 1 0 auto; }`, footer `margin: auto auto 0`).
- Content is centered and width-capped: `width: min(var(--content-width), 100% - 2rem); margin: 0 auto;` — this single pattern is reused for nav, page content, article body, footer, publication list.
- `--content-width: 65ch` keeps line length readable.
- Headings (`h1–h4`) use `text-wrap: balance`; paragraphs/`li` use `text-wrap: pretty`.
- Responsive heading sizes via `clamp()`, e.g. `font-size: clamp(1.75rem, 1.5rem + 1.25vw, 2.25rem);` for h1.

## Components

**Links** — `color: var(--rust)`, hover → `var(--dark-brown)`, `transition: color 0.2s`. In prose, links are underlined with `text-underline-offset: 2px`; nav/pub links are bold, no underline.

**Buttons/pill links** (e.g. article action links) — bordered, uppercase, letter-spaced tag style:
```css
font-size: 0.8125rem;
font-weight: 700;
letter-spacing: 0.56px;
text-transform: uppercase;
color: var(--rust);
border: 1px solid var(--rust);
border-radius: var(--radius);
padding: 0.4rem 0.9rem;
transition: background-color 0.15s ease, color 0.15s ease;
/* hover: background-color: var(--rust); color: var(--cream); */
```

**Venue/category tag** — small uppercase pill, solid rust background, cream text:
```css
font-size: 0.75rem;
font-weight: 700;
letter-spacing: 0.06em;
text-transform: uppercase;
color: var(--cream);
background-color: var(--rust);
padding: 0.25rem 0.75rem;
border-radius: var(--radius-sm);
```

**Cards/framed header** — a bordered box using a `::after` pseudo-element for the border (allows border + `overflow: clip` together):
```css
.article-header {
  border-radius: var(--radius);
  overflow: clip;
  position: relative;
}
.article-header::after {
  content: "";
  position: absolute;
  inset: 0;
  border: 2px solid var(--dark-brown);
  border-radius: var(--radius);
  pointer-events: none;
}
```

**Blockquote** — left rust accent bar via `::before`, larger italic-free but colored rust text:
```css
blockquote { padding: 0 0 0 1.75rem; position: relative; }
blockquote::before {
  content: ""; position: absolute; top:0; left:0;
  width: 0.5rem; height: 100%;
  background-color: var(--rust); border-radius: 4px;
}
blockquote p { font-size: 1.25rem; color: var(--rust); font-weight: 500; }
```

**Inline code** — `font-family: "Fragment Mono"`, `background: var(--card-bg)`, `padding: 0.125em 0.375em`, `border-radius: var(--radius-sm)`.

**Code blocks** — `background: var(--code-bg)`, `padding: 1.5rem`, `border-radius: var(--radius)`; syntax highlighting uses a Monokai-style Rouge palette (key colors: keywords `#66d9ef`, strings `#e6db74`, numbers/literals `#ae81ff`, names/functions `#a6e22e`, operators/errors `#f92672`, comments `#75715e`, default text `#f8f8f2`).

**Tables** — bordered with rounded corners, dark header row (`background: var(--dark-brown); color: var(--cream)`), centered cell text, `border: 2px solid var(--border-strong)` outer / `var(--border-medium)` inner.

**Floating round buttons** (e.g. scroll-to-top) — `border-radius: 50%`, `background: var(--dark-brown)`, `color: var(--cream)`, subtle shadow: `box-shadow: 0 2px 8px rgba(0,0,0,0.15)`, `opacity` fade on hover.

## Theming mechanism

Dark mode toggles a `data-theme="dark"` attribute on `<html>` (not just `prefers-color-scheme`), persisted to `localStorage.theme`. A no-FOUC inline script in `<head>` applies it before first paint:

```html
<script>
  (function () {
    var saved = localStorage.getItem("theme");
    if (saved === "dark" || (!saved && window.matchMedia("(prefers-color-scheme: dark)").matches)) {
      document.documentElement.setAttribute("data-theme", "dark");
    }
  })();
</script>
```

A `.theme-toggle` button (SVG icon, `stroke: currentColor`) flips the attribute and writes to `localStorage` on click.

## Quick-reference cheat sheet

| Token | Light | Dark |
|---|---|---|
| Background | `#f1e9df` | `#1a1714` |
| Heading/primary text | `#483a31` | `#e8ddd0` |
| Accent (links, brand) | `#89553e` | `#c4836a` |
| Body copy | `#7b6f65` | `#a89e94` |
| Card/surface | `#f4efe9` | `#242019` |
| Code background | `#2d2a2e` | `#2a2628` (always dark) |
| Radius | `6px` (cards), `3px` (small) | same |
| Body font | `National Park` | same |
| Mono font | `Fragment Mono` | same |
