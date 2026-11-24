# Utility Functions Rules

Follow these rules strictly whenever creating, modifying, or organizing utility functions under `src/utils/**`.

Also follow [naming-conventions.md](./naming-conventions.md) for `camelCase` function/variable names and `UPPER_SNAKE_CASE` file-level constants.

---

## 1. When to Use `src/utils/**`

- Place reusable logic and business or data helpers that do **not** need to live inside an API route or React component in `src/utils/**`.
- Before writing a new utility function, **ALWAYS** inspect `src/utils/**` first to see if an existing utility already fits or can be reused.

---

## 2. File Organization

- **Prefer one function per file**, matching the exported function name in `camelCase` (e.g., `src/utils/formatCurrency.ts`, `src/utils/slugify.ts`).
- Multiple closely related utilities **MAY** share a common file (e.g., `src/utils/dateUtils.ts`, `src/utils/stringUtils.ts`) when grouping them together makes clear organizational sense.

---

## 3. Single Responsibility

- Functions should generally **do one thing** cleanly, with a clear `camelCase` name and focused responsibility.
- If a function grows to handle multiple distinct tasks, split those tasks into separate single-purpose functions.

---

## 4. Exclusive vs. Reusable Helper Functions

- **Exclusive helpers**: A utility file **MAY** contain private (unexported) helper functions used exclusively by that utility to keep the main function readable.
- **Extract reusable helpers**: If an existing or new helper function could be used in multiple places across the codebase, **break it out into its own standalone exported utility function** (preferring its own file under `src/utils/**`, or an appropriate shared utility file) rather than duplicating it or leaving it trapped inside another utility.

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

- [ ] Checked `src/utils/**` first to avoid duplicating existing utilities.
- [ ] Non-API logic lives in `src/utils/**` rather than inside API routes or components.
- [ ] Each utility preferably lives in its own file (or shares a common file only with closely related utilities).
- [ ] Each function does one thing cleanly and follows `camelCase` naming (with `UPPER_SNAKE_CASE` for file-level constants).
- [ ] Private helpers are truly exclusive to one file, and any helper reusable in multiple places has been broken out into its own exported utility function.

