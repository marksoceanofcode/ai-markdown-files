# Naming Conventions

Follow these naming conventions across all TypeScript and TSX files (`src/**`).

---

## 1. Quick Reference

| Identifier Type | Convention | Examples |
| --- | --- | --- |
| Variables (local, props, state, parameters) | `camelCase` | `exampleVariable`, `userCount`, `isLoading` |
| Functions, methods, & hooks | `camelCase` | `exampleFunction`, `fetchUsers`, `useAuth` |
| Global or file-level constants | `UPPER_SNAKE_CASE` | `EXAMPLE_NAME`, `API_BASE_URL`, `MAX_RETRY_COUNT` |
| React components | `PascalCase` | `MyComponent`, `UserCard`, `IconClose` |

---

## 2. Detailed Rules

### Variables & Functions (`camelCase`)
- Use `camelCase` for all variables, function parameters, object properties, functions, methods, and custom hooks.
- Start with a lowercase letter and capitalize each subsequent word.

```ts
const exampleVariable = "hello";
let retryCount = 0;

function exampleFunction(userId: string) {
  const formattedId = userId.trim();
  return formattedId;
}
```

### Global & File-Scope Constants (`UPPER_SNAKE_CASE`)
- Use `UPPER_SNAKE_CASE` (all uppercase with underscores separating words) for constants that are global to the application or scoped to the top level of a file (such as configuration values, limits, keys, and fixed lookup values).
- Do **not** apply `UPPER_SNAKE_CASE` to top-level React components (`PascalCase`) or top-level functions (`camelCase`).

```ts
const API_BASE_URL = "https://api.example.com";
const MAX_ITEMS_PER_PAGE = 25;
const DEFAULT_ROLE = "viewer";
```

### React Components (`PascalCase`)
- All React components **MUST** use `PascalCase` (camelCase with the first letter capitalized), such as `MyComponent`, `PrimaryButton`, or `NavHeader`.
- Single-component files should match the component's `PascalCase` name (e.g., `MyComponent.tsx`).

```tsx
export interface MyComponentProps {
  id?: string;
  title: string;
}

function MyComponent({ id, title }: MyComponentProps) {
  return <section id={id}>{title}</section>;
}

export default MyComponent;
```

---

## Verification Checklist

Before finishing any coding task, verify:

- [ ] Variables and functions use `camelCase` (e.g., `exampleVariable`, `exampleFunction`).
- [ ] Global and file-level constants use `UPPER_SNAKE_CASE` (e.g., `EXAMPLE_NAME`).
- [ ] React components use `PascalCase` (e.g., `MyComponent`).

