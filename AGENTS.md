# Project Guidelines

## Overview

Static HTML/CSS dashboard demo used as webinar material for **Controlled AI-Assisted Coding**.  
No build system, no JavaScript framework, no package manager — just `public/index.html` and `public/styles.css`.

## Architecture

```
public/
  index.html   ← page structure and content
  styles.css   ← all styles (tokens + components)
```

## CSS Conventions

**Design tokens** — all values live in `:root` inside `styles.css`. Always use tokens, never hard-code colors, radii, or shadows.

| Group | Prefix | Example |
|-------|--------|---------|
| Neutral palette | `--neutral-*` | `--neutral-800` |
| Primary (indigo) | `--primary-*` | `--primary-600` |
| Accent (teal) | `--accent-*` | `--accent-400` |
| Semantic aliases | `--color-*` | `--color-bg`, `--color-text-muted` |

Prefer semantic aliases (`--color-text`) over raw palette tokens (`--neutral-800`) in component rules.

**Modern CSS nesting** — component styles are nested inside `.page-shell { … }`. No preprocessor. Follow the existing nesting depth; avoid flattening into top-level rules.

**Class naming** — descriptive kebab-case. Key classes: `.page-shell`, `.sidebar`, `.sidebar-block`, `.nav-list`, `.main-content`, `.page-header`, `.header-actions`, `.content-section`, `.section-heading`, `.card-grid`, `.card`, `.two-column-layout`, `.panel`, `.table-panel`, `.activity-list`.

**Buttons** — styled by position, not by class. First `<button>` in `.header-actions` = primary; last (non-first) = secondary. Do not add extra classes.

## HTML Conventions

- Use semantic landmarks: `<aside>`, `<main>`, `<header>`, `<section>`, `<nav>`, `<article>`.
- Always pair a `<p class="label">` with the heading that follows it inside `.section-heading`, `.sidebar-block`, and `.page-header > div`.
- Tables must be wrapped in `<article class="panel table-panel">` — this removes padding and clips overflow correctly.
- Add `aria-label` to every `<nav>`.

## No-Build Workflow

- Open `public/index.html` directly in a browser (or use a simple static server).
- No compile step, no `npm install`, no type-checking — changes are immediately visible on reload.
