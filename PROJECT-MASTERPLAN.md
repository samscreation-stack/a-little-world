# A Little World — Project Masterplan

**Document type:** Project source of truth  
**Status:** Phase 0 approved — implementation not started  
**Last updated:** 16 October 2026  
**Owner:** Project creator

---

## 1. Project Overview

### 1.1 Vision

A Little World is a private, personal website created as a gift for a couple. It should feel like entering a small world prepared with care: a place to revisit shared memories, discover personal observations, read meaningful letters, and preserve hopes for the future.

The experience should feel intimate, mature, editorial, and quietly magical. Real memories and personal writing must remain the focus.

### 1.2 Product Areas

1. **Our Memories** — a chronological collection of photographs, dates, stories, and meaningful moments.
2. **The Museum of Her** — a curated collection of exhibits about the small details, qualities, and moments the creator notices and appreciates.
3. **Letters for Later** — personal letters, “open when…” notes, voice messages, and future-dated time capsules.
4. **Studio** — a private owner-only area for creating, editing, previewing, publishing, ordering, and deleting content.
5. **Home Hub** — the central entry point connecting the three recipient-facing experiences.

These are parts of one application, one visual system, and one private space—not separate websites.

### 1.3 Product Principles

- Real details matter more than decorative effects.
- Privacy is enforced technically, not merely suggested by the interface.
- The experience is designed for mobile first.
- Content can be managed without editing application source code.
- The interface remains accessible, responsive, and understandable without animation.
- No invented memories, fabricated relationship facts, manipulative interactions, fake loading, or forced waiting.
- Features are added incrementally and tested before moving forward.

---

## 2. Approved Project Decisions

The following decisions are approved and must not be changed silently.

| Area | Decision |
|---|---|
| Application name | A Little World |
| Editor and environment | VS Code on Windows |
| Learning approach | Manual, step-by-step development with explanations |
| Existing experience | Some HTML, CSS, and JavaScript |
| Authentication | Owner-provisioned email/password accounts |
| Public registration | Not available |
| Roles | Owner and partner |
| Owner permissions | Create, edit, preview, publish/unpublish, reorder where applicable, and delete content |
| Partner permissions | Read published content and access eligible letters |
| Initial implementation milestone | Secure sign-in, home hub, and complete memory management |
| Memory order | Newest first by memory date |
| Memory chronology | Based on the date the memory happened, never upload time |
| Source control | Learn and use Git manually from the beginning |
| Coding style | Manual file creation and editing; minimal essential terminal commands |
| Scope control | Build in phases and do not skip agreed checkpoints |

### Important interpretation

The product requirements include the Museum of Her and Letters for Later in the intended product. The approved first milestone changes the **implementation sequence**, not the overall vision.

The UI/UX reference suggests oldest-first as its initial timeline preference. The approved project decision overrides that suggestion: newest-first is the default.

### Authentication clarification

Accounts must be provisioned by the owner through a trusted administrative process. There must be no public registration form or open account-creation flow.

The application must still enforce permissions on protected routes, database operations, and media access. A hidden button or an unlisted URL is not a security control.

---

## 3. Reference Documents

Keep the original reference documents intact:

- `PRD.md` — product requirements, user journeys, scope, priorities, and acceptance criteria.
- `TRD.md` — technical architecture, security requirements, data model, testing, and deployment.
- `UI-UX-Design-Brief.md` — visual direction, interface behavior, accessibility, and responsive design.

The filenames above describe the intended reference names. Preserve the actual uploaded filenames if they differ.

### Document precedence

1. Explicit decisions recorded in this masterplan.
2. Approved changes recorded in this masterplan's Decision Log.
3. Product scope and acceptance criteria from the PRD.
4. Technical constraints and security requirements from the TRD.
5. Visual direction and interaction guidance from the UI/UX Design Brief.

If two documents conflict, record the conflict and its resolution instead of quietly changing a requirement.

---

## 4. Technology Baseline

The planned baseline is:

| Layer | Planned technology |
|---|---|
| Framework | Next.js App Router |
| UI | React |
| Language | TypeScript |
| Styling | Tailwind CSS plus a small amount of custom CSS |
| Authentication | Supabase Auth |
| Database | Supabase PostgreSQL with Row Level Security (RLS) |
| Media | Supabase Storage using a private bucket |
| Source control | Git |
| Package management | npm with the lockfile committed |
| Deployment | Vercel or another suitable server-capable deployment platform |

Use current stable, compatible versions when implementation begins. Verify the official installation instructions before installing dependencies.

This is an approved planning baseline, not a statement that the tools have been installed or configured.

### Architecture rules

