---
name: css-nesting
description: "CSS nesting conventions for this project. Use when writing, editing, or refactoring CSS — including style blocks inside HTML files."
user-invocable: true
---

# CSS Nesting Conventions

## When to Use
- Writing new CSS rules
- Editing or refactoring existing styles
- Working inside `<style>` blocks in HTML files

## Rules

### 1. Use Native CSS Nesting
No preprocessors (Sass, Less). Use the built-in CSS nesting spec, supported baseline from 2024.

### 2. `&` Only for Same-Element Selectors
Use `&` only when targeting the element itself — pseudo-classes, pseudo-elements, and pseudo-selectors:

```css
/* Required */
a {
    &:hover { }
    &:first-child { }
    &:last-child:hover { }
}
```

For descendant selectors (type, class, combinator), omit `&`:

```css
/* Correct */
.nav-list {
    li + li { }
    a { }
}

/* Unnecessary */
.nav-list {
    & li + li { }
    & a { }
}
```

### 3. Global Resets Belong in `:root`
Base element resets (`h1–h4`, `p`, `button`) and shared utility classes (e.g. `.label`) are not structural. Nest them inside `:root`, not inside layout blocks:

```css
:root {
    h1, h2, h3, h4, p { margin-top: 0; }
    button { font: inherit; }
    .label { text-transform: uppercase; }
}
```

### 4. `body` Is the Root of the Structural Hierarchy
`body` should be the outermost nesting block that mirrors the HTML document. All layout-level selectors nest inside it:

```css
body {
    .page-shell {
        .sidebar { }
        .main-content { }
    }
}
```

### 5. Nesting Depth Mirrors the HTML Structure
The nesting hierarchy in CSS should reflect the DOM tree as closely as possible. If an element is a child of another in HTML, its styles should be nested inside the parent's block in CSS.

### 6. Media Queries Follow the Same Hierarchy
Inside `@media` blocks, maintain the same structural nesting — do not flatten selectors:

```css
@media (max-width: 900px) {
    body {
        .page-shell {
            grid-template-columns: 1fr;

            .sidebar { }
            .main-content { }
        }
    }
}
```
