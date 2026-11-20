# React Component Rules

Follow these rules strictly whenever creating or modifying React components under `src/components/**`.

---

## 1. Props Interface

- Every component **MUST** define a TypeScript `interface` for its props (use `interface`, not `type` unless a union type is strictly required).
- Name the interface `<ComponentName>Props` (e.g., `ButtonProps`, `UserCardProps`).
- Every props interface **MUST** include an optional `id` property (`id?: string;`), even if the component accepts no other props.
- Pass `id` to the component's root rendered element when applicable.

```tsx
export interface ButtonProps {
  id?: string;
  label: string;
  onClick?: () => void;
}

function Button({ id, label, onClick }: ButtonProps) {
  return (
    <button id={id} onClick={onClick}>
      {label}
    </button>
  );
}

export default Button;
```

---

## 2. File Structure & Exports

### Single Component per File (Default)
- Prefer placing each component in its own dedicated `.tsx` file.
- A file containing a single component **MUST** export that component as the **default export**.

```tsx
// src/components/UserBadge.tsx
export interface UserBadgeProps {
  id?: string;
  name: string;
}

function UserBadge({ id, name }: UserBadgeProps) {
  return <span id={id}>{name}</span>;
}

export default UserBadge;
```

### Multiple Components in One File (Exception)
- When multiple closely related components exist in the same file, **each component MUST be a named export**.
- Do not mix a default export with additional component exports in a multi-component file.

```tsx
// src/components/CardParts.tsx
export interface CardHeaderProps {
  id?: string;
  title: string;
}

export function CardHeader({ id, title }: CardHeaderProps) {
  return <header id={id}>{title}</header>;
}

export interface CardBodyProps {
  id?: string;
  children: React.ReactNode;
}

export function CardBody({ id, children }: CardBodyProps) {
  return <div id={id}>{children}</div>;
}
```

---

## 3. Barrel Exports (`index.ts`) & Imports

- Every new component **MUST** be exported from an `index.ts` barrel file so consumers can import components cleanly from `@components`.
- If components are organized into subdirectories (e.g., `src/components/icons/`), export them from the subdirectory's `index.ts` and re-export from `src/components/index.ts`.
- Re-export default-exported components as named exports, and re-export named components directly:

```ts
// src/components/index.ts
export { default as Button } from "./Button";
export { default as UserBadge } from "./UserBadge";
export { CardHeader, CardBody } from "./CardParts";
export * from "./icons";
```

- When importing components in other files, **ALWAYS** import from `@components` instead of deep relative or file-level paths:

```tsx
// Good
import { Button, UserBadge, IconClose } from "@components";

// Avoid
import Button from "../../components/Button";
import IconClose from "../../components/icons/IconClose";
```

*(Exception: Components inside `src/components/` importing a sibling helper or component may use relative imports if needed to avoid circular dependencies.)*

---

## Verification Checklist

Before finishing any component task, verify:

- [ ] Component has a dedicated props `interface` (e.g., `MyComponentProps`).
- [ ] Props interface includes `id?: string;`.
- [ ] Single-component file uses a `default` export; multi-component file uses named exports for every component.
- [ ] Component is added to `index.ts` and accessible via `import { ... } from "@components"`.

