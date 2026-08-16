# Home / Command Center — Screen Specification

**Status:** UI concept + layout specification — no code, no visual mockup
**Builds on:** UX strategy → Command Center architecture → Visual design system → Design tokens
**Viewport target:** laptop, ~1440px design width (works down to ~1280px)

---

## 1. Full Desktop Layout (RTL)

Three horizontal regions, right-to-left reading order, per the layout strategy: **Navigation (right) → Main Workspace (center) → AI companion (left, collapsed by default)**.

```
┌─────────────────────────────────────────────────────────────────────┬────────┐
│  MAIN WORKSPACE                                            (flows ➜) │  NAV   │
│  ┌───────────────────────────────────────────────────────────────┐  │  rail  │
│  │ A · Orientation Strip                                          │  │  88px  │
│  ├───────────────────────────────────────────────────────────────┤  │        │
│  │                                                                 │  │  ⌂     │
│  │ B · NOW                                     (dominant, full-w) │  │  Home  │
│  │                                                                 │  │        │
│  ├─────────────────────────────────────┬───────────────────────┤  │  □     │
│  │                                       │                         │  │  Hub   │
│  │ C · NEXT  (≈65% width, right/start)  │ D · NOTABLE (≈35%,left)│  │        │
│  │                                       │                         │  │  ◔     │
│  │                                       │                         │  │  Visit │
│  │                                       │                         │  │        │
│  └─────────────────────────────────────┴───────────────────────┘  │  ▤     │
│                                                                       │  Plan  │
│  E · AI Presence Line ─────────────────────────────────────── [·]   │        │
└─────────────────────────────────────────────────────────────────────┴────────┘
```

- **Navigation rail** — fixed `88px`, right edge, full viewport height, `--color-bg-inverse` (charcoal). Never resizes with content.
- **Main workspace** — fluid, 12-column grid, outer margin `--space-16` (64px) left/right, `--space-12` (48px) top. Carries ~85% of visual weight.
- **AI companion** — no reserved column at rest; only the Presence Line (a slim strip, not a column) lives in the base layout. Its expanded panel is an *overlay* anchored to the trailing (left) edge, described in §7 — it does not participate in the grid until invoked.
- Sections A–D sit inside the workspace grid in reading order top→bottom; E is fixed chrome pinned to the workspace's bottom edge, always visible, never scrolls away.
- Vertical rhythm between A→B: `--space-8`. Between B→(C/D row): `--space-12` (heaviest gap on the page — Now needs air around it). Between D's items and E: `--space-8`.

---

## 2. Navigation Structure

A collapsed icon rail, not a labeled sidebar menu — navigation should orient, not narrate.

| Item | Icon meaning | Notes |
|---|---|---|
| Home | dwelling/command mark | current screen — active state via `--color-brand-champagne` underline-dot, not a filled background block |
| Patient Hub | a person/record mark | opens to a patient search/context, not a list dump |
| Visit Room | an active-session mark | only visually "live" (subtle accent) when a visit is in progress |
| Treatment Planning | a plan/path mark | — |

- Icons only at rest, `--space-2` vertical rhythm between items, doctor's identity (avatar-less, initials in a plain circle) pinned to the rail's bottom.
- Hover reveals a label tooltip (`--text-meta`, `--color-bg-inverse` background) — the rail never auto-expands into a labeled sidebar, which would eat into the workspace's width budget on a laptop.
- Active state uses a **single small accent mark**, never a colored fill block — fill blocks read as generic SaaS chrome; a mark reads as quiet wayfinding.
- No settings, no notifications bell, no search icon in the rail — those are either inside the workspace (AI Presence Line is the "ask/search" affordance) or one level deeper (profile → settings), keeping the rail's job singular: move between the four core places.

---

## 3. Above-the-Fold Experience

Reading down the workspace in the first ~5 seconds, nothing requiring interaction:

1. **Orientation Strip** (`--text-meta`, `--color-text-muted`, single thin row, `--space-2` padding): *"یکشنبه · ۸ بیمار امروز · شروع کلینیک تا ۱۲ دقیقه دیگر"* — read, not processed, sub-second.
2. **Now** — the eye lands here next by sheer visual weight (largest type, most padding, only element using `--text-display`). Patient name in Peyda, one-line context in `--text-emphasis`, one olive primary button.
3. **Next**, quieter (`--text-body`/`--text-meta`, tighter row height) — confirms the day's shape without demanding reading.
4. **Notable**, only if genuinely populated — on a clean day this column is visibly calm (see zero-state below), never padded with filler.

