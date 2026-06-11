---
paths:
  - "src/**/*.tsx"
---

# Component Variant Patterns

Three approaches for variant styling — pick by complexity. Working reference implementations live in `src/components/`; read the matching one before writing a new variant component and follow its structure.

| Approach | Use for | Reference |
| --- | --- | --- |
| `cn()` conditionals | Simple components — few conditionals, single element | `src/components/ButtonCn.tsx` |
| CVA (`class-variance-authority`) | Single-element components with multiple variant combinations (buttons, badges) | `src/components/ButtonCva.tsx` |
| `tailwind-variants` (slots) | Multi-element components where variants affect several children (cards, forms, navigation) | `src/components/CardTv.tsx` |

Shared rules:

- Extend native element props with `ComponentProps<'button'>` etc.; derive variant prop types with `VariantProps<typeof variants>`
- Always accept `className` and apply it last — via the final `cn()` argument, or `base({ class: className })` with `tailwind-variants` (its `twMerge` is built in, no `cn()` wrapper needed)
- Export components as named exports
