# Product Requirements Document (PRD)

**Product:** A Little World  
**Document status:** Draft v1.0 — planning baseline  
**Audience:** Product, design, and implementation  
**Last updated:** 16 October 2026

---

## 1. Product summary

**A Little World** is a private, personal website made as a gift for one couple. Its first visit should feel like discovering a small world prepared especially for the recipient. After that first reveal, it becomes a growing archive of shared memories, personal observations, meaningful letters, and hopes for the future.

The product combines three connected experiences:

1. **Our Memories** — a chronological collection of photos, dates, stories, and small moments.
2. **The Museum of Her** — curated “exhibits” about details the creator notices and loves about her.
3. **Letters for Later** — personal letters, voice messages, “open when…” notes, and future-dated time-capsule entries.

These are not separate microsites. They share one visual system, one navigation model, and one secure private space.

## 2. Product vision

Create a digital place that communicates, through specific memories and considered design, “I notice you, I remember us, and I made this for you.” It should feel intimate and editorial—not like a generic Valentine’s template or a public social network.

### Product promise

A personal world that can be opened as a surprise today and revisited as the relationship grows.

### Experience principles

- **Specific beats extravagant:** real details, moments, and writing matter more than effects.
- **Quiet visual confidence:** generous spacing, restrained color, deliberate typography, and subtle motion.
- **Content leads:** the interface helps memories and letters matter; decoration never competes with them.
- **Private by design:** no public profiles, discoverability, public feed, or open registration.
- **Usable on a phone first:** assume she opens the link on a mobile browser.
- **No emotional coercion:** avoid guilt-based copy, forced choices, fake loading, manipulative lock animations, or “prove you love me” mechanics.
- **Accessible by default:** visible focus states, readable contrast, keyboard support, reduced-motion support, and descriptive labels.

## 3. Users and roles

### Primary user — the recipient

She opens the website as a surprise and explores memories, exhibits, and letters. Her experience should require little explanation. She can browse and revisit content without being asked to build or manage the product.

### Secondary user — the creator/owner

The creator prepares and maintains the experience: writes content, uploads media, selects dates, arranges exhibit order, and prepares letters. Editing should be possible through a focused private “Studio” area rather than by modifying source code for every content update.

### Access model

- **Owner:** can create, edit, publish, reorder, and delete content.
- **Partner:** can view published content and open letters that are eligible to be opened.
- **No public registration:** accounts are invited/provisioned by the owner; the app does not expose an open sign-up form.
- **Future option, not MVP:** allow the partner to contribute memories. Do not build collaborative editing until the core gift is stable.

## 4. Goals and non-goals

### Goals

1. Deliver a memorable first reveal after successful sign-in.
2. Make three content areas discoverable without overwhelming the home screen.
3. Present memories in meaningful chronological order based on the memory date—not upload time.
4. Let the owner manage content without editing application code.
5. Support image and text content first, with voice recordings supported where practical.
6. Keep future-dated letters inaccessible to the partner until their release time.
7. Work cleanly on small mobile screens, tablets, and desktop.
8. Establish a stable foundation for adding content over months or years.

### Non-goals for version 1

- Public sharing, public user profiles, comments, likes, followers, or a social feed.
- AI-generated love letters or invented memories.
- Chat/messaging between the couple.
- Calendar sync, push notifications, email campaigns, or automatic anniversary messages.
- Complex 3D worlds, WebGL, heavy particle effects, cursor-following effects, or full-screen scroll-jacking.
- Native iOS/Android applications.
- A general-purpose website builder.
- End-to-end encrypted storage. The planned implementation uses authenticated access controls, not end-to-end encryption; this distinction must not be misrepresented.

## 5. Information architecture

### Public/auth boundary

- `/login` — minimal, discreet sign-in page. It must not expose names, photos, letters, or relationship-specific details before authentication.
- `/auth/callback` — handles the configured sign-in callback.

### Private experience