No skeleton loaders, no staggered fade-ins per section — the page is not permitted to visibly "assemble." If data isn't ready, Now/Next/Notable render in a calm, static placeholder state rather than a spinner (the strategy's "already-composed" requirement).

**Zero-state for Notable:** a single quiet `--text-meta` line — *"چیز مهمی برای گزارش نیست"* — same vertical footprint as one item, no icon, no illustration. Calm is the content.

---

## 4. Now Section

Full workspace width, single open panel (not a boxed "card" with heavy border/shadow — `--shadow-none`, `--border-hairline` only, `--radius-md`, generous internal padding `--space-8`).

**Internal layout, RTL reading order (right → left):**

```
┌──────────────────────────────────────────────────────────────────────┐
│  [status pill]  علی محمدی                              [شروع ویزیت] │  ← --text-display (Peyda)
│                 پیگیری فشار خون · ۹:۳۰ ‑ در ۱۰ دقیقه دیگر            │  ← --text-emphasis (Roboto)
│                 [AI reasoning tag: "HbA1c روند صعودی از آخرین ویزیت"] │  ← --text-meta, --color-ai-accent
└──────────────────────────────────────────────────────────────────────┘
```

- **Right (start):** small status pill (`--color-status-*`, per §5 of tokens) + patient name at `--text-display`.
- **Directly below name:** one-line visit context at `--text-emphasis` — never more than one line; anything more belongs to the Patient Hub, one click away.
- **Optional third line:** an AI reasoning tag (`--color-ai-accent` on `--color-ai-surface`, `--radius-sm`) only if AI elevated this patient for a specific reason beyond the schedule — absent otherwise, never a placeholder.
- **Left (end):** exactly one primary button (`--color-brand-olive` fill). No secondary actions inline — nothing else competes with Now's single verb.
- **Transition state:** when a visit starts/ends, this entire block cross-fades/slides to the incoming Now content (`--shadow-1` momentarily during the move) — the one motion moment the strategy reserves for this section.
- **Between-patients state:** if no visit is imminent, Now shows the next scheduled patient in a slightly desaturated treatment (text at `--color-text-secondary` instead of primary) rather than leaving an empty hero — Now is never blank.

---

## 5. Next Section

Right/start column (~65% of the row), a **list, not a card grid** — rows separated by hairline dividers only, no individual card borders/shadows per row (this is the primary defense against the "many cards" failure mode).

```
NEXT                                                          مشاهده برنامه کامل ←
──────────────────────────────────────────────────────────────────────────
سارا احمدی        ۱۰:۰۰                                   [reason tag ⌐AI]
──────────────────────────────────────────────────────────────────────────
رضا کریمی         ۱۰:۳۰
──────────────────────────────────────────────────────────────────────────
مریم رضایی        ۱۱:۰۰                                   [reason tag ⌐AI]
──────────────────────────────────────────────────────────────────────────
(+ ۴ مورد دیگر امروز)
```

**Row anatomy (RTL):** name at `--text-emphasis` (right/start) · time at `--text-meta` immediately after · reason tag (if AI reordered or flagged this visit) sits at the left/end of the row in `--color-ai-accent`, `--text-meta` — never present on every row, only where earned.

- Section title `النَّـxt` uses `--text-section` (Peyda), with the "see full schedule" affordance as a plain text link (`--color-text-link`), right-justified opposite the title within the RTL row — no button chrome, this is a low-emphasis wayfinding action.
- Bounded to **4 visible rows**; beyond that, a single trailing `--text-meta` line ("+N more today") — never an inline expand-in-place that grows the section's height unpredictably.
- Reordered items (AI moved a later patient earlier) carry the reason tag as their *only* visual difference from a normal row — no color change to the row itself, keeping Next visually calm even when AI has intervened.
- Rows are clickable as a whole (preview/open), not per-field — one predictable interaction target per row.

---

## 6. Notable Section

Left/end column (~35% of the row), same list treatment as Next (hairline-separated rows, no per-item card chrome) — visually related to Next but clearly narrower and quieter, confirming its subordinate weight.

```
NOTABLE
──────────────────────────────────
●  پیگیری PHQ‑9 عقب‌افتاده
   بیمار: نگار حسینی                    [رد کردن]
──────────────────────────────────
●  نتیجه آزمایش دریافت شد
   بیمار: کاوه یزدانی                   [رد کردن]
──────────────────────────────────
(+ ۳ مورد دیگر)
```

