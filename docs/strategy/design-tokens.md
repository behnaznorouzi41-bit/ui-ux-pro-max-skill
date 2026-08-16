# Design Tokens — Medical Copilot

Implementation-ready tokens for Next.js + React (CSS custom properties, drop into `globals.css` / Tailwind theme). Source of truth: official brand palette (charcoal, olive, champagne, brown, ivory). Everything not in that set below is a labeled **utility extension** (clinical status, AI), kept minimal and tonally consistent with the brand.

## 1. Color Tokens

### Brand core (official — unchanged)
| Token | Hex | Role |
|---|---|---|
| `--color-brand-charcoal` | `#1E1E1E` | premium text, authority, contrast |
| `--color-brand-olive` | `#39432F` | primary brand / medical trust / primary actions |
| `--color-brand-champagne` | `#D8C6B0` | luxury accent — decorative highlights, premium moments only |
| `--color-brand-brown` | `#76564D` | secondary accent — reserved for AI presence + minor decorative accents (not secondary buttons, see §5) |
| `--color-brand-ivory` | `#E8DDCE` | backgrounds, soft surfaces |

Usage ratio target: **~70% neutral (ivory/white/charcoal), ~20% olive, ~10% champagne+brown combined.** Never split evenly.

### Neutral scale (derived, for surfaces + text hierarchy)
| Token | Hex | Use |
|---|---|---|
| `--color-bg-canvas` | `#FAF7F2` | page background (ivory-tinted, not stark white) |
| `--color-bg-surface` | `#FFFFFF` | cards, elevated panels |
| `--color-bg-muted` | `#E8DDCE` (`--color-brand-ivory`) | subtle fills, hover, selected rows |
| `--color-bg-inverse` | `#1E1E1E` (`--color-brand-charcoal`) | dark chrome (nav rail, inverse panels) |
| `--color-border-subtle` | `#E4DCD1` | default hairline borders |
| `--color-border-strong` | `#C9BBA8` | emphasized borders, input focus rest state |

### Text hierarchy
| Token | Hex | Use |
|---|---|---|
| `--color-text-primary` | `#1E1E1E` | verdicts, headings, primary body |
| `--color-text-secondary` | `#5B5650` | supporting body text |
| `--color-text-muted` | `#8A8378` | metadata, timestamps, placeholders |
| `--color-text-inverse` | `#F5F1EA` | text on dark/olive surfaces |
| `--color-text-link` | `#39432F` (`--color-brand-olive`) | interactive text |

### Clinical status vocabulary (utility extension — closed set)
| Token | Hex | Meaning | Note |
|---|---|---|---|
| `--color-status-stable` | `#39432F` (`--color-brand-olive`) | stable / no action needed | intentional reuse of brand olive ("medical trust"); differentiated by form (small dot/pill) vs. buttons |
| `--color-status-attention` | `#B8863B` | needs review | warm ochre, harmonizes with brown/champagne |
| `--color-status-urgent` | `#9C3B2E` | needs action now | muted brick-red, warm-toned not clinical-bright |
| `--on-status-*` | `#FFFFFF` | text/icon on filled status chips | — |

Status color is **never** used decoratively outside status indicators.

### AI presence color
| Token | Hex | Use |
|---|---|---|
| `--color-ai-accent` | `#76564D` (`--color-brand-brown`) | Presence Line, reasoning tags, AI overlay edge — the *only* functional use of brown |
| `--color-ai-accent-muted` | `#9C8175` | low-confidence AI signal (tint of brown) |
| `--color-ai-surface` | `rgba(118,86,77,0.06)` | AI overlay background wash |

`--color-ai-accent` must never appear on a status indicator; `--color-status-*` must never appear on an AI element.

---

## 2. Typography Tokens

Two-family system: **Peyda Extra Bold** (rare, display-only) + **Roboto** (everything else). RTL-first; verify Roboto's Fa glyph coverage before shipping body copy — fallback to a Persian-native body face if inconsistent.

```css
--font-display: 'Peyda', sans-serif; /* weight 800 only */
--font-body: 'Roboto', sans-serif;   /* 400 / 500 */
```

| Token | Family / Weight | Size | Line-height | Use |
|---|---|---|---|---|
| `--text-display` | Peyda 800 | 32px | 1.2 | Now's patient name, page verdicts |
| `--text-section` | Peyda 800 | 20px | 1.3 | Section titles (Next, Notable, Hub sections) |
| `--text-emphasis` | Roboto 500 | 16px | 1.5 | key inline values, reason tags |
| `--text-body` | Roboto 400 | 15px | 1.6 | standard reading text, AI responses |
| `--text-meta` | Roboto 400 | 13px | 1.5 | timestamps, labels, secondary metadata |

Rules:
- Peyda never drops below `--text-section`, never appears in Layer 3 (dense/tabular) views, never carries Latin-only strings.
- Numerals: **Latin digits (0–9)** for all clinical numerics (dates, scores, dosages) — forced `dir: ltr` inline within RTL text, for clinical precision/interop.
- Letter-spacing on Peyda display tier: `-0.01em` (tightened for authority); Roboto stays at default tracking.

---

## 3. Spacing System

Base unit **4px**. All layout/component spacing is a multiple of it — no arbitrary values.

