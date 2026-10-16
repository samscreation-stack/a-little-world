# UI/UX Design Brief

**Product:** A Little World  
**Document status:** Visual direction v1.0  
**Last updated:** 16 October 2026

---

## 1. Design challenge

Design a private romantic website that feels handcrafted, mature, personal, and quietly magical. It should feel closer to an intimate editorial journal or a tiny private museum than a generic “love website.” The emotional effect should come from the balance of composition, typography, real content, and thoughtful pacing—not a pile of hearts, gradients, or animations.

## 2. Creative direction

### Direction name: Paper, Starlight & Keepsakes

The visual world mixes warm archival paper, a restrained rose accent, gallery-like spacing, small star/constellation details, and the tactility of notes and photographs. It is warm, but not excessively pink. It is whimsical, but not childish. It is premium, but not cold.

### Keywords

Editorial, intimate, tactile, restrained, luminous, warm, personal, precise, unhurried.

### Anti-references

Do not use:

- A full-page pink gradient as the main visual identity.
- Repeating floating hearts or emoji as decoration.
- Glitter, confetti, cursor trails, continuous particle fields, or autoplay music.
- Five different card styles competing on one screen.
- Oversized script fonts for paragraphs or navigation.
- Generic stock couples as substitutes for real photographs.
- Excessive glassmorphism, blurred panels, neon glows, or hazy dark backgrounds.
- Forced scrolling, fake progress bars, fake loading, or interactions that delay access to content.
- Huge decorative borders around every component.

## 3. Visual system

The exact values below are the baseline tokens. They should be defined centrally and adjusted only after contrast and real-screen review.

### Color tokens

| Token | Hex | Usage |
|---|---|---|
| `paper` | `#FBF7F1` | Main page background; warm, not pure white. |
| `paper-deep` | `#F1E7DE` | Soft section surfaces and paper details. |
| `surface` | `#FFFDFA` | Cards, modal surfaces, reading panels. |
| `ink` | `#302630` | Main headings and high-priority text. |
| `muted-ink` | `#6D6069` | Supporting text and metadata. |
| `rose` | `#B75F79` | Primary accent, links, illustration details. |
| `rose-deep` | `#81384F` | High-contrast primary action and important interactive states. |
| `rose-wash` | `#F3E1E5` | Selected states, subtle highlights, quiet chips. |
| `plum` | `#493544` | Occasional deep section detail, not a constant dark theme. |
| `brass` | `#A88752` | Tiny star/line details and occasional metadata emphasis. |
| `line` | `#E3D8D1` | Dividers, outlines, subtle boundaries. |
| `focus` | `#245E83` | Clear keyboard focus ring. |

Do not rely on color alone to show state. All interactive combinations must be tested for contrast; color tokens are a starting palette, not a substitute for accessibility testing.

### Typography

- **Display/headlines:** Cormorant Garamond or a similarly refined serif. Use for large, short lines and meaningful section titles only.
- **Body/UI:** DM Sans or a similarly legible sans serif. Use for paragraphs, controls, metadata, and forms.
- **Handwritten accent:** Kalam or another readable handwriting face, restricted to tiny annotations or a short signature. Never use it for long paragraphs or functional labels.

Typography rules:

- Make type scale and line-height explicit; avoid random per-screen font sizes.
- Body copy should be comfortable to read on a phone (generally 16 px or larger for primary prose).
- Keep paragraph line length around 55–75 characters on larger screens.
- Use sentence case. Avoid all-caps labels everywhere; reserve small uppercase text for occasional exhibit/category metadata.
- Self-host or use the framework's optimized font loading when practical; include fallback fonts so the page remains readable if fonts fail.

### Shape, surfaces, and depth

- Use a small, consistent radius scale: approximately 10 px for controls and 16–22 px for larger content surfaces.
- Cards may have a faint border and almost imperceptible shadow; do not make every card float.
- Use 1 px separators, whitespace, and alignment before adding boxes.
- Polaroid-like treatment belongs to memory media, not every UI card.
- Use paper texture sparingly—prefer a subtle CSS texture or small bespoke SVG over a large image texture that reduces legibility.

### Spacing and layout

Use a consistent spacing scale (for example 4, 8, 12, 16, 24, 32, 48, 64, 88 px). The visual rhythm should come from repeated rules rather than ad hoc margin values.

- Main content max-width: approximately 1120–1200 px on desktop.
- Comfortable mobile gutters: 20–24 px where possible, with 16 px as a minimum where space is tight.
- Desktop page headers should align to the same content grid as their section content.
- Do not stretch reading text to the full desktop width.
- Use responsive layouts intentionally; do not shrink desktop cards until they barely fit.

