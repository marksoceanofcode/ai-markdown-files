# API Routes Rules

Follow these rules strictly whenever creating or modifying API route handlers under `src/app/api/**` (or `src/pages/api/**` if the project uses Pages Router).

Also follow [naming-conventions.md](./naming-conventions.md), [environment.md](./environment.md), and [utils.md](./utils.md).

---

## 1. Folder Location

- All API routes **MUST** be created inside the `api` folder (`src/app/api/<endpoint>/route.ts` in Next.js App Router, or `src/pages/api/<endpoint>.ts` in Pages Router).
- Do not place API endpoint handlers outside the `api` folder.

---

## 2. What Belongs in an API Route

Keep logic inside an API route **only** when it genuinely belongs at the API layer, such as:

- HTTP method handling (`GET`, `POST`, `PUT`, `PATCH`, `DELETE`).
- Request parsing, header/cookie inspection, and payload validation.
- Authentication, authorization, and session checks.
- Server-only operations (database queries, third-party API requests using private secrets from `.env.local`, webhook verification).
- Formatting HTTP responses, headers, and status codes (`NextResponse.json(...)`).

---

## 3. Move Non-API Logic to `src/utils/**`

- Logic that does **NOT** need to live in an API route (such as data transformations, formatting, reusable calculations, or general helpers) **MUST** be placed in `src/utils/**` and imported into the route handler.
- Follow [utils.md](./utils.md) when organizing, naming, and splitting utility functions.
- Keep route handlers thin and focused on HTTP orchestration.

---

## 4. Example

```ts
// src/app/api/users/route.ts
import { NextRequest, NextResponse } from "next/server";
import { sanitizeUserEmail } from "@/utils/sanitizeUserEmail";

const EXTERNAL_USERS_ENDPOINT = process.env.EXTERNAL_USERS_API_URL ?? "";
const INTERNAL_API_KEY = process.env.INTERNAL_API_KEY ?? "";

export async function POST(request: NextRequest) {
  const body = await request.json();
  const normalizedEmail = sanitizeUserEmail(body.email);

  if (!normalizedEmail) {
    return NextResponse.json({ error: "Valid email is required." }, { status: 400 });
  }

  const upstreamResponse = await fetch(EXTERNAL_USERS_ENDPOINT, {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
      Authorization: `Bearer ${INTERNAL_API_KEY}`,
    },
    body: JSON.stringify({ email: normalizedEmail }),
  });

  if (!upstreamResponse.ok) {
    return NextResponse.json({ error: "Failed to create user." }, { status: 502 });
  }

  const createdUser = await upstreamResponse.json();
  return NextResponse.json({ data: createdUser }, { status: 201 });
}
```

---

## Verification Checklist

Before finishing any API route task, verify:

- [ ] The route handler lives inside the `api` folder (`src/app/api/**` or `src/pages/api/**`).
- [ ] Only API-layer logic (HTTP handling, validation, auth, server-only calls, response formatting) lives in the route file.
- [ ] Any reusable or non-API logic has been placed in `src/utils/**` following `utils.md`.
- [ ] Any new environment variables used by the route are added to both `.env.local` and `.env.example` per `environment.md`.

