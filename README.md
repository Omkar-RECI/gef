# Global Entrepreneurs Forum Website

Official website repository for **Global Entrepreneurs Forum (GEF)**.

This README is written as a practical **developer handover / KT document** so any new developer can quickly understand:

- what this project is
- how to run it
- where content lives
- where images live
- which files should be edited for which pages
- what parts are still draft / manual

## 1. Project Overview

GEF is a multi-page marketing and event website built for:

- brand presence
- upcoming event promotion
- past edition archives
- speaker showcase
- media and gallery presentation
- partner / sponsor visibility
- direct contact and inquiry flow

This codebase is now being maintained manually. It was originally scaffolded with Lovable-related setup, but day-to-day edits are expected to be handled directly in code.

## 2. Tech Stack

- React 19
- TypeScript
- Vite
- TanStack Start
- TanStack Router
- Tailwind CSS v4
- Lucide React icons
- Wrangler / Cloudflare deployment support

## 3. Useful Commands

Install dependencies:

```bash
npm install
```

Run local development:

```bash
npm run dev
```

Build production bundle:

```bash
npm run build
```

Preview production build locally:

```bash
npm run preview
```

Run lint:

```bash
npm run lint
```

Format code:

```bash
npm run format
```

Cloudflare deploy:

```bash
npm run cf:deploy
```

Cloudflare local worker dev:

```bash
npm run cf:dev
```

## 4. Folder Structure

Top-level folders:

- `src/` : application source code
- `public/` : static public assets like uploaded images
- `.lovable/` : old Lovable project metadata
- `.output/`, `.wrangler/`, `.tanstack/` : generated build/runtime folders

Important source folders:

- `src/routes/` : all route/page files
- `src/content/` : structured page content for speakers, upcoming events, and past editions
- `src/components/gef/` : shared branded layout pieces like header, footer, CTA, announcement bar
- `src/components/ui/` : reusable UI primitives
- `src/lib/` : SEO helpers and utility code
- `src/assets/gef/` : local code-imported brand assets

Important public asset folders:

- `public/Assets/Gallary/` : homepage / media gallery images
- `public/Assets/Speakers/` : speaker photos
- `public/Assets/Leaders/` : leadership / patron / mentor photos
- `public/Assets/Associated-Logos/` : ecosystem partner logos
- `public/Assets/Events/` : event-specific images, media, gallery, and speaker assets

## 5. Core Architecture

This project follows a simple split:

1. **Route files** handle page layout and rendering
2. **Content files** hold structured data
3. **Public assets** hold uploaded images/logos
4. **Shared components** keep repeated UI consistent

That means a developer should avoid putting large hard-coded page data directly into random component files unless it is truly homepage-only or shared UI behavior.

## 6. Main Route Files

These are the most important route files:

- `src/routes/__root.tsx`
  - app shell
  - global SEO defaults
  - header/footer
  - announcement bar

- `src/routes/index.tsx`
  - homepage
  - hero slider
  - impact stats
  - about teaser
  - event section
  - ecosystem sections
  - gallery section

- `src/routes/about-gef.tsx`
  - About GEF page

- `src/routes/upcoming-events.tsx`
  - upcoming events listing page

- `src/routes/gef-pune-2026.tsx`
  - current detailed upcoming event page

- `src/routes/past-editions/index.tsx`
  - past editions listing page

- `src/routes/past-editions/$slug.tsx`
  - dynamic past edition detail page renderer

- `src/routes/speakers.tsx`
  - speaker listing page

- `src/routes/partners-sponsors.tsx`
  - partners and sponsors page

- `src/routes/media.tsx`
  - media and gallery page

- `src/routes/contact.tsx`
  - contact page

- `src/routes/sitemap[.]xml.ts`
  - generated XML sitemap route

## 7. Content Ownership Rules

This is the most important KT point.

### Shared site-wide data

Use:

- `src/data/gef.ts`

For content reused across multiple pages or homepage sections, for example:

- homepage impact stats
- why attend cards
- gallery collections
- shared FAQs
- shared partnership labels
- shared press/media arrays

