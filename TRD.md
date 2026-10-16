# Technical Requirements Document (TRD)

**Product:** A Little World  
**Document status:** Draft v1.0 — recommended architecture  
**Last updated:** 16 October 2026

---

## 1. Technical objective

Build a responsive, private, maintainable web application where design quality and privacy are intentional engineering requirements. The system must support owner-managed content, partner read access, private media, and future-dated letters with enforceable release rules.

This is a recommended baseline, not a claim that any code has already been implemented.

## 2. Recommended stack

| Layer | Recommendation | Reason |
|---|---|---|
| Framework | Next.js App Router + React + TypeScript | Supports a structured application with server-rendered pages and server-side data checks. |
| Styling | Tailwind CSS plus a small layer of custom CSS | Makes design tokens consistent while allowing bespoke editorial components. Avoid a default component-library appearance. |
| Interaction/motion | CSS transitions first; a small motion library only where it provides clear value | Avoids excessive dependencies and makes reduced-motion behavior easier to maintain. |
| Auth/database | Supabase Auth + PostgreSQL with Row Level Security (RLS) | Gives an authenticated user model and database-enforced access policies. |
| File storage | Supabase Storage, private bucket | Photos/audio stay private and are served through authorized policies or short-lived signed URLs. |
| Deployment | Vercel or another supported Node.js deployment | Use server-capable deployment; do not use static export if it prevents the required server-side privacy checks. |
| Source control | Git + remote repository | Enables history, rollback, and reviewable changes. |
| Package management | npm with lockfile committed | Reproducible installs and consistent test results. |

### Version policy

Use current stable, mutually compatible versions at implementation time. Confirm official installation and migration instructions before installing or upgrading packages. Commit the package lockfile. Do not run broad dependency upgrades as part of an unrelated UI change.

## 3. Application architecture

```text
Browser
  └── Next.js UI
      ├── Server-rendered routes and protected data access
      ├── Client components only for interaction (filters, dialogs, audio controls)
      ├── Owner Studio forms
      └── Design system (tokens + components)
             │
             ▼
      Supabase Auth (session/cookies)
             │
             ├── PostgreSQL + RLS
             │     ├── private space membership
             │     ├── memories
             │     ├── exhibits
             │     └── letters and unlock rules
             │
             └── Private Storage bucket
                   └── images and audio assets
```

### Architectural rules

- Prefer server components for route layouts and data loading. Use client components only when browser interaction or local state is required.
- Authorize every protected route and every write operation on the server; hiding buttons is not authorization.
- Keep the Supabase service-role key exclusively in trusted server-side environment variables if it is needed at all. Do not expose it to client code or `NEXT_PUBLIC_*` variables.
- Use the public/anon key only with properly configured RLS and storage policies.
- Keep the project small: do not introduce microservices, a separate API server, Redux, a CMS, or a large design system without evidence that the app needs them.

## 4. Suggested repository structure

```text
src/
  app/
    (auth)/
      login/page.tsx
      auth/callback/route.ts
    (private)/
      layout.tsx
      page.tsx
      memories/page.tsx
      memories/[id]/page.tsx
      museum/page.tsx
      museum/[slug]/page.tsx
      letters/page.tsx
      letters/[id]/page.tsx
      future/page.tsx
      studio/page.tsx
      studio/memories/new/page.tsx
      studio/memories/[id]/page.tsx
      studio/exhibits/[id]/page.tsx
      studio/letters/[id]/page.tsx
    globals.css
    layout.tsx
  components/
    layout/
    navigation/
    memories/
    museum/
    letters/
    studio/
    feedback/
    ui/
  lib/
    supabase/
      client.ts
      server.ts
      middleware.ts
    auth/
    data/
    validation/
    dates/
    media/
  types/
  styles/
public/
  illustrations/
  icons/
supabase/
  migrations/
  seed.example.sql
```

This is a target structure. Folder names can shift to match current Next.js conventions, but keep route code, shared components, data access, validation, and design tokens separated.

## 5. Data model

Use UUID primary keys, `created_at`/`updated_at` timestamps, foreign keys, constraints, and indexes deliberately. Every private content table must include a `space_id` so policies can scope data to the appropriate relationship space.

### `spaces`

| Column | Type | Notes |
|---|---|---|
| `id` | uuid PK | One private world per couple for V1. |
| `name` | text | Display name; avoid exposing it publicly. |
| `time_zone` | text | IANA timezone; initialize to `Asia/Dhaka` and make configurable. |
| `created_by` | uuid FK to auth user | Creator/owner. |
| `created_at` | timestamptz | Audit timestamp. |

### `space_members`

| Column | Type | Notes |
|---|---|---|
| `id` | uuid PK | |
| `space_id` | uuid FK | The private space. |
| `user_id` | uuid FK to auth user | Member. |
| `role` | enum/text constrained to `owner` or `partner` | Role used in authorization. |
| `created_at` | timestamptz | |