## 4. Global shell and navigation

### Desktop

- Slim top navigation with a quiet wordmark/monogram at left and clear destination links.
- Current section indicated by a subtle underline or color change, not a large pill.
- Account/Studio access should be discreet and visible only to the authorized owner where appropriate.
- Main content sits in a centered grid with generous vertical space.

### Mobile

- Use a compact header with a clear back path.
- A bottom navigation bar is optional; do not add it unless it clearly improves usability. If used, limit it to the three core destinations and provide proper safe-area spacing.
- Do not hide important navigation in a hamburger if the information architecture can fit cleanly.
- Keep all controls clear of the home-indicator/safe-area region.
- Verify no horizontal overflow from long dates, exhibit titles, images, or decorative SVGs.

### Navigation labels

Prefer simple labels: **Home**, **Our Memories**, **The Museum**, **Letters for Later**. Use evocative language in the page copy, but do not make core navigation ambiguous.

## 5. Key screens and visual behavior

### A. Sign-in screen

Purpose: allow invited access without spoiling the surprise or disclosing personal content.

- A warm neutral background, one centered, compact sign-in panel, clear explanation, one primary action, and helpful error state.
- No relationship-specific names or images before authentication.
- No registration link.
- Avoid security theater such as a fake “secret portal” or arbitrary password riddle.

### B. First-visit reveal

Purpose: establish a personal emotional moment and then get out of the way.

- Begin with calm negative space and a small custom constellation/illustration.
- Show one personal sentence supplied by the creator. Keep the line short enough to read at a glance.
- One prominent action: “Enter our little world” (editable copy).
- Animation is subtle (fade/position transition, approximately 200–500 ms), skippable, and disabled or reduced under `prefers-reduced-motion`.
- The reveal must not require audio, scrolling, waiting, or completing a game.
- Returning users go directly to the home hub unless they choose to replay it.

### C. Home hub

Purpose: give an immediate sense of the world and three obvious ways to explore it.

- Hero: display serif title, one concise personal introduction, small constellation artwork, and optionally one chosen photograph.
- Under the hero: three strongly differentiated but visually related destinations: Our Memories, The Museum of Her, Letters for Later.
- One small “recently added” detail may appear after content exists; do not turn the home page into a dashboard full of widgets.
- A date/anniversary count is optional and should not dominate the experience.
- Avoid a giant CTA that consumes half the screen on mobile.

### D. Our Memories archive

Purpose: browsing through time should feel effortless and personal.

- Start with a simple page intro and chronology.
- The visual treatment can alternate between editorial photo sizes while staying aligned to one grid. Variation must be rule-based, not random.
- Date labels must remain readable. Use the date that the owner assigned to the memory, not its upload time.
- Begin with oldest-first storytelling as the default; offer a clear newest-first toggle if useful.
- If the memory has no reliable date, group it under “Undated” instead of assigning a fabricated place in the timeline.
- Filter chips are optional and should only appear when there is enough content to justify them.
- Clicking a memory should open a full detail page or accessible lightbox with a clear close/back action and keyboard support.

### E. Memory detail

- Large image with a deliberate aspect ratio; avoid forced cropping of meaningful faces or objects.
- Title and date are visually separate from the story.
- Reading area has a comfortable line length.
- Additional photos are easy to browse and have useful alt text.
- Include a visible back-to-memories path and preserve the user's previous archive position.

### F. The Museum of Her

Purpose: a curated collection, not a standard product grid.

- Treat each exhibit like a gallery label: category/number, title, brief description, and one optional image or illustrated motif.
- Use explicit editorial ordering.
- Keep the overview spare—approximately 3–6 exhibits visible without overwhelming the screen.
- Exhibit detail pages may use a more spacious layout and one quiet accent color.
- The visual language can include tiny gallery-label typography and thin dividing lines; avoid fake museum UI that is hard to read.

### G. Letters for Later

Purpose: make emotional support and time-capsule letters easy to find without turning them into a game.

- Group by “Open when…” and “For a date in the future” if both types exist.
- Available letters use clear interactive cards/envelopes; locked letters show title/date and an honest “Opens on…” label.
- Locked letters should feel inviting, not punitive. No countdown is necessary; if used, it must be accurate and accessible.
- The body of locked content must not be sent to the browser before release.
- Reading screen: simple paper-like surface, readable prose, generous line height, and optional audio controls with transcripts/text alternatives.

### H. Future Room

For V1, treat this as a small curated page or exhibit, not a task manager. It may contain hopes, places to visit, things to learn together, or traditions to build. Avoid turning affection into a checklist or making promises on behalf of the partner.