### Speakers

Use:

- `src/content/speakers.ts`

This file is the main profile source for:

- speakers page
- homepage key figure references
- past edition speaker filtering helpers

If a speaker photo, designation, organization, or country changes, this is usually the first file to check.

### Upcoming events

Use:

- `src/content/upcoming-events/index.ts`

This file controls:

- upcoming event listing page
- homepage event slider references
- whether an event has its own detail page

Important field:

- `hasEventPage`

If `true`, the event can link to its own page like `/gef-pune-2026`.
If `false`, it stays listing-only.

### Past editions

Use:

- `src/content/past-editions/index.ts`
- `src/content/past-editions/gec-pune-2025.ts`
- `src/content/past-editions/gec-dubai-2025.ts`
- `src/content/past-editions/shared.ts`

Rules:

- each event should own its own content file
- route rendering is shared through `src/routes/past-editions/$slug.tsx`
- shared transformation helpers for leadership / speaker cards live in `shared.ts`

Do not turn `index.ts` into a giant content file. It should stay a registry.

## 8. How Dynamic Past Edition Pages Work

Past edition detail pages are rendered by:

- `src/routes/past-editions/$slug.tsx`

It looks up event content from:

- `src/content/past-editions/index.ts`

That index imports detailed content objects like:

- `GEC_PUNE_2025_CONTENT`
- `GEC_DUBAI_2025_CONTENT`

So if a new past edition is added, normal flow is:

1. create a new content file in `src/content/past-editions/`
2. export its content object
3. register it in `src/content/past-editions/index.ts`
4. confirm it appears on the listing page and dynamic route

## 9. How Upcoming Event Pages Work

The main current detailed upcoming event page is:

- `src/routes/gef-pune-2026.tsx`

The listing cards for all upcoming events are controlled by:

- `src/content/upcoming-events/index.ts`

If in future another upcoming event gets its own page:

1. create a new route file in `src/routes/`
2. keep the URL simple, for example `/gef-phuket-april-2027`
3. update `hasEventPage: true` in the event archive

## 10. Homepage Content Map

Homepage file:

- `src/routes/index.tsx`

This file contains a lot of custom homepage-only sections, so it is one of the largest files in the repo.

Typical homepage updates happen here:

- hero slider images
- impact bar layout
- about preview section
- event section
- ecosystem visualization
- ecosystem partner section
- key figures section
- homepage gallery section

If a section is homepage-only, it is acceptable for that section logic to live inside `index.tsx`.

## 11. Image Handling Rules

### A. Public folder images

If the image should be easy to replace later without code import changes, keep it under `public/Assets/...` and reference it like:

```ts
"/Assets/Speakers/example.jpg"
```

Use this for:

- speaker photos
- leader photos
- event galleries
- sponsor logos
- media clippings

### B. Imported source assets

If the image is part of UI structure and imported directly into code, keep it in:

- `src/assets/gef/`

Use this for:

- header/footer logos
- hero banners imported via Vite
- code-managed brand assets

### C. Best practice for this repo

For future maintainability:

- profile photos -> `public/Assets/Speakers/` or `public/Assets/Leaders/`
- event photos -> `public/Assets/Events/<Event Name>/`
- shared gallery images -> `public/Assets/Gallary/`
- logos -> `public/Assets/Associated-Logos/`

## 12. SEO / Search Setup

SEO helpers live in:

- `src/lib/seo.ts`

Global SEO shell is wired in:

- `src/routes/__root.tsx`

Page-level SEO is added through:

- `createSeoHead(...)`

Already implemented:

- page titles
- descriptions
- canonical URLs
- Open Graph fields
- schema helpers
- XML sitemap route
- `robots.txt`
- `llms.txt`
- `llms-full.txt`

If a developer creates a new public page, they should also:

1. add proper `createSeoHead(...)`
2. add relevant schema if needed
3. ensure sitemap includes the page if applicable

## 13. Forms / Functional Limitations

This is still an important operational note.

Some UI flows are currently presentation-first and may not be fully connected to backend systems.

Developers should manually verify before claiming any flow is production-ready:

