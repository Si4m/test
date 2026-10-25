# Eliyas Classroom — template foundation (Phase 1, multi-page)

React + Vite + Tailwind v4 + React Router. Design system + inline-editing
foundation from `INLINE_EDITING_ROADMAP.md`, built against
`RAKIBS_CLASSROOM_CLONE_SPEC.md` for visual reference. Backend is
intentionally stubbed out — see "The backend swap" below.

## Run it ok

```
npm install
npm run dev
```

Top-right of the nav: **Admin mode / Student view** toggle. Flip it to see
the exact same page render with or without edit affordances — that's the
whole "no separate admin panel" architecture in one switch.

Every entity on every page is fully editable, not just decorative:

- **Home** — hero text, logo, brand name, teacher photos all editable in place
- **Courses** — add/edit/delete courses, swap thumbnails, filter pills work
- **Exam** — add/edit/delete exams (title, batch, status, questions/duration/marks)
- **Store** — add/edit/delete books, swap covers, price with compare-at
- **About** — reuses the faculty row (edit teacher name/institution/subject/years)

Nothing here is a static mock screen with fake "coming soon" text — every
card has a working kebab menu (Edit/Delete) and every grid has a working
"+ Add" tile, backed by real (if temporary) CRUD against `dataService.js`.
Changes persist across reloads via localStorage.

## The backend swap

`src/lib/dataService.js` is the *only* file that talks to storage. Every
function (`siteSettings.get/upsert`, `media.upload`, `courses/teachers/
exams/books.list/create/update/remove`) is written to the shape the real
backend will have. When Supabase access is given, replace the internals
of this one file — no component in `src/components` or `src/pages` needs
to change.

## What's here

```
src/
  lib/
    AdminContext.jsx  — the isAdmin switch every editable component reads
    dataService.js    — STUB backend (localStorage-backed for now)
    mockData.js       — seed content matching the reference site's shapes
  components/
    ui/
      EditableText.jsx  — Category A (site_settings-backed text)
      EditableImage.jsx — Category B (media-bucket-backed images)
      Modal.jsx         — the one modal shape, reused by every entity
      AddCardTile.jsx   — the "+ Add X" tile, admin-only
      EntityMenu.jsx    — the Edit/Delete kebab every entity card gets
      Button.jsx, Card.jsx, Badge.jsx, PriceTag.jsx
    layout/
      Navbar.jsx — routed nav, logged-out vs logged-in variants
      Footer.jsx — Running Batches pulled from real course data, never hardcoded
    sections/
      TeachersGrid.jsx, FacultyRow.jsx, QuickLinks.jsx, ContactStrip.jsx
  pages/
    Home.jsx, Courses.jsx, Exam.jsx, Store.jsx, About.jsx
  App.jsx — router shell (shared Navbar/Footer + <Routes>)
```

`public/_redirects` + `public/_headers` (work on Cloudflare Pages and Netlify) and `vercel.json` (Vercel) are included so
client-side routes like `/courses` don't 404 on a hard refresh once deployed.

## Design tokens

`src/index.css` — dark theme extracted from the reference screenshots.
Edit the `@theme` block to retint the whole app for a future client
rebrand instead of hunting down hardcoded colors per component.

## What's NOT here yet (by design)

- No Supabase client anywhere — see "The backend swap" above.
- No auth — `Navbar`'s `loggedIn` prop and the demo user object are
  placeholders for real session state.
- Subject → Chapter → Lesson drill-down, Dashboard, and the Lesson/
  Doubt-Solve page aren't built yet — Phase 2 territory.
- Category D (section-level page builder / `page_sections`) is Phase 3.
- Store's "Combined" (bundle) tab is a placeholder note — bundles reuse
  this same grid+modal pattern once bundle grouping is defined.
- Android app promo section and the AI Study Partner section are
  intentionally excluded per the agreed template scope.

## Next steps

Dashboard + course-detail + lesson/doubt-solve pages (the logged-in
Library area), or the Subject→Chapter→Lesson drill-down under Courses —
pick whichever to continue with.