### I. Owner Studio

Purpose: functional and reliable, visually consistent but not as decorative as the recipient experience.

- Use a clear admin layout with a left rail on wide screens and an accessible compact mobile navigation.
- Group content types: Memories, Exhibits, Letters.
- Forms have explicit labels, visible validation, save/publish state, and unsaved-change protection where practical.
- Media upload shows progress and recoverable errors. Preview includes mobile width.
- Delete requires a confirmation dialog naming the item and explaining the effect.
- Studio controls never appear to the partner.

## 6. Component inventory

Create and reuse components instead of restyling every page independently.

- `SiteHeader` / `PrivateNavigation`
- `PageIntro`
- `ConstellationIllustration` (small, custom SVG; accessible decorative art)
- `DestinationCard`
- `MemoryTimeline` / `MemoryCard`
- `MemoryMediaGallery`
- `PhotoLightbox`
- `ExhibitCard` / `ExhibitLabel`
- `LetterCard` / `LockedLetterCard`
- `AudioNotePlayer`
- `StudioSidebar` / `StudioMobileNav`
- `FormField` / `DateTimeField` / `MediaUploader`
- `EmptyState` / `LoadingSkeleton` / `ErrorState`
- `Toast` / `ConfirmDialog`

The inventory is a guide, not a command to create a component for every wrapper. Components should exist when they represent a reusable behavior or stable design pattern.

## 7. Interaction and motion rules

- Interactive controls have hover, focus-visible, active, disabled, success, and error states where applicable.
- Use the same transition curve and duration scale across the app.
- Prefer a subtle fade/translate and small state changes; avoid bounce and elastic animation for core navigation.
- Decorative stars can have a rare, slow twinkle, but must not impair reading or performance. Static artwork is acceptable and often preferable.
- Never autoplay music or voice.
- Avoid hover-only interactions; all functionality must work by touch and keyboard.
- Respect `prefers-reduced-motion`; content must remain understandable with all animation disabled.
- Destructive actions require confirmation and clear wording.
- Focus moves appropriately when modals open/close; Escape closes dismissible dialogs when safe.

## 8. Content and microcopy principles

- Write in the creator’s natural voice. Edit for clarity, but do not fabricate shared history or feelings.
- Prefer concrete details to generic phrases such as “you are my everything.”
- Keep navigation direct and page introductions short.
- Buttons describe the action, not a vague emotion.
- Error messages explain what happened and what to do next.
- Do not use guilt-based prompts, relationship tests, fake secrets, or coercive “unlock” mechanics.

Examples of UI copy (editable):

- Welcome: “I made a little place for our moments.”
- Primary action: “Enter our little world”
- Memory empty state: “Our wall is waiting for its first memory.”
- Letter lock state: “This letter opens on 14 February 2027.”
- Upload failure: “That file didn’t finish uploading. Try again; your text is still here.”

Do not ship those example lines blindly if they do not sound like the creator.

## 9. Accessibility and responsive acceptance

- Target WCAG 2.2 AA as the standard for the core experience.
- Use semantic headings in a logical order and semantic buttons/links.
- Every form field has a programmatic label and clear error association.
- Images have appropriate alt text; decorative SVGs are hidden from assistive technology.
- All actions work without a mouse.
- Focus indicators are visible and not hidden by sticky navigation.
- Modal/dialog behavior manages focus and returns focus after close.
- Contrast is tested for text, controls, and focus states; never use muted color so lightly that metadata becomes unreadable.
- Layout works at 320 px width and at browser zoom; no horizontal overflow.
- Check at 200% zoom, reduced motion, and with a keyboard-only pass.
- Audio content should have a text alternative; controls must be labeled.

## 10. Design QA checklist

Before accepting a screen, ask:

1. Is the primary action obvious without competing CTAs?
2. Does the content feel personal, or is decoration trying to do all the work?
3. Are type, spacing, borders, radii, and shadows drawn from shared tokens?
4. Does it work at 320–390 px widths without overflow?
5. Is every control usable by touch and keyboard?
6. Are empty, loading, error, and disabled states designed rather than improvised?
7. Is the screen still beautiful when there are no photos or very long text?
8. Does the interface avoid revealing private content before authorization?
9. Does motion respect reduced-motion preferences?
10. Can the page be understood without animation, color perception, or hover?

## 11. Visual acceptance statement

The finished interface should feel cohesive even when the content changes. A new memory, exhibit, or letter should inherit the same typographic hierarchy, spacing, controls, and interaction behavior automatically. Do not solve each new screen by adding a new color, card shape, animation, or unrelated visual metaphor.