- Prefer server-side data loading and authorization for protected content.
- Use client-side components only where interaction requires them.
- Validate inputs and authorize writes on the server.
- Use database policies to restrict access to private records.
- Keep private media in authenticated storage.
- Never expose the Supabase service-role key in browser code or public environment variables.
- Do not use static-only deployment if it prevents the required server-side security checks.
- Avoid unnecessary libraries, separate API servers, microservices, and complex state-management systems.

---

## 5. Users and Permissions

### Owner

The owner prepares and maintains the website.

The owner must be able to:

- Sign in securely.
- Access the Studio.
- Create and edit memories.
- Upload and manage memory media.
- Set a memory's actual date.
- Preview content before publishing.
- Publish and unpublish content.
- Create and manage museum exhibits in an explicit order.
- Create and manage letters, including future release dates.
- Delete content with a clear confirmation step.

### Partner

The partner explores the prepared experience.

The partner must be able to:

- Sign in using an owner-provisioned account.
- View the home hub.
- Browse published memories and their details.
- Browse published museum exhibits when that area is implemented.
- View published letters that are eligible to open.
- See an honest release date for a locked letter without receiving its hidden content.

The partner must not be able to manage owner content, view drafts, or access another private space.

### Unauthenticated visitor

An unauthenticated visitor must not be able to access private content or media.

The sign-in screen must not reveal private names, photographs, letter content, or other relationship-specific details.

---

## 6. Visual Direction

### 6.1 Creative direction

**Paper, Starlight & Keepsakes**

The website should combine warm archival paper, restrained rose accents, refined typography, generous spacing, subtle gallery details, and small constellation-inspired illustrations.

It should feel personal and premium without looking childish, overly pink, generic, or excessively animated.

### 6.2 Baseline color tokens

| Token | Value | Intended use |
|---|---|---|
| `paper` | `#FBF7F1` | Main background |
| `paper-deep` | `#F1E7DE` | Secondary surfaces |
| `surface` | `#FFFDFA` | Reading panels and cards |
| `ink` | `#302630` | Primary text |
| `muted-ink` | `#6D6069` | Supporting text |
| `rose` | `#B75F79` | Accent |
| `rose-deep` | `#81384F` | High-contrast actions |
| `rose-wash` | `#F3E1E5` | Subtle highlights |
| `plum` | `#493544` | Occasional deeper detail |
| `brass` | `#A88752` | Small decorative accents |
| `line` | `#E3D8D1` | Dividers and borders |
| `focus` | `#245E83` | Keyboard focus |

These are baseline design tokens. Contrast must be tested rather than assumed.

### 6.3 Typography

- Display headings: Cormorant Garamond or a comparable refined serif.
- Body text and interface controls: DM Sans or a similarly readable sans serif.
- Handwritten accent: Kalam or a comparable readable handwriting font, used sparingly.

Primary reading text should generally be at least 16 px on mobile. Typography, spacing, borders, and radii must follow shared rules rather than being invented separately for every page.

### 6.4 Avoid

- Full-page pink gradients.
- Repeated floating hearts or emoji decoration.
- Glitter, cursor trails, continuous particles, and autoplay audio.
- Generic stock couple photography replacing real memories.
- Excessive glassmorphism, neon effects, and hazy backgrounds.
- Fake progress indicators, fake loading, forced scrolling, or interactions that delay access.
- Too many competing card styles.
- Decoration that makes text harder to read.

---

## 7. Application Structure and Information Architecture

The intended routes are:

- `/login` — sign-in.
- `/auth/callback` — authentication callback if required by the chosen flow.
- `/` — private home hub.
- `/memories` — memory archive.
- `/memories/[id]` — individual memory.
- `/museum` — exhibit collection.
- `/museum/[slug]` — individual exhibit.
- `/letters` — letter collection.
- `/letters/[id]` — eligible letter detail.
- `/studio` — owner-only content management.
- `/studio/memories/...` — memory management.
- `/studio/exhibits/...` — exhibit management.
- `/studio/letters/...` — letter management.
- `/future` — optional future area; not a first-milestone requirement.

Routes may be consolidated when implementation begins, provided the information architecture and access restrictions remain clear.

### Home hub

The hub should provide three clear destinations: Our Memories, The Museum of Her, and Letters for Later. During the first implementation milestone, the Museum and Letters destinations may be clearly marked as planned or unavailable rather than presenting unfinished features as functional.

### Studio

Studio is an owner-only management area. Its controls must never be exposed as usable partner controls.

---

## 8. Phased Development Roadmap

A phase is complete only when its acceptance criteria have been checked. Progress is not measured by the amount of code written.

### Phase 0 — Planning and project control

**Status: Approved; documentation is the current task.**

Deliverables:

- Review and preserve the three reference documents.
- Finalize `PROJECT-MASTERPLAN.md`.
- Create `brain.md` for operational tracking.
- Confirm the approved decisions and phased roadmap.

