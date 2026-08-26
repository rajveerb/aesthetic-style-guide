# Aesthetic Style Guide

A portable spec for a warm, editorial, paper-like site aesthetic — cream and rust tones that evoke old book pages rather than a tech-blue SaaS template. Use it to reproduce the same look across new projects without copying a whole stylesheet.

The full spec lives in [`style-guide.md`](style-guide.md).

## What's covered

- **Fonts** — `National Park` for body/UI, `Fragment Mono` for code, `Playfair Display` reserved as a serif accent, all loaded via Google Fonts.
- **Color tokens** — CSS custom properties for light mode with a `[data-theme="dark"]` override block. Code blocks stay dark (Monokai-style) in both themes.
- **Layout** — centered `65ch` content column, sticky footer via flex, `clamp()`-based heading sizes, `text-wrap: balance`/`pretty`.
- **Components** — links, pill buttons, category tags, framed cards, blockquotes, inline code and code blocks, tables, floating round buttons.
- **Theming** — `data-theme` attribute on `<html>`, persisted to `localStorage`, with a no-FOUC inline script.
- **Cheat sheet** — a one-table summary of every token in light and dark.

## Quick start

1. Add the Google Fonts `<link>` from the [Fonts](style-guide.md#fonts) section to your `<head>`.
2. Paste the `:root` and `:root[data-theme="dark"]` blocks from [Color tokens](style-guide.md#color-tokens) into your stylesheet.
3. Drop in the no-FOUC script from [Theming mechanism](style-guide.md#theming-mechanism) and wire a toggle button to flip `data-theme`.
4. Build components against the tokens (`--cream`, `--rust`, `--dark-brown`, `--radius`, …) using the snippets in [Components](style-guide.md#components).

## Palette at a glance

| Token | Light | Dark |
|---|---|---|
| Background | `#f1e9df` | `#1a1714` |
| Heading / primary text | `#483a31` | `#e8ddd0` |
| Accent (links, brand) | `#89553e` | `#c4836a` |
| Body copy | `#7b6f65` | `#a89e94` |
| Card / surface | `#f4efe9` | `#242019` |

## License

[MIT](LICENSE)
