# Utility Functions Rules

Follow these rules strictly whenever creating, modifying, or organizing reusable utility functions under `src/utils/**`.

Also follow [naming-conventions.md](./naming-conventions.md) for `camelCase` function/variable names and `UPPER_SNAKE_CASE` file-level constants.

---

## 1. Purpose & Reuse First

- Use `src/utils/**` for focused, reusable functions that primarily serve a single purpose across the application (such as formatting, parsing, calculations, or string/date helpers).
- Before writing a new utility function, **ALWAYS** inspect `src/utils/**` first to see if an existing utility already exists and can be reused.

---

## 2. Single Responsibility

- Functions should primarily **do one thing** cleanly, with a descriptive `camelCase` name and predictable inputs and outputs.
- If a function grows to handle multiple distinct responsibilities, split those tasks into separate single-purpose functions.

---

## 3. File Organization

- **Prefer one function per file**, matching the exported function name in `camelCase` (e.g., `src/utils/formatCurrency.ts`, `src/utils/slugify.ts`).
- Multiple closely related utilities **MAY** share a common file (e.g., `src/utils/dateUtils.ts`, `src/utils/stringUtils.ts`) when grouping them together makes clear organizational sense.

---

## 4. Exclusive vs. Reusable Helper Functions

- **Exclusive helpers**: A utility file **MAY** contain private (unexported) helper functions used exclusively by that utility to keep the main function readable.
- **Extract reusable helpers**: If an existing or new helper function could be used multiple times across the application, **break it out into its own standalone exported utility function** (preferring its own file under `src/utils/**`, or an appropriate shared file) rather than duplicating it or leaving it hidden inside another utility.

---

## 5. Examples

### Single-Function File with an Exclusive Private Helper

```ts
// src/utils/formatCurrency.ts
const DEFAULT_LOCALE = "en-US";
const DEFAULT_CURRENCY = "USD";

// Exclusive to formatCurrency — keep unexported in this file
function normalizeAmount(rawAmount: number): number {
  if (!Number.isFinite(rawAmount)) {
    return 0;
  }

  return Math.round(rawAmount * 100) / 100;
}

export function formatCurrency(amount: number, currency = DEFAULT_CURRENCY): string {
  const safeAmount = normalizeAmount(amount);

  return new Intl.NumberFormat(DEFAULT_LOCALE, {
    style: "currency",
    currency,
  }).format(safeAmount);
}
```

### Reusable Helper Extracted to Its Own File

```ts
// src/utils/clampNumber.ts
export function clampNumber(value: number, min: number, max: number): number {
  return Math.min(Math.max(value, min), max);
}
```

```ts
// src/utils/calculateProgressPercent.ts
import { clampNumber } from "./clampNumber";

const MIN_PERCENT = 0;
const MAX_PERCENT = 100;

export function calculateProgressPercent(completed: number, total: number): number {
  if (total <= 0) {
    return MIN_PERCENT;
  }

  const rawPercent = Math.round((completed / total) * MAX_PERCENT);
  return clampNumber(rawPercent, MIN_PERCENT, MAX_PERCENT);
}
```

---

## Verification Checklist

Before finishing any utility function task, verify:

- [ ] Inspected `src/utils/**` first to avoid duplicating existing utilities.
- [ ] Each function focuses on a single purpose reusable across the application and uses `camelCase` naming (with `UPPER_SNAKE_CASE` for file-level constants).
- [ ] Each utility preferably lives in its own file (or shares a common file only with closely related utilities).
- [ ] Private helpers remain truly exclusive to one file, and any helper usable in multiple places has been broken out into its own exported utility function.
