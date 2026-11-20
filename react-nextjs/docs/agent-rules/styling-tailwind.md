# Styling & Tailwind CSS Rules

Follow these rules strictly whenever styling JSX/TSX elements across the project.

---

## 1. Preferred CSS Framework

- Use **Tailwind CSS** utility classes as the default styling approach.
- Avoid writing custom CSS files or inline `style` objects unless a style cannot be cleanly expressed with Tailwind utilities (e.g., dynamic runtime pixel values).

---

## 2. `className` Ordering Rule (Left to Right)

Always order Tailwind classes inside `className` into **three distinct groups**, from left to right:

1. **Base (unprefixed) classes** — sorted strictly in **alphabetical order** (`a` → `z`).
2. **State / pseudo-class variants** (e.g., `active:`, `disabled:`, `focus:`, `focus-visible:`, `hover:`) — placed **after** all base classes, sorted alphabetically by variant prefix and then alphabetically by utility within the same variant.
3. **Media query / responsive breakpoints** — placed **last**, ordered from **smallest to largest** breakpoint:
   `xs:` (if configured) → `sm:` → `md:` → `lg:` → `xl:` → `2xl:`
   Within each breakpoint, sort utilities alphabetically (base utilities for that breakpoint before responsive+state variants like `md:hover:...`).

### Summary Order

```text
[base classes: a → z]  [state variants: active:, disabled:, focus:, hover:, ...]  [breakpoints: xs: → sm: → md: → lg: → xl: → 2xl:]
```

---

## 3. Examples

```tsx
// Good
<button
  id={id}
  className="bg-blue-600 cursor-pointer flex font-medium items-center px-3 py-2 rounded-md text-sm text-white active:bg-blue-800 disabled:opacity-50 focus:outline-none focus:ring-2 hover:bg-blue-700 xs:px-4 sm:px-5 sm:text-base md:px-6 lg:px-8"
>
  Submit
</button>
```

```tsx
// Avoid (unsorted base classes, states mixed with breakpoints, breakpoints out of order)
<button
  id={id}
  className="flex md:px-6 hover:bg-blue-700 text-white bg-blue-600 sm:px-5 rounded-md focus:ring-2 px-3 py-2"
>
  Submit
</button>
```

### Multi-line or Conditional Classes
When using helper utilities (such as `clsx`, `cn`, or template literals), maintain the same left-to-right ordering within each class string and keep base classes before state classes before responsive classes:

```tsx
// Good
<div
  className={[
    "bg-white border flex flex-col gap-2 p-4 rounded-lg",
    "focus-within:border-blue-500 hover:shadow-md",
    "sm:gap-4 sm:p-6 md:flex-row lg:p-8",
    isActive ? "border-blue-600 text-blue-700" : "border-gray-200 text-gray-800",
  ].join(" ")}
/>
```

---

## Verification Checklist

Before finishing any UI or styling task, verify:

- [ ] Tailwind CSS utilities are used for styling.
- [ ] Base classes come first and are sorted alphabetically from left to right.
- [ ] State variants (`active:`, `disabled:`, `focus:`, `hover:`, etc.) come after base classes, sorted alphabetically.
- [ ] Responsive breakpoint classes come last, ordered from smallest to largest (`xs:` → `sm:` → `md:` → `lg:` → `xl:` → `2xl:`).

