# GEMINI.md - Project Context & Instructions

This file serves as the primary context for AI agents and developers working on this project.

## 1. Project Overview

This is a modern Single Page Application (SPA) built with **React 19**, **TypeScript**, and **Vite 8**. It utilizes **TanStack Router** for type-safe, file-based routing and **Tailwind CSS v4** for styling. The project enforces strict code quality using **Biome** and uses **pnpm** (default) for package management; npm/yarn/bun also work.

## 2. Tech Stack

| Technology | Version | Purpose |
| :--- | :--- | :--- |
| **React** | 19.2+ | UI Library (using `react-jsx` transform) |
| **TypeScript** | 6.x | Static Typing |
| **Vite** | 8.x | Build Tool (using `@vitejs/plugin-react`) |
| **TanStack Router** | 1.144+ | Type-safe Routing |
| **Tailwind CSS** | 4.x | Utility-first CSS (via `@tailwindcss/vite`) |
| **Utilities** | - | `cn` (clsx + tailwind-merge), `class-variance-authority`, `tailwind-variants` |
| **Icons** | - | `lucide-react` |
| **Biome** | 2.3+ | Linter & Formatter |
| **pnpm** (default) | 10.x | Package Manager; npm/yarn/bun also work |

## 3. Key Commands

> Replace `<pm>` with your package manager (pnpm / bun / yarn / npm).

**Development**
- `<pm> run dev`: Start the development server (http://localhost:5173).
- `<pm> run preview`: Preview the production build locally.

**Build & Quality**
- `<pm> run build`: Type-check (`tsc`) and build for production.
- `<pm> run check`: Run Biome to lint and format code.
- `<pm> run lint`: Run Biome linter (fix with `--write`).
- `<pm> run format`: Run Biome formatter (fix with `--write`).

**Task Runner (Mise)**
- `mise run vite:dev` → `<pm> run dev`
- `mise run vite:build` → `<pm> run build`
- `mise run vite:preview` → `<pm> run preview`

## 4. Project Structure

```
ts-swc/
├── src/
│   ├── assets/            # Static assets
│   │   └── images/        # Optimized image handling
│   ├── components/        # Reusable UI components
│   │   └── ButtonCn.tsx   # Example component using 'cn' utility
│   ├── lib/               # Utility functions
│   │   ├── image.ts       # Eager image loading
│   │   ├── imageAsync.ts  # Lazy image loading
│   │   └── utils.ts       # Contains 'cn' (clsx + tailwind-merge)
│   ├── routes/            # TanStack Router file-based routes
│   │   ├── __root.tsx     # Root layout
│   │   └── index.tsx      # Homepage
│   ├── index.css          # Global styles & Tailwind import
│   ├── main.tsx           # Entry point
│   └── routeTree.gen.ts   # AUTO-GENERATED - DO NOT EDIT
├── biome.json             # Linter/Formatter config
├── pnpm-lock.yaml         # Lockfile (do not delete)
├── tsconfig.app.json      # App-specific TS config
└── vite.config.ts         # Vite configuration
```

## 5. Coding Conventions & Best Practices

### React
- **Imports**: Do NOT import `React`. Use named imports for hooks (e.g., `import { useState } from 'react'`).
- **Components**: Use function declarations. Explicitly type props with interfaces.
- **Structure**: Use semantic HTML tags (`header`, `main`, `footer`, etc.).
- **Fragments**: Use `<>...</>` syntax.

### TypeScript
- **Path Alias**: Use `@/` to import from `src/`.
- **Strictness**: `noImplicitAny` and `strict` are enabled.
- **Modules**: `moduleResolution: "bundler"`.

### Code Quality (Biome)
- **Formatting**: 2 spaces indentation, double quotes for JSX, no semicolons (unless needed), trailing commas.
- **Workflow**: Run `<pm> run check` before committing.

### Routing (TanStack Router)
- **File-Based**: Routes are defined by files in `src/routes/`.
- **Generation**: `src/routeTree.gen.ts` is auto-generated. **NEVER edit it manually.**
- **Creation**: Use `createFileRoute` for defining route components.
- **Navigation**: Use `Link` component.
  ```tsx
  import { Link } from '@tanstack/react-router'
  <Link to="/about" className="[&.active]:font-bold">About</Link>
  ```

### Tailwind CSS v4
- **Documentation**: Use available **Tailwind CSS MCP** tools (e.g., `search_tailwind_docs`, `get_tailwind_utilities`) to reference the latest v4 documentation and verify utility availability.
- **Configuration**: NO `tailwind.config.js`. Use CSS variables or `@theme` directives in `src/index.css`.
- **Imports**: `@import "tailwindcss";` in `src/index.css`.
- **Class Names**: Use v4 syntax (e.g., `gap-*` instead of `space-x-*`, no `divide-*`).
- **Utilities**:
  - Use `cn(...)` for merging classes.
  - Use `class-variance-authority` (CVA) for variant APIs.
  - Use `tailwind-variants` for slot-based multi-element styling.
- **Best Practices**:
  - Prefer standard spacing scale (`p-4`) over arbitrary values (`p-[16px]`).
  - Ensure color contrast meets WCAG AA standards.
  - Use `aria-*` attributes for interactive elements.
- **Font Family**:
  - `--heading-font-family` is applied to `h1–h6` via `@layer base` in `src/index.css`.
  - `--default-font-family` is the default body font via `@theme`.
  - **Avoid redundant classes**: Never use `font-[var(--heading-font-family)]`, `font-(--heading-font-family)`, or similar on headings/body.
  - For non-heading elements needing the heading font, update `@layer base` or define a utility in `@theme`.

## 6. Component Patterns

### `cn` Utility
Use for simple components with conditional classes.
```tsx
import { cn } from '@/lib/utils'
// className={cn('base', active && 'active', className)}
```

### Variant API (CVA)
Use for single-element components with multiple variants.
```tsx
import { cva } from 'class-variance-authority'
// const buttonVariants = cva(...)
```

### Slot-Based (Tailwind Variants)
Use for multi-element components.
```tsx
import { tv } from 'tailwind-variants'
// const card = tv({ slots: { ... }, variants: { ... } })
```

## 7. Asset Management

- **Location**: Place images in `src/assets/images/`.
- **Eager Loading**: `import { getImage } from '@/lib/image'` (returns string).
- **Lazy Loading**: `import { getImageAsync } from '@/lib/imageAsync'` (returns Promise).
- **Galleries**: `getAllImages()` or `getAllImagesAsync()`.
- **Extensions**: Can be omitted (auto-resolves).

## 8. Critical Rules & Pitfalls

1.  **Package Manager**: Use **pnpm** (default); npm/yarn/bun also work. Replace `<pm>` with your package manager.
2.  **Auto-Generated Files**: Do not modify `src/routeTree.gen.ts`.
3.  **Tailwind Config**: Do not create `tailwind.config.js`. Use `src/index.css`.
4.  **Class Merging**: Always use `cn()` when accepting a `className` prop.
5.  **Dependencies**: Use `<pm> add` to install new packages.