**Item anatomy:** a small status/urgency dot (`--radius-pill`, `--color-status-*`) leading the line · one-line verdict+reason at `--text-body` · patient reference at `--text-meta` directly beneath · a quiet dismiss affordance (`--color-text-muted`, text-only, appears on hover — not a persistent icon cluttering every row).

- Hard cap **4 items visible**; overflow collapses to one `--text-meta` line, same pattern as Next, so a doctor learns one "+N more" convention for the whole page.
- Dismissing an item removes it with a brief undo affordance (`--text-meta`, 4–5s window) rather than a confirmation dialog — low-consequence, reversible, per the interaction principles.
- Each item opens directly to the relevant patient context at the flagged detail — not a generic patient landing page.

---

## 7. AI Presence Line

**Rest state** — a single slim strip, fixed to the workspace's bottom edge, full width but with all content anchored to the **left (trailing) end** in RTL, since it's a secondary, invoked-on-demand affordance:

```
                                                    ✦ آماده‌سازی یادداشت برای ۳ ویزیت امروز   [بپرس]
```

- Height: minimal, single-line (`--text-meta`), background transparent/`--color-bg-canvas` — it must read as the quietest element on the page at rest, per the visual language spec.
- Icon/mark and any "already did" text use `--color-ai-accent-muted`; the `[بپرس]` ("Ask") entry affordance is the only piece at full `--color-ai-accent` weight, since it's the interactive target.
- **Silence rule:** if AI has nothing ambient to report, only the `[بپرس]` affordance shows — no "AI is monitoring" placeholder text, no idle icon animation.

**Active state** — activating `[بپرس]` (or a keyboard shortcut) opens a panel as an **overlay from the left/trailing edge**, not a route change:

```
┌───────────────────────────────────────┬──────────┐
│  MAIN WORKSPACE (dimmed --shadow-2      │  AI      │
│  underneath, still visible & in place)  │  panel   │
│                                          │  360px   │
│                                          │  ← edge: │
│                                          │  2px     │
│                                          │  ai-     │
│                                          │  accent  │
└───────────────────────────────────────┴──────────┘
```

- Panel width: fixed `~360px`, `--color-bg-surface`, `--radius-lg` on its leading corner only (the edge touching the workspace stays square, reinforcing "attached beside," not "floating above").
- Workspace behind it dims slightly and becomes non-interactive but stays visually present — the doctor never loses their Now/Next/Notable position while consulting AI.
- Closing (explicit close control, or clicking back into the dimmed workspace) returns instantly, no reload/re-render of the workspace state.
- This exact rest/active pattern is what the doctor will re-encounter, identically, in Patient Hub, Visit Room, and Treatment Planning.

---

## 8. Specialty-Aware Personalization Example

Same structural spec (A–E, identical grid, identical component treatment) — only **content vocabulary** and a **restrained specialty accent** change. Two doctors, same screen architecture:

### Psychiatrist's Home
- Orientation Strip: unchanged structure.
- Now: patient context line reads *"جلسه پیگیری خلق و خو · هفته ششم"*.
- Next reason tag example: *"عدم تکمیل GAD‑7 هفته گذشته"*.
- Notable example: *"بازبینی PHQ‑9 عقب‌افتاده — بیمار: نگار حسینی"*.
- Specialty accent (thin `2px` rule under the Orientation Strip only, never on cards): a muted plum-adjacent tint drawn from the neutral system, not a new saturated brand hue.

### Orthopedist's Home
- Now: patient context line reads *"پیگیری بعد از عمل زانو · روز ۳"*.
- Next reason tag example: *"کاهش ROM نسبت به ویزیت قبل"*.
- Notable example: *"بازبینی تصویربرداری آماده است — بیمار: کاوه یزدانی"*.
- Specialty accent: a different muted tint from the same restrained system.

**What does not change:** section order, Now's single-primary-action rule, Next's 4-row cap, Notable's dismiss pattern, the AI Presence Line's rest/active behavior, typography tiers, spacing rhythm. A doctor moving between specialties (or a multi-specialty practice) relearns nothing structurally — only the words and one thin accent line differ.

---

## What This Screen Deliberately Avoids

| Avoided pattern | How this spec prevents it |
|---|---|
| Generic SaaS dashboard | No KPI tiles, no chart widgets, no "usage" metrics anywhere on Home |
| Many cards | Next/Notable are hairline-divided lists, not card grids; only Now is a single open panel |
| Crowded analytics | Zero numeric/statistical visualization on this screen — trends live in Layer 3, not Home |
| AI chatbot UI | No persistent chat window, no avatar/bubble; AI is one slim strip + an overlay panel invoked on demand |