- `/` — the welcome/home hub. The first successful visit reveals the prepared experience; later visits return to the hub.
- `/memories` — chronological memory archive.
- `/memories/[id]` — memory detail view.
- `/museum` — exhibit collection.
- `/museum/[slug]` — an individual exhibit.
- `/letters` — “open when…” collection and time-capsule index.
- `/letters/[id]` — an eligible letter detail view.
- `/future` — optional V1.1 area for shared plans/dreams. For V1, this may be a curated “Future Room” exhibit rather than a complex feature.
- `/studio` — owner-only content management.
- `/studio/memories/new`, `/studio/memories/[id]`, `/studio/exhibits/[id]`, `/studio/letters/[id]` — content management routes.

Routes can be consolidated during implementation, but the information architecture and access restrictions must remain consistent.

## 6. Core user journeys

### Journey A — first reveal

1. Recipient opens the invitation URL.
2. If not signed in, she sees a calm sign-in screen that does not reveal private content.
3. After successful authentication, the application checks whether the first-reveal state has been completed.
4. If not, show a short welcome moment: a quiet visual, one personal line written by the creator, and one clear “Enter our little world” action.
5. Transition to the home hub. The reveal must be skippable and must not block the site if animation fails.

### Journey B — relive a memory

1. Open **Our Memories** from the home hub.
2. Browse a clear timeline/gallery ordered by the date the memory happened.
3. Filter by year or content type if there is enough content to justify filters.
4. Open a memory to view its photo, caption/story, date, and any additional media.
5. Return to the same position in the archive without losing the browsing context.

### Journey C — discover an exhibit

1. Open **The Museum of Her**.
2. View a small, intentionally ordered set of exhibits.
3. Open an exhibit to read its short title, descriptive text, and relevant media.
4. Navigate to the next/previous exhibit when available, or return to the museum.

### Journey D — open a letter

1. Open **Letters for Later**.
2. Choose a context such as “when you miss me” or “when today feels heavy,” or view a dated time-capsule entry.
3. If available, open the letter and read/listen to the content.
4. If it is time-locked, show only its title, intended opening date, and a restrained lock state. Do not send the hidden letter body or its media URL to the browser before it is eligible to open.

### Journey E — creator adds content

1. Owner enters **Studio**.
2. Selects a content type and enters text/date/metadata.
3. Adds media where needed.
4. Previews the content at a mobile width.
5. Saves as draft or publishes it.
6. The new item appears in the intended order based on its memory date or explicit exhibit order—not its creation timestamp.

## 7. Functional requirements

Priority definitions: **P0** = required for initial release, **P1** = useful next, **P2** = later enhancement.

| ID | Priority | Requirement | Acceptance criteria |
|---|---|---|---|
| FR-01 | P0 | Invite-only authentication | Unauthenticated users cannot access private routes or content. Public registration is not available. |
| FR-02 | P0 | Welcome/home hub | Presents the three principal destinations clearly and works at mobile widths without horizontal overflow. |
| FR-03 | P0 | Memory archive | Displays published memories in memory-date order; missing dates are not fabricated. |
| FR-04 | P0 | Memory detail | Shows title, date, writing, and media with a clear return path. |
| FR-05 | P0 | Museum exhibits | Displays curated exhibits in explicit owner-defined order. |
| FR-06 | P0 | Letters collection | Distinguishes available letters from future-locked letters without exposing locked content. |
| FR-07 | P0 | Time-capsule enforcement | Partner access to future letters is denied by server/database rules until `unlock_at` is reached. UI-only locks do not satisfy this requirement. |
| FR-08 | P0 | Owner Studio | Owner can create, edit, publish/unpublish, reorder where relevant, and delete content with confirmation. |
| FR-09 | P0 | Media storage | Uploaded media uses a private storage bucket and authenticated access. |
| FR-10 | P0 | Responsive/accessibility baseline | Keyboard focus is visible; interactive elements have accessible names; reduced motion is respected; touch targets are usable. |
| FR-11 | P0 | Empty/loading/error states | Each data-backed page has intentional loading, empty, and recoverable error states. |
| FR-12 | P1 | Voice notes | Owner can attach an audio recording; the recipient has standard play/pause controls and a transcript or text alternative where feasible. |
| FR-13 | P1 | Search and filters | Search/filter controls appear only when archive size makes them useful. |
| FR-14 | P1 | First-visit state | The welcome reveal is shown on the first authenticated visit and can be revisited from a discreet “Start here” entry. |
| FR-15 | P1 | Draft/preview | Owner can preview draft content without publishing it to the partner. |
| FR-16 | P2 | Partner contributions | Partner can submit or add their own memories under explicit product rules. |
| FR-17 | P2 | Anniversary/occasion reminders | Optional and disabled by default; requires a separate privacy and delivery decision. |

