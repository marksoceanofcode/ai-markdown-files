# Environment Variables Rules

Follow these rules strictly during project setup and whenever adding, reading, or updating environment variables in `.env.local` and `.env.example`.

Also follow [naming-conventions.md](./naming-conventions.md) for constant naming conventions.

---

## 1. Create Missing Environment Files

- Check the project root for both `.env.local` and `.env.example`.
- If either `.env.local` or `.env.example` does not exist yet, **create it**.
- Ensure `.env.local` is ignored by Git (verify that `.gitignore` includes `.env.local` or `.env*.local`).

---

## 2. Keeping `.env.local` and `.env.example` in Sync

- `.env.local` stores real local development values and secrets and **MUST NEVER** be committed to version control.
- `.env.example` is the committed template documenting required environment variables.
- Whenever a new environment variable is added to `.env.local` or referenced in code, **ALWAYS** add the same key to `.env.example` with a safe placeholder value.
- **NEVER** put real secrets, API keys, tokens, or passwords in `.env.example`.

---

## 3. Naming & Exposure

- Environment variable keys **MUST** use `UPPER_SNAKE_CASE` (e.g., `DATABASE_URL`, `API_SECRET_KEY`).
- Keep secrets, database credentials, and private API keys server-only by **omitting** the `NEXT_PUBLIC_` prefix.
- Only prefix a variable with `NEXT_PUBLIC_` when the value is intentionally safe to expose in client-side browser bundles.

---

## 4. Example

```dotenv
# .env.example (committed template — placeholders only)
DATABASE_URL="postgresql://user:password@localhost:5432/app_db"
INTERNAL_API_KEY="your-internal-api-key"
NEXT_PUBLIC_APP_URL="http://localhost:3000"
```

```dotenv
# .env.local (local only — gitignored)
DATABASE_URL="postgresql://postgres:localdev@localhost:5432/my_app_dev"
INTERNAL_API_KEY="sk_test_example_local_value"
NEXT_PUBLIC_APP_URL="http://localhost:3000"
```

---

## Verification Checklist

Before finishing any task involving environment setup or environment variables, verify:

- [ ] Both `.env.local` and `.env.example` exist in the project root (created if missing).
- [ ] `.env.local` is covered by `.gitignore` and not tracked by Git.
- [ ] `.env.example` contains every key present in `.env.local` with safe placeholder values only.
- [ ] All environment variable keys use `UPPER_SNAKE_CASE`, and only client-safe values use the `NEXT_PUBLIC_` prefix.