Acceptance criteria:

- Both project-control files exist in the project root.
- The implementation sequence is clear.
- No application implementation has begun.
- Unresolved decisions remain explicitly recorded.

### Phase 1 — Environment and Git foundations

Deliverables:

- Confirm the project folder and VS Code workspace.
- Check the installed development tools.
- Learn the essential Git concepts.
- Initialize Git manually and make an initial planning checkpoint.
- Establish a `.gitignore` strategy before adding credentials or local configuration.

Acceptance criteria:

- The user understands the role of the terminal, project root, repository, commit, and working tree.
- Git status and history can be inspected.
- Private credentials are not committed.
- The setup is documented in `brain.md`.

### Phase 2 — Application foundation and secure sign-in

Deliverables:

- Create the Next.js application.
- Establish the initial route and layout structure.
- Configure environment variables.
- Connect Supabase Auth.
- Build the sign-in experience.
- Implement owner-provisioned email/password authentication.
- Protect private routes and handle sessions.

Acceptance criteria:

- Valid provisioned accounts can sign in.
- Invalid credentials produce a useful error.
- Public registration is absent.
- Unauthenticated visitors cannot access private routes or private data.
- Secrets remain outside source control.
- Relevant checks pass.

### Phase 3 — Shared design foundation and home hub

Deliverables:

- Implement shared color, typography, spacing, and focus tokens.
- Create reusable layout and navigation patterns.
- Build the private home hub.
- Add responsive destination areas for the three product experiences.
- Design intentional empty, loading, and error states.

Acceptance criteria:

- The home hub works at phone, tablet, and desktop widths.
- The visual system follows the approved design direction.
- The three destinations are easy to understand.
- Incomplete destinations are not presented as finished features.
- Keyboard navigation and visible focus work.

### Phase 4 — Complete memory management

This is the first complete content-management milestone.

Deliverables:

- Define and implement the memory data model.
- Create secure database access policies.
- Configure private media storage and access policies.
- Build the memory archive and memory detail page.
- Build owner-only memory creation and editing.
- Support title, memory date, caption/story, publication status, and photographs.
- Support multiple images where appropriate.
- Add preview, publish/unpublish, and confirmed deletion.
- Implement newest-first default sorting using the memory date.
- Preserve a clear return path from memory detail to the archive.

Acceptance criteria:

- The owner can create, view, edit, preview, publish, unpublish, and delete memories.
- The partner can only view published memories.
- An unauthenticated visitor cannot retrieve private records or media.
- Sorting uses the actual memory date, not upload time.
- Missing dates are handled honestly; they are not fabricated.
- Upload errors are recoverable and do not silently destroy text.
- The archive works at 320 px width without horizontal overflow.
- Relevant automated checks and manual permission tests pass.

### Phase 5 — The Museum of Her

Deliverables:

- Implement exhibit storage and owner-defined display order.
- Build the museum overview and exhibit details.
- Add Studio creation, editing, preview, publishing, ordering, and deletion.
- Connect related memories only where useful and supported by the data model.

Acceptance criteria:

- The partner sees published exhibits in the intended order.
- Drafts and management actions remain private.
- Long exhibit titles and text remain readable on mobile.
- The museum uses the shared visual system.

### Phase 6 — Letters for Later

Deliverables:

- Implement letter records and owner-only management.
- Support “open when…” and future-dated letter types.
- Implement publication and release-time rules.
- Build the letter index and reading experience.
- Add optional audio support only when the secure text-and-letter flow is stable.

Acceptance criteria:

- The owner can manage letters and their release dates.
- Published, eligible letters can be opened by the partner.
- Locked letters reveal only approved metadata, such as title and opening date.
- Locked bodies and private media URLs are not sent to the partner before release.
- Server/database rules enforce release timing; a visual lock alone is insufficient.
- Boundary times and time zones are tested.

### Phase 7 — Emotional polish and content completion

Deliverables:

- Add the first-visit reveal.
- Refine microcopy using the creator's authentic voice.
- Add subtle motion only where it improves the experience.
- Respect reduced-motion settings.
- Prepare real, truthful memories, exhibits, and letters.
- Consider the optional Future Room as a small curated experience.

Acceptance criteria:

- The reveal is skippable and does not block access.
- No fake content or invented relationship history is present.
- Audio never autoplays.
- The interface remains understandable with motion disabled.
- The experience is cohesive with real content and with empty states.

### Phase 8 — Quality assurance and security review

Deliverables:

- Run lint, type checking, production build, and available automated tests.
- Test owner, partner, and unauthenticated sessions independently.
- Test direct data requests and protected media access.
- Review accessibility, responsive behavior, and error recovery.
- Test long titles, missing images, empty data, slow connections, and expired sessions.

