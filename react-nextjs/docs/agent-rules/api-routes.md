# API Routes Rules

Follow these rules strictly whenever creating or modifying API route handlers under `src/app/api/**` (or `src/pages/api/**` if the project uses Pages Router).

Also follow [naming-conventions.md](./naming-conventions.md) and [environment.md](./environment.md).

---

## 1. Folder Location

- All API routes **MUST** be created inside the `api` folder (`src/app/api/<endpoint>/route.ts` in Next.js App Router, or `src/pages/api/<endpoint>.ts` in Pages Router).
- Do not place API endpoint handlers outside the `api` folder.

---

## 2. Purpose & Scope

Use API routes for server-side endpoints where network or backend orchestration belongs:

- **Requesting other endpoints**: Proxying third-party APIs, calling downstream backend services using private secrets or keys from `.env.local`, and handling incoming webhooks.
- **Endpoint logic**: Handling HTTP methods (`GET`, `POST`, `PUT`, `PATCH`, `DELETE`), parsing and validating requests, checking auth or sessions, performing database operations, and returning structured HTTP responses and status codes.
- **Querying local data (`src/data/**`)**: Reading, filtering, or serving local data files or modules stored in the `data` folder when exposing that data through an API endpoint makes sense.

---

## 3. Examples

### Requesting an External Endpoint

```ts
// src/app/api/users/route.ts
import { NextRequest, NextResponse } from "next/server";

const EXTERNAL_USERS_API_URL = process.env.EXTERNAL_USERS_API_URL ?? "";
const INTERNAL_API_KEY = process.env.INTERNAL_API_KEY ?? "";

export async function POST(request: NextRequest) {
  const body = await request.json();
  const normalizedEmail = typeof body.email === "string" ? body.email.trim().toLowerCase() : "";

  if (!normalizedEmail) {
    return NextResponse.json({ error: "Valid email is required." }, { status: 400 });
  }

  const upstreamResponse = await fetch(EXTERNAL_USERS_API_URL, {
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

### Querying Local Data (`src/data/**`)

```ts
// src/app/api/products/route.ts
import { NextRequest, NextResponse } from "next/server";
import products from "@/data/products";

export async function GET(request: NextRequest) {
  const categoryParam = request.nextUrl.searchParams.get("category");

  const filteredProducts = categoryParam
    ? products.filter((product) => product.category === categoryParam)
    : products;

  return NextResponse.json({ data: filteredProducts }, { status: 200 });
}
```

---

## Verification Checklist

Before finishing any API route task, verify:

- [ ] The route handler lives inside the `api` folder (`src/app/api/**` or `src/pages/api/**`).
- [ ] The route handles genuine endpoint responsibilities (requesting external endpoints, server-side endpoint logic, or querying local data under `src/data/**`).
- [ ] HTTP methods, status codes, and error responses are handled cleanly.
- [ ] Any new environment variables used by the route are added to both `.env.local` and `.env.example` per `environment.md`.