```css
--space-1: 4px;   /* icon-to-label gaps */
--space-2: 8px;   /* tight grouping within a component */
--space-3: 12px;  /* internal card padding, small */
--space-4: 16px;  /* internal card padding, default */
--space-6: 24px;  /* between related blocks (e.g. Verdict → Action) */
--space-8: 32px;  /* between unrelated cards/sections */
--space-12: 48px; /* section-level separation (e.g. Now vs Next) */
--space-16: 64px; /* page-level margins, laptop outer gutters */
```

- **Component spacing** (internal padding, icon gaps): `--space-1` to `--space-4`.
- **Layout spacing** (between sections, page margins): `--space-8` to `--space-16`.
- Layer 3 (dense tables/history) may drop to `--space-2`/`--space-3` internally — the only place tightening is allowed.

---

## 4. Shape Language

```css
--radius-sm: 6px;   /* inputs, status chips, small controls */
--radius-md: 10px;  /* cards, buttons */
--radius-lg: 16px;  /* modals, AI overlay panel */
--radius-pill: 999px; /* status dots/pills, tags */

--border-hairline: 1px solid var(--color-border-subtle);
--border-focus: 1.5px solid var(--color-brand-olive);
```

### Shadows / elevation
Shadows are near-absent by default — premium restraint, not flat-design dogma. Used only to lift genuinely floating elements above content (never on resting cards).

```css
--shadow-none: none;                              /* default card state */
--shadow-1: 0 1px 2px rgba(30,30,30,0.04);         /* hover/active card */
--shadow-2: 0 4px 16px rgba(30,30,30,0.08);        /* AI overlay, popovers */
--shadow-3: 0 12px 32px rgba(30,30,30,0.12);       /* modals only */
```

Elevation rule: a resting card = `--shadow-none` + `--border-hairline`. Elevation communicates *floating above the page* (overlay, modal), not *importance* — importance is spacing/typography's job (per foundation doc).

---

## 5. Component Tokens

### Cards
```
background:     var(--color-bg-surface)
border:         var(--border-hairline)
radius:         var(--radius-md)
padding:        var(--space-4)
shadow:         var(--shadow-none)  → var(--shadow-1) on hover/active only
state (needs-attention): left border-accent 3px var(--color-status-attention|urgent)
```

### Buttons
| Variant | Background | Text | Border | Use |
|---|---|---|---|---|
| Primary | `--color-brand-olive` | `--color-text-inverse` | none | the one primary action |
| Secondary | transparent | `--color-text-primary` | `--border-hairline` | routine secondary action |
| Ghost/tertiary | transparent | `--color-text-secondary` | none | low-emphasis, rare actions |
| Destructive | transparent | `--color-status-urgent` | `1px solid var(--color-status-urgent)` | record-altering actions — requires confirm step |

Radius: `--radius-md`. Padding: `--space-2` `--space-4`. No brown, champagne, or status colors on buttons — buttons stay in the olive/neutral system only.

### Inputs
```
background:      var(--color-bg-surface)
border (rest):   var(--border-hairline)
border (focus):  var(--border-focus)
radius:          var(--radius-sm)
text:            var(--text-body), var(--color-text-primary)
placeholder:     var(--color-text-muted)
```
No color-only error state: pair `--color-status-urgent` border with an inline icon + text message.

### Status indicators
```
shape:      pill (--radius-pill), fixed small size — never scales with content importance
fill:       var(--color-status-stable|attention|urgent)
text/icon:  var(--on-status-*)  (white)
pairing:    icon + label always accompanies color (never color-only)
```

### AI elements
| Element | Token usage |
|---|---|
| Presence Line (rest) | text `--color-text-muted`, icon `--color-ai-accent-muted`, no fill |
| Presence Line (has update) | icon `--color-ai-accent`, `--text-meta` label |
| Reasoning tag | text `--color-ai-accent` on `--color-ai-surface`, `--radius-sm`, `--text-meta` |
| AI overlay panel | `--color-bg-surface`, left edge `2px solid var(--color-ai-accent)`, `--radius-lg`, `--shadow-2` |
| Confidence indicator | fill opacity of `--color-ai-accent` scales with confidence (never a numeric badge) |

No animation on any AI element at rest (no pulse/glow) — only open/close transitions per motion principles.

---

## 6. RTL Rules

- **Direction:** root `dir="rtl"`, `lang="fa"`. Logical CSS properties only (`margin-inline-start`, `padding-inline-end`, `inset-inline-*`) — never physical `left`/`right`.
- **Navigation:** anchored to the **right** edge (reading-start); nav icons that imply direction (back, expand, chevrons) mirror; product/logo mark does not mirror.
- **Icons:** directional icons (arrows, chevrons, progress) flip with `dir`; content/status icons (checkmarks, alert triangles, medical pictograms, body maps) never flip — mirroring a body map or chart misrepresents it.
- **Numbers:** Latin digits, always `dir="ltr"` inline (`unicode-bidi: isolate`) — applies to dates, scores, dosages, phone numbers, currency.
- **Mixed Persian/Latin content:** wrap embedded Latin fragments (drug names, brand terms) in `<span dir="ltr">` with `unicode-bidi: isolate` so bidi reordering never breaks sentence flow; add `--space-1` inline padding around the direction switch to prevent visual crowding.
- **Text alignment:** `text-align: start` everywhere (never hardcoded `right`) so components stay portable if an LTR locale is ever added.
