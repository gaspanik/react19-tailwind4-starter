---
paths:
  - "src/**/*.{tsx,css}"
---

# Tailwind CSS v4 Rules

Tailwind v4 runs via the `@tailwindcss/vite` plugin. Config is CSS-based — never create `tailwind.config.js`. Global styles live in `src/index.css` (`@import "tailwindcss"` plus `@theme` tokens).

## v4 syntax only

Never generate v3-specific utilities; convert them when refactoring:

- ❌ `space-x-*` / `space-y-*` → ✅ `gap-*` with flex/grid
- ❌ `divide-*` → ✅ borders on individual children

## Values and tokens

- Use the standard spacing scale first (`gap-2`, `p-4`, `w-80`). Arbitrary values (`w-[42px]`) are a last resort for design-mandated exact values — and add a `@theme` token if used more than once
- Never use Tailwind scale colors (`gray-*`, `neutral-*`, …) for project-specific colors. Define every color as a `@theme` token, including dark-background and hover variants:

```css
/* src/index.css */
@theme {
  --color-dark: #111111;
  --color-dark-hover: #262626;
  --color-muted: #666666;
  --color-muted-dark: #a3a3a3;   /* subdued text on dark backgrounds */
  --color-border: #e5e5e5;
  --color-border-dark: #404040;  /* borders on dark backgrounds */
}
```

```tsx
// ❌ <p className="text-neutral-400">   →   ✅ <p className="text-muted-dark">
```

## Styling approach

- Tailwind-first: no CSS modules or styled-components; keep styles in `className` props
- Compose conditional classes with `cn()` from `@/lib/utils`; apply an external `className` last so it can override
- Icons: `lucide-react` with consistent sizing — `w-4 h-4` inline, `w-6 h-6` for headings

## Font family

`--default-font-family` is the body default (set in `@theme`) and `--heading-font-family` is already applied to h1–h6 via `@layer base` — both in `src/index.css`. Never write `font-[var(--heading-font-family)]`, `font-(--heading-font-family)`, or their `--default-font-family` equivalents; they are redundant. To use the heading font on a non-heading element (e.g. a logo `<a>`), extend `@layer base` or define a dedicated utility in `@theme` instead.

## Shared class consolidation (`*:`)

When 3+ sibling elements share 2+ identical classes, move them to the parent with the `*:` variant (applies to direct children only):

```tsx
// ❌ Avoid
<ul>
  <li><a className="hover:text-white">About</a></li>
  <li><a className="hover:text-white">Works</a></li>
  <li><a className="hover:text-white">Contact</a></li>
</ul>

// ✅ Correct
<ul className="*:hover:text-white">
  ...
</ul>
```

## Accessibility

- Prefer semantic HTML over generic `div`/`span` where an element's role matches: `header`, `nav`, `main`, `section`, `article`, `footer`
- Every `<img>` needs `alt` — descriptive text for meaningful images, `alt=""` for purely decorative ones
- Every `<button>` needs an explicit `type="button"` (or `"submit"` when it should submit a form) — an omitted type defaults to `"submit"` inside `<form>`, a common source of bugs
- Every form input has an associated `<label htmlFor>` (or `aria-label` if visually hidden)
- Navigation: use TanStack Router's `Link` inside `<nav aria-label="Main navigation">` with `<ul>/<li>` structure. `Link` auto-applies the `.active` class — style it with `className="[&.active]:font-bold [&.active]:text-blue-600"`
- Collapsible/menu buttons: set `aria-expanded={isOpen}`, `aria-controls="<target-id>"`, and a descriptive `aria-label`
- Never write text in ALL CAPS directly in JSX — write proper case and apply the `uppercase` class. Screen readers may spell out capitalized text letter by letter; brand names and proper nouns are exempt

```tsx
// ❌ <a href="#about">ABOUT</a>
// ✅ <a href="#about" className="uppercase">About</a>
```
