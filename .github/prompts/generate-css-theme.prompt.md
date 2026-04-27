---
description: "Generate a CSS :root theme with foreground/background, neutral palette, and named color palettes using light-dark() and color-mix()"
name: "Generate CSS Theme"
argument-hint: "Optional: target file to insert the theme into"
agent: "agent"
---

Generate a CSS theme inside the `:root` selector by following the steps below exactly.

## Step 1 — Gather inputs

Use the `vscode_askQuestions` tool to ask all of the following in a single call:

1. **foreground_light** — What is the foreground (text) color for **light mode**? (e.g. `#1e293b`)
2. **foreground_dark** — What is the foreground (text) color for **dark mode**? (e.g. `#f1f5f9`)
3. **background_light** — What is the background color for **light mode**? (e.g. `#ffffff`)
4. **background_dark** — What is the background color for **dark mode**? (e.g. `#0f172a`)
5. **neutral_shades** — How many intermediate shades should the neutral palette have? Enter a number between 1 and 9 (e.g. `9` gives entries at 0, 10, 20 … 90, 100).
6. **primary_color** — What is the primary palette base color? (e.g. `#2563eb`)
7. **primary_shades** — How many steps in the primary palette? Enter a number between 1 and 9 (same meaning as above).
8. **accent_color** — What is the accent palette base color? (e.g. `#fbbf24`)
9. **accent_shades** — How many steps in the accent palette?
10. **extra_palettes** — Do you want any additional palettes? If yes, list each one as `name:#hexcolor:shades` separated by commas (e.g. `success:#16a34a:4,danger:#dc2626:4`). Leave blank for none.

## Step 2 — Build the CSS

Using the answers, generate a `:root` block with the following structure:

### color-scheme declaration
```css
color-scheme: light dark;
```

### --foreground and --background
Use the CSS `light-dark()` function:
```css
--foreground: light-dark(<foreground_light>, <foreground_dark>);
--background: light-dark(<background_light>, <background_dark>);
```

### Neutral palette
- Entries are named `--neutral-0` through `--neutral-100`, stepping by `floor(100 / (shades + 1))` rounded to the nearest 10. Always include `--neutral-0` and `--neutral-100`.
- `--neutral-0` equals `var(--background)`
- `--neutral-100` equals `var(--foreground)`
- Every intermediate entry `--neutral-N` uses:
  ```css
  --neutral-N: color-mix(in srgb, var(--foreground) N%, var(--background));
  ```

### Named color palettes (primary, accent, and any extras)
For each palette named `<palette>` with base color `<base>` and `<steps>` shades:

- **Always declare a `--<palette>-base` token first** and use `var(--<palette>-base)` everywhere in that palette's entries. Never repeat the raw hex value.
  ```css
  --<palette>-base: <base>;
  ```
- Entries are named `--<palette>-0` through `--<palette>-100`, stepping by `floor(100 / (steps + 1))` rounded to the nearest 10. Always include the extremes and `--<palette>-50`.
- **`--<palette>-50`** equals `var(--<palette>-base)`.
- **Below 50** — mix the base token with the background; percentage of base color = `N * 2`:
  ```css
  --<palette>-N: color-mix(in srgb, var(--<palette>-base) calc(N * 2%), var(--background));
  ```
- **Above 50** — mix the foreground into the base token; percentage of foreground = `(N - 50) * 2`:
  ```css
  --<palette>-N: color-mix(in srgb, var(--foreground) calc((N - 50) * 2%), var(--<palette>-base));
  ```
- `--<palette>-0` equals `var(--background)` and `--<palette>-100` equals `var(--foreground)`.

Compute all percentages as plain integers (no `calc()` needed for static values).

## Step 3 — Output target

- If an argument was provided (a file path), insert the generated `:root { … }` block into that file using `replace_string_in_file` or `create_file` as appropriate.
- Otherwise, output the complete CSS as a code block in chat so the user can copy it.

## Step 4 — Confirm and offer next steps

After outputting the theme, briefly confirm what was generated (palette names, shade counts) and ask the user if they want to:
- Adjust any palette
- Add more palettes
- Generate a companion dark-mode override block
