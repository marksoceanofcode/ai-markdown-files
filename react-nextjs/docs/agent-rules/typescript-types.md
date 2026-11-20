# TypeScript Types Rules

Follow these rules strictly whenever creating, modifying, or using shared types under `src/types/**`.

---

## 1. Check Existing Types First

- Before creating any new type, **ALWAYS** inspect `src/types/**` (starting with `src/types/index.ts`) to see if an existing type already fits your need.
- Reuse existing types directly whenever possible.
- If a slight variation is needed, prefer composing or deriving from an existing type (e.g., intersections `&`, `Pick`, `Omit`, `Partial`) instead of duplicating overlapping definitions.

---

## 2. Prefer `type` Over `interface`

- **ALWAYS** use `type` by default for definitions in `src/types/**`.
- Only use `interface` when there is a specific technical need for it (for example: declaration merging, module augmentation, or a contract explicitly implemented by a class).

---

## 3. Grouping Related Types

- Group closely related types into the same file where it makes logical sense (e.g., by domain or feature such as `user.ts`, `auth.ts`, or `api.ts`).
- Place a type in its own standalone file only when it does not naturally belong with other types.

---

## 4. File Structure & Exports

### Single Type per File
- If a file defines only **one** type, it **MUST** use a **default export**.
- Declare the `type` first and then export it as default (`export default type ...` is not valid TypeScript syntax).

```ts
// src/types/theme.ts
type ThemeMode = "light" | "dark" | "system";

export default ThemeMode;
```

```ts
// src/types/pagination.ts
type Pagination = {
  page: number;
  pageSize: number;
  totalItems: number;
};

export default Pagination;
```

### Multiple Types in One File
- If a file defines **multiple** related types, **each type MUST be exported individually as a named export**.
- Do not mix a default export with named exports in a multi-type file.

```ts
// src/types/user.ts
export type UserRole = "admin" | "editor" | "viewer";

export type User = {
  id: string;
  name: string;
  email: string;
  role: UserRole;
};

export type UserProfile = User & {
  avatarUrl?: string;
  bio?: string;
};
```

---

## 5. Barrel Exports (`index.ts`) & Imports

- Every type file **MUST** be re-exported from `src/types/index.ts` (and any intermediate subdirectory `index.ts` if subdirectories are used).
- For **single-type files** (default exports), re-export them as named types using `export type { default as ... }` (safe with Next.js `isolatedModules`).
- For **multi-type files** (named exports), either re-export them individually or use a wildcard `export *` re-export:

```ts
// src/types/index.ts
export type { default as ThemeMode } from "./theme";
export type { default as Pagination } from "./pagination";
export * from "./user";
```

- When importing shared types in other files, **ALWAYS** import from `@types` on a single import line instead of deep relative or file-level paths:

```ts
// Good
import { User, UserRole, ThemeMode, Pagination } from "@types";

// Avoid
import { User, UserRole } from "../../types/user";
import ThemeMode from "../../types/theme";
```

*(Note: Use `import type { ... } from "@types"` if the project enables `verbatimModuleSyntax`. Files inside `src/types/` referencing sibling type files may use relative imports to avoid circular dependencies.)*

---

## Verification Checklist

Before finishing any task involving shared types, verify:

- [ ] Checked `src/types/**` first to ensure no existing type could be reused or composed.
- [ ] Used `type` rather than `interface` (unless a specific need like declaration merging or class implementation applies).
- [ ] Related types are grouped cleanly into the same file where it makes sense.
- [ ] Single-type files use a `default` export; multi-type files use individual named exports for every type.
- [ ] All new types are exported from `src/types/index.ts` (named re-export or `export *`) and accessible via `import { ... } from "@types"`.