Constraint: unique `(space_id, user_id)`. Do not allow a user to promote themselves by editing a client-provided role.

### `memories`

| Column | Type | Notes |
|---|---|---|
| `id` | uuid PK | |
| `space_id` | uuid FK | Required for RLS scope. |
| `title` | text | Required; length validated. |
| `story` | text | Optional text/Markdown subset; sanitize/render safely. |
| `memory_date` | date | The date the memory happened; chronology uses this field. |
| `memory_time` | time nullable | Optional; do not invent time when unknown. |
| `date_precision` | enum/text | For example `exact`, `month`, `year`, or `unknown`; only store precision the owner actually knows. |
| `cover_path` | text nullable | Storage object path, not a permanent public URL. |
| `status` | enum/text constrained to `draft` or `published` | Partner can only read published content. |
| `display_order` | integer nullable | Optional manual ordering among records on the same date. |
| `created_by` | uuid FK | Audit. |
| `created_at`, `updated_at` | timestamptz | Audit; never substitute for `memory_date` in the timeline. |

Recommended index: `(space_id, status, memory_date, display_order)`.

### `exhibits`

Suggested columns: `id`, `space_id`, `slug`, `title`, `subtitle`, `body`, `cover_path`, `status`, `display_order`, `created_by`, `created_at`, `updated_at`.

Constraints: unique `(space_id, slug)`; indexed by `(space_id, status, display_order)`.

### `letters`

Suggested columns: `id`, `space_id`, `title`, `body`, `letter_type`, `visible_teaser`, `unlock_at` (nullable `timestamptz`), `cover_path` (nullable), `audio_path` (nullable), `status`, `created_by`, `created_at`, `updated_at`.

`letter_type` can include values such as `open_when`, `time_capsule`, and `personal_note`. Validate allowed values in the database and application. Store unlock dates with timezone-aware semantics. The page should show the release time in the configured display timezone.

### Content lifecycle

- Draft content is visible only to the owner.
- Published content is visible to authorized members according to the relevant policy.
- Future-locked letters are not readable by the partner until the release timestamp.
- Deletion should be confirmed in the UI. For first release, hard delete can be acceptable if media cleanup is reliably handled; otherwise use soft-delete and schedule cleanup for a later phase.

## 6. Authentication and authorization

### Authentication

- Configure Supabase Auth using the official server-side integration guidance for Next.js.
- Prefer a straightforward invited account flow, such as email magic link/OTP or securely provisioned accounts. Choose one flow and test email delivery and callback URLs before launch.
- Do not implement public sign-up.
- Use server-readable sessions/cookies using the supported SSR package/integration.
- Protect private layouts and routes. Redirect unauthenticated users to `/login` with a safe return path.
- Ensure callback handling validates redirects and cannot be used as an open redirect.

### Authorization model

Define reusable authorization checks such as `requireUser()`, `requireSpaceMembership()`, and `requireOwner()`. These improve code clarity but do not replace RLS. Use both application checks and database policies as defense in depth.

**Minimum RLS policy requirements:**

1. RLS is enabled on every private table.
2. A user can select rows only when they are a member of the row's `space_id` and the content is published, except the owner can view their own drafts.
3. Only the owner can insert/update/delete content in V1.
4. Users cannot read or change membership/role rows to grant themselves access or privilege.
5. Time-locked letters must have a database policy that denies partner reads while `unlock_at > now()`. The owner may access drafts/future letters to manage them.
6. Policies must be tested for both owner and partner, including direct API queries—not only through the rendered UI.

For complex membership checks, use carefully reviewed SQL helper functions with a safe `search_path`; avoid recursive RLS policy traps. Store migrations in source control and apply them predictably.

## 7. Time-capsule security

A front-end condition such as `if (now < unlockAt) showLock()` is only a presentation state and is insufficient by itself.

Required behavior:

- The list endpoint may return a safe lock card containing the letter ID, title, intended unlock time, and non-sensitive teaser.
- Before unlock, the partner must not receive the body text, audio path, cover path if sensitive, a signed URL, or any data that reveals the hidden content.
- RLS or a trusted server-side query path must deny the actual content row to the partner until the unlock timestamp has passed.
- Signed media URLs must only be minted after authorization and only for media the viewer is allowed to access.
- Compare timestamps using server/database time in UTC; render display time in the configured timezone.
- Test the exact boundary: just before unlock, at/after unlock, and different browser timezone settings.
- An owner may preview a locked letter in Studio, but owner-only preview data must never enter partner-facing routes or shared caches.

## 8. Private media

