# Visual Design System — Foundation

**Status:** Design foundation only — no screens, no components, no code
**Builds on:** `medical-copilot-ux-strategy.md`, `command-center-ux-architecture.md`
**Scope:** Desktop/laptop, Next.js + React, Persian (RTL)

This document defines the *rules* the visual system must obey, not the visual system itself. Every number and ratio below is a foundation to build tokens and components against later — not a final palette or spec.

---

## 1. Visual Personality

### 1.1 The emotional target
A doctor should perceive this product the way they'd perceive a highly competent, unobtrusive colleague in a well-run private clinic: **precise, calm, quietly confident.** Not clinical-cold (a sterile hospital system), not consumer-playful (a wellness app), not enterprise-dense (a hospital ERP). The closest reference feeling is a **premium diagnostics suite or a private banking terminal** — instruments built for professionals who are under pressure and need clarity, not persuasion.

### 1.2 Three personality axes
- **Premium, not decorative.** Quality is communicated through restraint, proportion, and material quietness (few colors, considered spacing, no ornament) — never through gradients, glows, or "delight" flourishes that a clinical tool doesn't need.
- **Intelligent, not futuristic.** The product is AI-native but must not visually cosplay as "AI software" (no neon gradients, particle effects, glassy sci-fi chrome). Intelligence is signaled through *organization* — things already sorted, ranked, and explained — not through visual tropes borrowed from consumer AI products.
- **Medical, not sterile.** Enough warmth in neutrals and typography to avoid feeling like a hospital kiosk, while status/urgency color stays unambiguous and clinically legible. Trustworthy reads as "considered," not "cold."

### 1.3 What the product must never look like
A generic SaaS admin dashboard (card-grid-of-everything aesthetic), a consumer health app (rounded, bright, gamified), or a "look how advanced our AI is" showcase (heavy AI-branding chrome, constant animation, chat-bubble-everywhere UI). If a screen could be mistaken for a generic analytics dashboard with the labels swapped, the personality has failed.

### 1.4 The single sentence test
Every visual decision gets checked against: *does this make the doctor feel more prepared, or does it make the product feel more impressive?* Prepared wins, always.

---

## 2. Layout System

