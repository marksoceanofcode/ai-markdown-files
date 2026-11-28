# Public Folder & Static Assets Rules

Follow these rules strictly whenever adding, organizing, or referencing static image and video files under `public/**`.

---

## 1. Images (`public/images/**`)

- All project image files (`.svg`, `.png`, `.jpg`, `.jpeg`, `.webp`, etc.) **MUST** be placed inside `public/images/` so they can be referenced via `/images/*`.
- **Shared / general images**: Place images used across multiple pages or globally (such as logos or site-wide icons) at the root level of `public/images/` (e.g., `public/images/logo.svg`).
- **Page-specific images**: Place images used on a specific page inside a subfolder matching that page's name/route slug so it is obvious which page they belong to (e.g., employee photos for the `about-us` page belong in `public/images/about-us/`).

---

## 2. Videos (`public/videos/**`)

- Follow the exact same folder organization for video files (`.webm`, `.mp4`, etc.) inside `public/videos/`, referenced via `/videos/*`.
- **Shared / general videos**: Place videos used across multiple pages at the root level of `public/videos/`.
- **Page-specific videos**: Place videos used on a specific page inside a correspondingly named subfolder (e.g., `public/videos/about-us/`).

---

## 3. Referencing Assets in Code

- Always reference public media using root-relative URL paths starting with `/images/` or `/videos/`.
- **NEVER** include `/public` in the asset URL path (Next.js serves everything inside `public/` directly from `/`).

---

## 4. Examples

### Folder Structure

```text
public/
├── images/
│   ├── logo.svg
│   ├── hero-banner.webp
│   └── about-us/
│       ├── team-group.png
│       └── employee-jane.jpg
└── videos/
    ├── brand-intro.mp4
    └── about-us/
        ├── office-tour.webm
        └── office-tour.mp4
```

### Usage in TSX

```tsx
import Image from "next/image";

// Shared image at root of public/images/
<Image src="/images/logo.svg" alt="Company logo" width={140} height={40} />

// Page-specific image under public/images/about-us/
<Image src="/images/about-us/employee-jane.jpg" alt="Jane Doe" width={320} height={320} />

// Page-specific video under public/videos/about-us/
<video controls preload="metadata">
  <source src="/videos/about-us/office-tour.webm" type="video/webm" />
  <source src="/videos/about-us/office-tour.mp4" type="video/mp4" />
</video>
```

---

## Verification Checklist

Before finishing any task involving static images or videos, verify:

- [ ] Image files (`.svg`, `.png`, `.jpg`, `.webp`, etc.) live in `public/images/` and are referenced via `/images/...`.
- [ ] Video files (`.webm`, `.mp4`, etc.) live in `public/videos/` and are referenced via `/videos/...`.
- [ ] Shared/general media is placed at the root of `public/images/` or `public/videos/`.
- [ ] Page-specific media is placed in a subfolder matching the page name (e.g., `public/images/about-us/`, `public/videos/about-us/`).
- [ ] Code references omit `/public` and start with `/images/` or `/videos/`.