- Create a **private** Storage bucket, e.g. `relationship-media`; do not use a public bucket for personal photographs or recordings.
- Use predictable, non-sensitive object paths such as `<space_uuid>/<asset_uuid>.<ext>`. Do not put a person's name or letter title in object paths.
- Validate MIME type, extension, and reasonable file size server-side. Do not trust the browser's file name or claimed MIME type alone.
- Store object paths in the database, not permanent public URLs.
- Generate short-lived signed URLs or use authorized delivery after checking membership and content publication/unlock state.
- Set maximum file sizes appropriate for mobile uploads. Compress/resize photographs before or during upload where feasible; retain originals only if required.
- Clean up associated storage objects after confirmed content deletion, with error handling for partial failures.
- Provide alt text for meaningful photographs; decorative imagery should have empty alt text.

## 9. Validation and safe rendering

- Validate input on both client and server. Use schemas (for example, Zod) for form validation and strongly typed data boundaries.
- Enforce limits for titles, story bodies, tags, and upload sizes.
- Treat stored text as untrusted. Plain text is safest; if Markdown is supported, use a safe renderer and disallow raw HTML.
- Never build SQL queries with string concatenation.
- Keep secrets in environment variables; commit only an `.env.example` with placeholder values.
- Avoid logging private letter bodies, session tokens, signed URLs, or uploaded content.

## 10. Performance and quality requirements

- Use server-rendered content where practical and avoid shipping private content before authorization.
- Use optimized image components or equivalent responsive image handling.
- Set width/height or aspect-ratio containers to reduce layout shift.
- Lazy-load below-the-fold imagery and defer nonessential audio/media.
- Keep animation lightweight; honor `prefers-reduced-motion`.
- Avoid a large background video, 3D engine, heavy canvas effects, or multiple animation libraries.
- Provide loading skeletons only where they reflect real loading; never use fake loading as a romantic effect.
- Use pagination or incremental loading if the memory archive grows significantly.

## 11. Environment variables and secrets

Example names only—actual values belong in local environment configuration and the deployment provider:

```text
NEXT_PUBLIC_SUPABASE_URL=
NEXT_PUBLIC_SUPABASE_ANON_KEY=
SUPABASE_SERVICE_ROLE_KEY=   # server-only; omit if not needed
NEXT_PUBLIC_SITE_URL=
```

Never commit `.env.local` or real credentials. Do not expose the service-role key through a public prefix. Production callback URLs and allowed origins must be configured deliberately.

## 12. Testing strategy

### Automated checks

- Linting and formatting.
- TypeScript type check.
- Production build.
- Unit tests for date formatting, sorting, role checks, and unlock-state calculations.
- Component tests for accessible interactions and empty/error states where practical.
- End-to-end tests for sign-in, first reveal, core navigation, Studio write operations, and locked-letter behavior.

### Security acceptance tests

Use separate owner and partner test accounts, plus an unauthenticated session.

- Unauthenticated user cannot load a private route or fetch private data.
- Partner cannot create/update/delete content.
- Partner cannot read a draft.
- Partner cannot retrieve a locked letter's body/media before release, even by calling the data API directly.
- Partner can read the letter after release, assuming it is published.
- A member cannot query another space by substituting a different `space_id`.
- Private object URLs cannot be reused indefinitely after an intended short expiry.
- The owner can manage the content they own.

### Manual design QA

Test at minimum: 320 px phone, 360–390 px phone, tablet portrait, laptop, and wide desktop; keyboard-only navigation; reduced motion; slow connection; long titles; no image; many memories; empty account; upload failure; expired session.

## 13. Deployment and maintenance

1. Create development Supabase project and migrations.
2. Configure authentication URLs, allowed redirects, private bucket, and RLS.
3. Develop locally with `.env.local` excluded from Git.
4. Run automated checks and manual role/security tests.
5. Deploy to a preview environment and test with separate accounts.
6. Configure production environment variables and domain.
7. Re-run the full acceptance checklist after deployment.
8. Back up important content and document the restore process before relying on the site as a long-term archive.

Use a server-capable deployment target. A static export is not appropriate if required authorization or unlock enforcement depends on server code.

## 14. Official reference material

Verify the current instructions again when implementing, especially before upgrading packages or changing authentication code.

- Next.js App Router: https://nextjs.org/docs/app
- Next.js Server and Client Components: https://nextjs.org/docs/app/getting-started/server-and-client-components
- Next.js deployment: https://nextjs.org/docs/app/getting-started/deploying
- Supabase server-side authentication: https://supabase.com/docs/guides/auth/server-side
- Supabase Row Level Security: https://supabase.com/docs/guides/database/postgres/row-level-security
- Supabase Storage access control: https://supabase.com/docs/guides/storage/security/access-control
- Tailwind theme variables/design tokens: https://tailwindcss.com/docs/theme

## 15. Technical definition of done

A feature is not complete just because it renders.

- It follows the agreed design tokens and component patterns.
- It is responsive and keyboard-accessible.
- It has loading, empty, success, and error states appropriate to the action.
- It has server-side validation and authorization where data is changed or protected.
- It does not leak private content, credentials, or permanent media URLs.
- It passes relevant lint, type, build, automated, and manual checks.
- Its implementation and any schema changes are documented in `brain.md` and source control.