- contact form submission behavior
- partner inquiry flow
- registration CTA behavior
- brochure / document downloads
- legal links

## 14. Lovable / Legacy Notes

This project still contains some Lovable-era setup.

Examples:

- `.lovable/`
- `@lovable.dev/vite-tanstack-config`
- `src/lib/lovable-error-reporting.ts`

Current recommendation:

- do not depend on Lovable for normal edits
- keep the project manually maintainable
- remove Lovable-specific dependencies only in a careful cleanup task, not casually

## 15. Files Developers Should Usually Not Edit Manually

- `src/routeTree.gen.ts`
  - generated file

- `.output/`
  - build output

- `.tanstack/`
  - generated tooling folder

- `.wrangler/`
  - generated deployment/runtime files

These are generated or environment-specific and should not be used as hand-edited source of truth.

## 16. How To Add A New Speaker

1. Upload photo to:
   - `public/Assets/Speakers/`
2. Add profile object in:
   - `src/content/speakers.ts`
3. Fill:
   - `name`
   - `designation`
   - `organization`
   - `country` if outside India
   - `photo`
4. Check:
   - speakers page
   - homepage key figure / testimonial references if used
   - any past edition filtering logic if relevant

## 17. How To Add A New Past Edition

1. Add event images under:
   - `public/Assets/Events/<Event Name>/`
2. Create:
   - `src/content/past-editions/<slug>.ts`
3. Export event content object
4. Register in:
   - `src/content/past-editions/index.ts`
5. Confirm:
   - `/past-editions`
   - `/past-editions/<slug>`
   - sitemap route if needed

## 18. How To Add A New Upcoming Event

1. Add event card data in:
   - `src/content/upcoming-events/index.ts`
2. If detail page is needed:
   - create route in `src/routes/`
   - set `hasEventPage: true`
3. If detail page is not ready:
   - keep `hasEventPage: false`

## 19. Current Live Content Notes

As of **Wednesday, August 19, 2026**, the repo contains:

- upcoming event listings for 2026 and 2027
- detailed page for `GEF Pune Dec 2026`
- past edition pages for:
  - `GEC Pune 2025`
  - `GEC Dubai 2025`

Developers should treat dates, venues, speakers, partner lists, and media entries as **content-managed information** that must be verified before publishing changes.

## 20. Common Developer Starting Points

If task is about...

- homepage sections -> `src/routes/index.tsx`
- header/footer -> `src/components/gef/Header.tsx`, `src/components/gef/Footer.tsx`
- event archive cards -> `src/content/upcoming-events/index.ts`, `src/content/past-editions/index.ts`
- speaker profiles -> `src/content/speakers.ts`
- Dubai/Pune detail pages -> their respective files in `src/content/past-editions/`
- SEO -> `src/lib/seo.ts`, page route `head()` blocks
- favicon/global head tags -> `src/routes/__root.tsx`
- static image replacement -> `public/Assets/...`

## 21. Deployment Notes

This repo is prepared for Cloudflare-style deployment via Wrangler.

Relevant files:

- `wrangler.jsonc`
- `package.json` scripts

Before deployment:

1. run `npm run build`
2. verify no broken links or missing assets
3. confirm event content is correct
4. confirm forms and email/contact details are correct

## 22. Recommended Working Style For Future Devs

- keep page-specific content close to that page's content file
- do not add random data blobs into unrelated files
- prefer public asset paths for easy image swapping
- keep route files focused on rendering/layout
- keep generated files untouched
- test mobile and desktop after every UI change
- run build before handing over work

## 23. Quick KT Summary

If a new developer joins, they should understand this repo in one line:

**Routes render the pages, content files hold structured business data, and public assets hold the images that non-technical teams will replace most often.**

## 24. Final Note

This repository is already in a workable manual-maintenance state, but it should still be treated like a living event website:

- content changes often
- images will keep changing
- event pages will keep growing
- clarity of ownership matters more than clever abstractions

When in doubt, prefer:

- simple structure
- clear file ownership
- explicit content placement
- easy handover for the next developer