Acceptance criteria:

- No unresolved critical security or privacy issue.
- No unauthorized access to private records, drafts, or media.
- No access to locked letter bodies before release.
- No horizontal overflow at 320 px.
- Keyboard, focus, contrast, and reduced-motion checks have been performed.
- The build and required checks pass.

### Phase 9 — Deployment and maintenance

Deliverables:

- Configure a server-capable deployment target.
- Set production environment variables and authentication URLs.
- Verify the production database and storage policies.
- Test production sign-in and permissions.
- Document backup and recovery considerations.

Acceptance criteria:

- Production behavior matches the tested application.
- No credentials are committed or exposed to the browser.
- Private content remains protected after deployment.
- Recovery and maintenance steps are documented.

### Phase 10 — Post-launch improvements

Possible improvements:

- Search and filters.
- Voice notes and text alternatives.
- Partner contributions.
- Optional reminders.
- Additional Future Room features.

These features are not prerequisites for the first milestone. Add them only after the core product is stable and their requirements are clear.

---

## 9. Required Development Workflow

Every meaningful task follows this cycle:

1. **Understand:** explain the purpose and expected outcome.
2. **Locate:** name the exact file or folder and its path.
3. **Act:** state whether to create, replace, or edit and where.
4. **Explain:** provide the code or configuration and explain its role.
5. **Inspect:** review the changed content before running it.
6. **Test:** give exact test steps and expected results.
7. **Record:** update `brain.md` with changes, results, and the next action.
8. **Checkpoint:** use Git deliberately, with a clear commit message when the change is ready.

Do not skip ahead when a test fails. Diagnose the failure, fix the current step, rerun the test, and then continue.

### Terminal command policy

- Use commands only when they are necessary.
- Explain what each command does before asking the user to run it.
- State the directory from which a command should be run.
- Avoid unnecessary bulk commands and unexplained command sequences.
- Prefer a small number of verifiable actions over a large setup script.

### Git learning policy

Git is part of the learning process, not an invisible background tool.

Explain the working tree, staging, commits, branches when needed, and how to inspect history. Do not assume a change was committed merely because files were edited. Never commit secrets or real environment credentials.

---

## 10. Definition of Done

A task or feature is complete only when:

- It satisfies the agreed requirements.
- The changed files and their purpose are understood.
- The relevant test has been run and its result recorded.
- Error and empty states have been considered.
- Responsive and accessibility requirements have been checked where relevant.
- Protected data and operations have appropriate authorization.
- `brain.md` reflects the actual state.
- Git status and checkpoint decisions have been reviewed.

A feature that merely renders on screen is not necessarily complete.

---

## 11. Risks and Mitigations

| Risk | Mitigation |
|---|---|
| Scope grows before the core works | Follow the phased roadmap and defer optional features. |
| Code is copied without understanding | Explain each meaningful change and test it immediately. |
| Private content leaks | Test route authorization, RLS, storage policies, and direct data access. |
| Future letters leak before release | Enforce release rules server-side and test API access. |
| Memories appear in the wrong order | Sort by the assigned memory date, newest first. |
| Mobile layouts break | Test small widths early, including 320 px. |
| Git history contains secrets | Set up ignore rules and inspect changes before staging. |
| Visual design becomes cliché-heavy | Follow the approved design brief and prioritize authentic content. |
| Debugging derails the roadmap | Return to the current task and resolve its failure before moving forward. |
| Project state becomes unclear | Keep `brain.md` updated after every meaningful session. |

---

## 12. Decision Log

| ID | Decision | Status |
|---|---|---|
| D-01 | Owner-provisioned email/password authentication; no public registration | Approved |
| D-02 | Newest-first default memory order, based on memory date | Approved |
| D-03 | First implementation milestone is secure sign-in, home hub, and complete memory management | Approved |
| D-04 | Git is learned and used manually from the beginning | Approved |
| D-05 | Next.js App Router, React, TypeScript, Tailwind CSS, and Supabase are the technical baseline | Planned baseline |
| D-06 | Private storage, server-side authorization, and RLS are required security controls | Required |
| D-07 | The Museum and Letters remain in product scope but follow the first milestone | Approved |
| D-08 | Development proceeds through small, explained, tested steps | Approved |

New decisions must include the date, reason, affected scope, and whether they supersede an existing decision.

---

## 13. Current State and Next Action

**Current phase:** Phase 0 — planning and project control.  
**Implementation status:** Not started.  
**Current task:** Create and save this masterplan and `brain.md` manually in VS Code.  
**Next phase after documentation is checked:** Phase 1 — environment and Git foundations.

Do not begin application implementation until the two project-control files are saved and the planning baseline is confirmed.