### 2.1 Desktop/laptop grid
A **12-column grid** at the laptop breakpoint (design target ~1280–1440px content width), with generous outer margins rather than edge-to-edge content — premium software gives its content room to breathe rather than maximizing density. Column spans are used structurally to express the strategy's hierarchy directly in layout:
- Dominant content (Now, a patient's primary context) claims wide spans (structurally ~7–8 columns).
- Subordinate content (Next, secondary context panels) claims narrower spans (~4–5 columns).
- Navigation and the collapsed AI affordance live outside the 12-column content grid entirely, in fixed-width chrome regions, so they never compete with content for grid math.

Gutters are wide enough to read as separation, not just spacing (structurally larger than typical dense-dashboard gutters) — this is one of the primary tools for the "calm, not crowded" feeling.

### 2.2 RTL structure
RTL is a first-class layout axis, defined independently rather than mirrored from an LTR assumption:
- **Reading flow starts at the right edge.** Navigation anchors right; primary content begins at the right and flows left; the AI companion (per the Command Center architecture) anchors to the trailing left edge, since it's a secondary, invoked-on-demand region.
- **Hierarchy within a row reads right-to-left.** In any horizontal grouping (e.g., a patient entry with name, tag, and time), the most important element sits at the visual start (right), consistent with how Persian text is scanned.
- **Numerals and mixed-direction content get an explicit rule.** Persian text, Latin drug names, and numeric values (dates, lab values, dosages) will co-occur on the same line. The system must define — as a layout rule, before any component is built — that numerals and Latin fragments stay internally LTR while embedded in an RTL sentence, with consistent spacing around the direction switch so lines never visually stutter.
- **Icons and directional affordances mirror; status/medical iconography does not.** Navigational chevrons, back arrows, and progress indicators flip for RTL. Clinical icons/pictograms (body maps, anatomical diagrams, charts) stay in their standard orientation — mirroring an anatomical diagram would misrepresent it.

### 2.3 Spacing principles
- **A single modular spacing scale**, not ad hoc values — every gap, padding, and margin in the product is drawn from one scale so rhythm stays consistent across Command Center, Patient Hub, Visit Room, and Treatment Planning.
- **Spacing communicates grouping, not decoration.** Related elements (a verdict and its supporting reason) sit close; unrelated elements (two different Notable items) get a full step of separation. A doctor should be able to tell what belongs together from spacing alone, without borders or dividers doing that job.
- **Whitespace scales with importance, not just hierarchy level.** The Now section (§ Command Center architecture) gets the most breathing room on the page, reinforcing its dominance through space as well as size — space itself is a hierarchy signal, not just a gap-filler.
- **Borders and dividers are a last resort.** Given the minimal/Swiss-influenced direction appropriate to this positioning, grouping and separation should be achieved through spacing and subtle background differentiation before reaching for a rule line — visual noise from unnecessary borders is one of the fastest ways to make medical software feel cluttered.

### 2.4 Density rules
- **Default density is spacious**, matching the "high white space" mandate from the source strategy — this is a premium product, not a data-dense operations console.
- **Density may only tighten in Layer 3 (Detail & History)** — full timelines, complete instrument results, archived data — where a doctor has deliberately opted into browsing depth and a denser, tabular treatment is appropriate and expected.
- **Layers 0–2 (Status, Priorities, Context) never adopt dense/tabular treatment**, even under content pressure. If a section is at risk of feeling crowded, the fix is progressive disclosure (show less, offer more on demand) — never shrinking spacing to fit more in.
- **Density is consistent within a layer across specialties.** An orthopedic Patient Hub and a psychiatric Patient Hub carry the same spacing rhythm even though their content differs — this is what makes the specialty plug-in architecture feel like one product.

---

## 3. Component Philosophy

These are behavioral contracts for component *families*, not specs for individual components — no components are being designed yet.

### 3.1 Cards
- A card represents **one unit of judgment** — a patient, a flagged item, a single assessment result — never a generic container for unrelated content stacked together.
- Every card follows the same internal reading order as the information typing system from the strategy: **Verdict → Signal/Context → Action**, top to bottom (or start-to-end in RTL), so a doctor's scan pattern transfers between every card in the product regardless of what it represents.
- Cards are visually quiet by default (minimal or no border/shadow) — elevation and emphasis are reserved to signal *state* (e.g., "this needs attention") rather than used decoratively on every card equally. If every card looks equally important, none of them do.
- Cards do not nest cards. A card may contain a status indicator, text, and an action — never another full card — to prevent the "cards all the way down" density problem common in dashboard software.

### 3.2 Information blocks
- Information blocks (a data field, a stat, a short passage) always declare their type visually (Verdict/Signal/Context/Action per the strategy) through consistent, reused visual treatment — a doctor should recognize "this is a verdict" from its treatment before reading its content, on any screen in the product.
- Labels are minimized in favor of self-evident presentation. A number formatted and positioned like a "verdict" doesn't need a label saying "Status:" in front of it — over-labeling is itself a form of clutter in a system aiming for calm.
- Empty or "nothing to report" states are treated as first-class, reassuring content (a calm, explicit "no flags" reads as care, not as a missing feature) — never left as visually broken blank space.

### 3.3 Actions
- **One primary action per context**, always visually singular (per the strategy's "one primary action" principle) — every other available action is visually secondary, and tertiary actions (rare, destructive, or configuration-adjacent) are deliberately de-emphasized further still.
- Action language is **verb-first and specific** ("Start visit," "Review result") rather than generic ("OK," "Submit," "Continue") — specificity itself builds trust that the system knows what it's doing.
- Destructive or high-consequence actions (anything that alters the medical record, dismisses a clinical flag) carry a distinct, consistent visual treatment separate from routine actions, and require a deliberate confirmation step — this is a direct extension of the strategy's "AI suggests, doctor confirms" principle into the action system itself.
- Actions never rely on color alone to communicate their nature (destructive vs. routine vs. AI-suggested) — shape, weight, or icon accompany color, per accessibility discipline.

### 3.4 Status indicators
- A **single, closed vocabulary** of clinical status states (e.g., stable / needs review / urgent — the exact set to be finalized, but it must stay small and closed) used identically everywhere in the product — Command Center, Patient Hub, specialty modules. A doctor learns this vocabulary once.
- Status is never color-only. Every status indicator pairs color with shape/icon and a text label, satisfying both the accessibility guideline against color-only meaning and the higher stakes of a clinical-error context.
- Status indicators are small and consistent in size regardless of context — they are a signal, not a decorative badge, and must never grow more prominent than the content they're describing.
- Urgency-tier status (the "needs attention now" tier) is the only status treatment allowed to claim strong color — this scarcity is what keeps it meaningful; if every state uses saturated color, urgency stops standing out.

### 3.5 Medical data visualization
- Every clinical visualization (a trend line, a body map, a lesion comparison, an assessment score chart) leads with an **interpretation, not just a rendering** — a one-line verdict sits with the chart, so a doctor isn't required to visually interpret a trend line unassisted to know if it's good or bad news.
- Visualizations follow the same progressive disclosure rule as everything else: a compact, verdict-carrying preview at Layer 2, full interactive/detailed visualization at Layer 3.
- A **restrained, purpose-built visual vocabulary per data type** (trends over time, spatial/mapped data like pain or lesion maps, comparative before/after imagery, instrument/score results) — each type gets one consistent treatment reused across every specialty that needs it, rather than each specialty inventing its own chart style. This directly implements the strategy's "shared tracking module" architecture visually.
- Color in medical visualizations is reserved for clinical meaning (severity, change direction, out-of-range values) — never used for categorical decoration (e.g., coloring lesions or trend lines by arbitrary series color) where that would compete with or dilute the status color vocabulary from §3.4.
- Medical imagery (actual photos, scans) is presented with maximum neutrality around it — no colored frames, no decorative treatment — so nothing in the interface competes with or distorts perception of the clinical image itself.

---

## 4. Typography System

### 4.1 Roles, not just fonts
**Peyda Extra Bold** is a *voice for conclusions*: headlines, verdicts, patient names in dominant contexts (Now), section identity at the top of a major view. It is used sparingly and briefly — short strings only, never body copy, never long labels. Its entire value is that its rarity makes it authoritative; overusing it flattens that signal.

**Roboto Regular** (with its weight range as needed — Regular for body, a mid weight for emphasis within body/UI, never Bold competing with Peyda's role) is the *voice for everything operational*: body text, UI labels, data values, form content, AI conversational text. It is the default typeface for the vast majority of the interface by character count.

### 4.2 Hierarchy scale (structural, not final sizes)
A single modular type scale spans both typefaces, with Peyda Extra Bold occupying only the top 1–2 steps:

| Tier | Typeface | Use |
|---|---|---|
| Display / Verdict | Peyda Extra Bold | Now's patient name, page-level verdicts, major section identity |
| Section Title | Peyda Extra Bold (smaller step) | Section headers (Next, Notable, Patient Hub sub-sections) |
| Emphasis Body | Roboto (mid weight) | Important inline values, reason tags, key data points |
| Body | Roboto Regular | Standard reading text, descriptions, AI responses |
| Support / Meta | Roboto Regular (smaller step) | Timestamps, labels, secondary metadata |

Every tier below Section Title stays in Roboto — Peyda never appears at body-text scale or in dense/tabular contexts (Layer 3), where its display character would work against legibility and density.

### 4.3 RTL and mixed-script typography
- Peyda is built for Persian display type and must carry the visual weight of headlines entirely in Persian — it is not expected to render Latin characters gracefully, so any Latin content in a headline-tier string (rare — e.g., a brand or drug name) needs a defined fallback rather than forcing Peyda to render it.
- Roboto has full Latin coverage but is being asked to carry Persian body text too; the system must verify Roboto's Persian glyph support and line-height behavior against Peyda's, and define a fallback Persian body face if Roboto's Farsi rendering proves visually inconsistent with the rest of the system — this is a foundational typography risk to resolve before body text is finalized, not an implementation afterthought.
- **Numerals policy:** a single consistent choice (Persian or Latin digits) for clinical numerals (dates, dosages, scores) across the entire product, chosen for clinical clarity and consistency with how doctors already read charts and prescriptions in Persian practice — not left to per-context default.
- Line-height and letter-spacing are tuned per script rather than reusing Latin-typical values, since Persian script's vertical rhythm and joining behavior differ from Latin — this affects every tier, especially body copy at Roboto.

### 4.4 What typography must never do
Never use size or weight alone to imply clinical urgency (that's the status/color system's job, kept separate) — typography signals *information type* (verdict vs. body vs. meta), color and status indicators signal *clinical meaning*. Conflating the two systems would make both harder to read at a glance.

---

## 5. Color Strategy

No final brand hex values exist yet — this section defines **roles and behavior**, with indicative anchors (marked *provisional*) to show what kind of palette satisfies the direction, to be finalized against real brand color once supplied.

### 5.1 Structure: four independent color roles
The system keeps four color roles strictly separate so they never get confused with each other, even though they'll all appear on the same screen:

1. **Neutral base** — the vast majority of the interface (backgrounds, cards, body text, borders). Carries the "premium private clinic" feeling on its own, before any accent or status color is added.
2. **Primary/brand accent** — the product's identity color, used narrowly (primary actions, key interactive moments, the product's own chrome) — not spread across content.
3. **Clinical status vocabulary** — a small, closed set (e.g., stable / attention / urgent, provisionally: a calm green-adjacent, a warm amber, a clear red) reserved *exclusively* for clinical meaning per §3.4. This set must never be reused for anything non-clinical (no "urgent-red" marketing banner, no decorative amber highlight).
4. **AI presence color** — see §5.4 — a distinct hue from both the brand accent and the status vocabulary, used minimally and specifically.

### 5.2 Primary usage
The brand/primary accent appears in a **small number of specific, learnable places**: the single primary action on any screen, key active/selected states in navigation, and the product's own identity marks. Provisionally, a restrained, low-saturation hue (candidates in the deep teal/ink-blue or warm graphite-with-metallic-accent family, per the "premium + medical" precedent — not the bright cyan/teal common to generic health apps, to satisfy the "unique" requirement) rather than a loud brand color competing with clinical status meaning.

### 5.3 Accent and specialty usage
A secondary, more restrained accent tier — provisionally including the specialty-context accent introduced in the Command Center architecture (§5.3 of that document) — is used only for *context identification* (which specialty am I in), never for emphasis or action. This tier must always read as visually subordinate to both the primary accent and the status vocabulary, so a doctor never mistakes "this is psychiatry's accent color" for "this needs attention."

### 5.4 AI presence color behavior
AI needs to be recognizable *as AI* wherever it appears (the Presence Line, reason tags it authors, its active/overlay state) without invoking urgency or brand-action meaning. This calls for a **fourth, distinct hue** — provisionally a quiet, slightly cooler or more muted tone than the primary accent — used exclusively for: the Presence Line's affordance, reasoning tags attached to AI-authored content, and the AI companion overlay's edge/framing when active.

Behavior rules:
- AI color appears **only at the seam** where AI is visibly acting (per the Command Center architecture's §4.2) — never as a background wash or a pervasive tint implying "AI is watching everything," which would undercut the calm, ambient positioning.
- AI color's saturation/presence can modulate slightly with confidence (a lower-confidence suggestion rendered slightly more muted) — but this is a subtle secondary signal, never a replacement for the explicit confidence/provenance display the strategy already requires.
- AI color must remain visually distinguishable from the clinical status vocabulary at a glance — critically, it must never be confusable with the "urgent" status color, since AI flags and clinical urgency flags will sometimes sit near each other on Notable.

### 5.5 What color must never do
Never used for pure decoration (gradients, illustrative color, marketing-style accent) — in this system, if something has color, it means something. Never let brand/primary accent and clinical-urgent status share a hue family, even at different values — a doctor's split-second color read must never require thinking about which system they're looking at.

---

## 6. AI Visual Language

### 6.1 A visual signature, not a character
AI is represented through **consistent placement, motion, and the dedicated AI color (§5.4)** — not through an avatar, mascot, or anthropomorphic chat bubble. This keeps AI feeling like a capability of the product (per the strategy's "colleague, not chatbot" principle) rather than a separate personality layer.

### 6.2 The Presence Line's visual treatment
The single persistent AI affordance (from the Command Center architecture) is deliberately the quietest interactive element on any screen at rest — smaller and lower-contrast than primary actions — and only gains visual presence (via the AI color) when it has something to report or when actively engaged. Its resting state should almost recede; its active state should be unmistakable. This range — quiet by default, clear when needed — is itself the core of the AI visual language.

### 6.3 Reasoning tags
Wherever AI justifies a ranking or flag (Next reorders, Notable items), the reasoning tag uses a small, consistent visual pattern (AI color, Roboto support-tier type) distinct from both the status indicator it's attached to and from generic metadata — a doctor should be able to tell "this label is AI's reasoning" from its treatment alone, before reading it.

### 6.4 The active/overlay state
When the AI companion expands (per the layout strategy), its visual framing — edge treatment, entry/exit motion — consistently uses the AI color as its identifying signature, and the underlying workspace dims or recedes slightly rather than disappearing, reinforcing "beside," not "instead of" (§6.4 of the Command Center architecture).

### 6.5 Confidence and provenance made visible
Per the strategy's trust principle, any AI output carrying a confidence/basis signal expresses it through a restrained, consistent visual device (not a numeric percentage badge, which reads as falsely precise for clinical judgment, and not an intrusive disclaimer block) — something closer to a calm, glanceable gradation (e.g., how filled/solid the AI-color marker appears) paired with the ability to expand to full reasoning on demand.

### 6.6 What AI's visual language must never do
No pulsing, glowing, or looping animation to signal "AI is thinking" or "AI is present" — motion is reserved for actual state change (§7), and ambient AI busy-indicators are exactly the kind of manufactured activity the strategy's calm principle rules out. No separate visual "skin" for AI-generated content blocks (no colored background wash around AI text) beyond the reasoning-tag/Presence Line signature — over-marking AI content everywhere it appears would reintroduce the "AI widget" problem the Command Center architecture explicitly avoids.

---

## 7. Motion and Interaction Principles

### 7.1 Motion has one job: explain state change
Motion is used exclusively to make a transition **comprehensible** — never to add polish, delight, or brand personality for its own sake. Every animated moment must answer "what changed and why" for the doctor watching it.

### 7.2 Where motion is expected
- **Now's handoff** (a visit ending, the next patient becoming current) — the defining animated moment on the Command Center, per the architecture doc.
- **Layer transitions** (Layer 1 → 2, e.g., opening a patient from Next or Notable) — a clear sense of "drilling in," so the doctor never loses spatial orientation about where they came from.
- **Notable resolution** (an item being dismissed/resolved and leaving the list) — confirms the action landed.
- **AI companion open/close** — reinforces the "overlay beside the workspace" relationship from the layout strategy.
- **Status changes** (a status indicator updating) — a brief, restrained transition so a doctor catches the change even if their eyes were elsewhere for a moment.

### 7.3 Where motion is forbidden
Nothing animates to attract attention to new content arriving (no bouncing badges, no sliding-in notifications) — per §1.5 of the strategy, that job belongs entirely to the color/status system, not motion. No looping or idle animation anywhere (no ambient background movement, no idle AI indicator) — a static screen is the correct resting state for a calm clinical tool. No animation on routine, high-frequency interactions (hover states, simple selections) beyond the minimum needed for responsiveness feedback — motion is reserved, not default.

### 7.4 Character of motion
Fast and purposeful rather than showy — transitions should feel closer to a well-made physical instrument's response (immediate, slightly damped, no bounce or elastic overshoot) than to a consumer app's playful easing. Duration and easing should be short enough that a doctor moving quickly through patients never feels the interface is making them wait for its own animation.

### 7.5 Interaction principles
- **Every interactive element has one obvious afford­ance state and one obvious result** — no interactions that require discovery (hidden gestures, hover-only reveals of primary information) given the deliberately non-decorative, professional-instrument positioning.
- **Confirmation scales with consequence.** Routine navigation is instant with no confirmation; actions that touch the medical record or dismiss a clinical flag require a deliberate, explicit confirmation step, consistent with the strategy's "AI suggests, doctor confirms" rule extended to the interaction model generally.
- **Undo over confirmation where safe.** For low-risk, reversible actions (dismissing a Notable item, collapsing a section), prefer an immediate action with a brief undo option over a blocking confirmation dialog — this keeps the interface fast without sacrificing safety, reserving hard confirmation dialogs for genuinely high-consequence actions.
- **Keyboard and precision-input support is a first-class expectation**, not an accessibility afterthought — this is a laptop-first professional tool used repeatedly through a clinical day, and doctors will expect to move through patients and confirm routine actions without reaching for a trackpad every time.

---

## Summary Table

| Dimension | Core rule |
|---|---|
| Personality | Precise, calm, quietly confident — premium diagnostics instrument, not SaaS dashboard or consumer health app |
| Layout | 12-col laptop grid, generous margins, RTL-native (not mirrored), spacious by default, density only in Layer 3 |
| Cards | One unit of judgment, Verdict→Signal→Action order, quiet by default, never nested |
| Actions | One primary action always, verb-first language, consequence-scaled confirmation |
| Status | Small closed vocabulary, never color-only, urgency color kept scarce to stay meaningful |
| Data viz | Interpretation before rendering, one treatment per data type reused across specialties |
| Typography | Peyda Extra Bold for rare conclusions, Roboto for everything operational, RTL/numeral rules defined explicitly |
| Color | Four independent roles — neutral base, primary accent, closed status vocabulary, distinct AI color — never overlapping in meaning |
| AI visual language | Signature via placement/color/motion, not a character; quiet at rest, clear when active; no busy-indicator animation |
| Motion | Explains state change only; forbidden for attention-getting or idle decoration; fast and damped, not bouncy |