## 8. Content requirements

### Memory

Required: title, memory date, publication status. Optional: short caption/story, one or more photos, audio, location label, tags, and a private note for the owner. Upload time is audit metadata only and must not define the memory's position in the archive.

### Museum exhibit

Required: title, short introductory copy, explicit display order, publication status. Optional: subtitle, cover image, body text, and related memories.

Suggested initial exhibits:

- **The First Impression** — the creator’s memory of first noticing her.
- **The Smile Collection** — moments connected to her smile.
- **Things Nobody Notices** — small details the creator remembers.
- **The Masterpiece** — a heartfelt letter presented as a gallery piece.
- **The Future Room** — plans and hopes, written honestly without promising outcomes outside either person’s control.

These are prompts, not prewritten facts. The owner must supply truthful content.

### Letter

Required: title, body, type/context, publication status. Optional: unlock date/time, audio attachment, cover artwork, and a short visible teaser. A letter that is locked for the partner must never leak its body in page props, HTML, API payloads, static bundles, metadata, or media URLs.

## 9. Success criteria

This is a personal product, so traffic and engagement metrics are not the main measure of success.

### Release acceptance

- All P0 journeys work from a clean browser session.
- A user without membership cannot read private database rows or access private media.
- The partner cannot retrieve the contents of a future-dated letter before release time, including by querying the data API directly.
- The experience has no horizontal overflow at 320 px viewport width.
- The first meaningful content appears promptly on a typical mobile connection; large media is lazy-loaded/optimized where appropriate.
- No critical keyboard, focus, or contrast issue remains in the core journeys.
- `npm run lint`, `npm run typecheck`, and `npm run build` pass before release.
- A real test account is used to verify owner and partner permissions independently.

### Qualitative success

- The recipient understands where to begin without instructions.
- The site feels personal because of real content, not generic romantic copy.
- Navigation is calm and easy to remember.
- The design remains coherent after at least 20 memories/exhibits/letters have been added.

## 10. Risks and mitigations

| Risk | Mitigation |
|---|---|
| The design becomes a cliché-heavy love template | Use a restrained editorial direction; require real copy and meaningful media before adding decoration. |
| Beautiful UI hides weak privacy | Treat authentication, row-level security, private storage, and locked-letter tests as release blockers. |
| The project becomes too complex for the first release | Build in phases and defer collaborative features, reminders, and complex effects. |
| Content changes require code edits | Build a minimal owner-only Studio in the MVP. |
| Image-heavy pages are slow on mobile | Use optimized images, lazy loading, size limits, and reserved aspect ratios to prevent layout shift. |
| Time zones create a confusing release date | Store timestamps with timezone awareness, define the display timezone in settings, and test the release boundary. |
| The site accidentally reveals sensitive details in link previews | Use generic page metadata and do not include private titles/photos in unauthenticated metadata. |

## 11. Release phases

### Phase 0 — content and design foundation

Settle the visual system, app structure, key copy, sample content, user roles, and data model before adding decorative polish.

### Phase 1 — private core

Authentication, permissions, home hub, memories, museum, letters, Studio, private media, and responsive behavior.

### Phase 2 — emotional polish

First-visit reveal, subtle motion, audio notes, thoughtful transitions, and carefully chosen microcopy.

### Phase 3 — post-launch improvements

Search, tags, partner contributions, and other enhancements only when real use shows they are needed.

## 12. Decisions that must not be quietly changed

1. No public sign-up or public content feed.
2. Time-locked letters must be protected by server/database enforcement—not a front-end-only lock.
3. Memory chronology is based on the chosen memory date, not upload time.
4. The initial product is mobile-first and content-first.
5. No fake content, fabricated relationship facts, fake loading, or manipulative interactions.
6. The first release should be small enough to test thoroughly.
