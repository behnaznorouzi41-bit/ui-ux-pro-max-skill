# Home / Command Center — UX Architecture

**Status:** UX architecture only — no visual UI, no components
**Builds on:** `medical-copilot-ux-strategy.md`
**Scope:** The first screen a doctor sees each morning, desktop/laptop, Persian RTL

---

## 1. Above-the-Fold Experience

### 1.1 The first 5 seconds
Nothing is clicked, scrolled, or waited for. In the first 5 seconds, before any interaction, the doctor's eyes should land on and register, in this order:

1. **A greeting-scale orientation line** — who they are, what day/time it is, how many patients today. Not a "welcome back" pleasantry — a factual anchor ("Sunday, 8 patients, clinic opens in 12 minutes"). This is read, not processed — it takes under a second.
2. **The current/next patient**, rendered large and unmistakable — the single most visually dominant element on the screen. This is the "Now" anchor (§3.1). The doctor should be able to say the patient's name and reason for visit without reading anything else.
3. **A short queue behind it** — the "Next" rail, visible but visually quieter, confirming the shape of the rest of the morning at a glance (how many, any standing out).
4. **One or two Notable signals**, only if truly warranted — never filling the fold just to look active. If nothing urgent exists outside the schedule, this space stays visibly calm rather than being padded with low-value content.

Nothing below this point competes for attention in the first look. If the doctor stops looking after 5 seconds, they still know today's shape and what's first.

### 1.2 Emotional impact
The target feeling is **relief through preparation**, not welcome-screen positivity. The doctor should feel: *"This already thought about my day before I did."* Concretely, that emotional state is produced by:

- **Absence of a loading/assembling moment** — the page must never visibly "build itself" in front of the doctor (staggered widget pop-ins, spinners per section). It should read as already-composed, the way a well-prepared briefing document is handed to you finished, not written while you watch.
- **Absence of noise-to-signal ambiguity** — every visible element in the fold is something the doctor would independently agree matters. Nothing decorative, promotional, or generically "dashboard-y" (no usage stats, no gamified streaks, no marketing banners).
- **Certainty over volume** — one clearly right first action (go see this patient / look at this flag), not a menu of equally-weighted options to sort through. Decisiveness reads as competence; choice-paralysis reads as "just another system."
- **Restraint as a luxury cue** — the amount of *unused* space in the fold is itself part of the emotional signal. A private-clinic-grade product doesn't need to fill the screen to feel valuable.

If a doctor's honest reaction on day one is "oh — it already knows," the fold has succeeded. If the reaction is "let me find what I need," it has failed, regardless of how much correct information is technically present.

---

## 2. Exact Page Sections

Six sections, top to bottom. No more are added without demoting or merging an existing one — this list is the enforced ceiling, not a starting point.

### Section A — Orientation Strip
- **Purpose:** Ambient self-location: date, time-in-day, clinic/session state, sync/connection state, doctor identity.
- **Priority:** Layer 0 (Status). Lowest visual weight on the page, always present.
- **Shown:** Day/date, count of today's patients, current clinic session state (e.g., "in session" / "before hours"), AI availability indicator.
- **Hidden:** Anything patient-specific, any numeric KPIs (visit counts, revenue, utilization) — those belong to an analytics surface, not this product's home.

### Section B — Now
- **Purpose:** Answer "where am I / who's in front of me" without ambiguity.
- **Priority:** Highest — the single dominant element of the page.
- **Shown:** Current or imminent patient's name, one-line visit reason/context, time position ("in 10 min" / "in progress"), a single primary action (open patient / start visit).
- **Hidden:** Full patient history, chart details, prior visit notes — Now answers "who," not "what about them." That's Layer 2, one click away.

### Section C — Next
- **Purpose:** Confirm the shape of the rest of the day and let urgency reorder it.
- **Priority:** High, visually subordinate to Now (smaller, denser, quieter).
- **Shown:** A short ranked queue of upcoming patients — name, time, and a one-line reason if AI has flagged something about that specific visit (otherwise no reason shown, to avoid manufacturing false signal). Ranking can promote a later, higher-urgency patient above a literally-sooner routine one, with a visible reason for the reorder.
- **Hidden:** Every appointment for the full day if the list is long — Next shows a bounded look-ahead (design target: 3–5 visible), with a single "see full schedule" action to expand, not an infinite scroll of the whole day up front.

### Section D — Notable
- **Purpose:** Surface what doesn't belong to a specific appointment: follow-ups due, results back, AI-surfaced insights on patients not on today's schedule.
- **Priority:** Medium — present but never louder than Now/Next.
- **Shown:** A capped set of items (3–5), each as a verdict + one-line reason + patient reference, each independently dismissible.
- **Hidden:** Anything that isn't actionable today. A trend that's merely "worth knowing" but requires no action belongs in that patient's Hub, not on Home — Notable is for things that justify surfacing *before* the doctor asked.

### Section E — AI Presence Line
- **Purpose:** The single, consistent entry point into active AI interaction (see §4). Not a content section — a persistent affordance.
- **Priority:** Always available, but minimal footprint; never a panel competing with B/C/D for space.
- **Shown:** A quiet, always-reachable prompt/entry affordance, plus (only when relevant) a one-line ambient note of what AI already did this morning ("Prepared notes for 3 visits").
- **Hidden:** Chat history, AI settings, model/confidence configuration — those live in a dedicated AI surface reached *from* here, not inline.

### Section F — Practice Pulse *(optional, lowest priority)*
- **Purpose:** A single, quiet, non-clinical line of context if truly nothing else needs the space (e.g., "no flags today," a light-day acknowledgment). Exists to keep the zero-state calm rather than empty-feeling (see §1.2), never to add busywork.
- **Priority:** Lowest. First section cut on any density pressure, and never shown if it would push Now/Next/Notable below the fold.
- **Shown:** At most one line, non-actionable.
- **Hidden:** Anything resembling analytics, benchmarks, or comparisons — this is not a metrics section and must never grow into one.

**Ordering rule:** A → B → C → D → E, with F only filling true leftover space. On a laptop viewport, A–D must resolve within the first screen (§1.1); E is persistent chrome, not a scroll-order section; F never displaces A–D.

---

## 3. Now / Next / Notable Structure

These three zones are the entire working content of the Command Center. Each has a distinct temporal question, a distinct visual weight, and a distinct interaction pattern — the doctor should be able to tell which zone they're looking at from its weight and rhythm alone, without reading a label.

### 3.1 Now
- **Question it answers:** "What am I doing this instant?"
- **Cardinality:** Exactly one item, always. Never zero (if between patients, Now shows the *next* one in a slightly quieter state rather than leaving a void), never more than one.
- **Behavior:** Now transitions, it doesn't refresh. When a visit starts or ends, the Now item transforms/hands off to what was previously the top of Next — this transition is the one moment on this page where motion is expected to communicate state change (per the strategy's motion principle).
- **Interaction:** One primary action only (open/start). No secondary actions compete here — anything else about that patient is reached by entering their Hub, not from Now itself.

### 3.2 Next
- **Question it answers:** "What's coming, and in what order should I actually think about it?"
- **Cardinality:** Bounded look-ahead (3–5 visible), not the full remaining day.
- **Behavior:** Default order is chronological, but AI is allowed to re-rank when a non-adjacent item carries higher clinical priority than its time slot implies — and when it does, that reorder must be visibly justified (a short reason tag), never a silent shuffle. A doctor should never wonder "why is this one out of order."
- **Interaction:** Scanning, not acting — Next is a confirmation zone. Selecting an item previews/opens that patient's context; it does not "start" a visit early on its own (starting early is a deliberate, separate action once opened).

### 3.3 Notable
- **Question it answers:** "What deserves my attention that isn't already represented by today's schedule?"
- **Cardinality:** Hard-capped (3–5 shown; overflow becomes a single "+N more" affordance, never an expanding inline list).
- **Behavior:** Each Notable item is independently resolvable — dismiss, snooze, or act — and once resolved it leaves the list rather than staying present in a "done" state. The zone's job is to trend toward empty over the course of a session, not to accumulate.
- **Interaction:** Each item opens directly to the relevant context (usually that patient's Hub at the specific flagged detail), skipping any intermediate list-browsing step.

### 3.4 Cross-zone rules
- **No item appears in two zones at once.** A patient who is both "next up" and "has a flag" appears once, in Now/Next, carrying the flag as part of their entry — Notable is reserved for things with no scheduled-visit home.
- **Visual weight strictly descends** Now > Next > Notable, in size, color intensity, and spacing — this ordering must hold even as content changes day to day, so the doctor's scan pattern never has to be relearned.
- **Zones resize, not reflow, with volume.** A light day (few Next items, empty Notable) yields more white space, never layout restructuring; a heavy day compresses density within each zone's existing footprint before ever pushing a zone below the fold.

---

## 4. AI Presence on the Page

The design challenge stated by the brief is precise: AI must be **felt everywhere and seen nowhere as a discrete "AI widget."** This is achieved by treating AI as the *authorship* of the page, not a *tenant* on it.

### 4.1 AI as author, not occupant
Now's ranking, Next's ordering (and reorder justifications), and Notable's entire contents are AI-produced. There is no separate "AI panel" alongside them — removing AI from the page would leave the page empty, not simplified. This is the primary mechanism that satisfies "always available but not distracting" from the source strategy: distraction requires a competing element, and there isn't one.

### 4.2 The one visible seam: the Presence Line
The only place AI is visible *as AI* is Section E — a single, quiet, persistent affordance (never a floating bubble, never a modal that appears unprompted). It does two things and only two:
- Offers a way in to ask something ("active" mode from the strategy's AI philosophy).
- Optionally states, in one line, what it already did ("Prepared notes for 3 visits," "Reviewed overnight results") — evidence of ambient work, not a running log.

This line is the seam between "AI invisibly organized this page" and "AI will now talk to me if I ask" — and it should be the *only* place that seam is visible.

### 4.3 Reasoning is attached, not centralized
Per the strategy's provenance principle, whenever AI has made a judgment call visible on this page (a Next reorder, a Notable item), the "why" sits directly against that item (a short reason tag), not behind a separate "AI insights" click-through. The doctor never has to leave Now/Next/Notable to find out why AI surfaced something — but they also never see reasoning they didn't ask to see, since the reason tag is a short label, not an explanation panel, until they choose to expand it.

### 4.4 Silence is a valid state
On a day with no flags, no reorders, and nothing ambient to report, Section E shows only the entry affordance — no manufactured "AI is working" busy-state, no empty activity feed. An AI that has nothing useful to say says nothing; this is what keeps its presence from becoming a tax on calm days.

### 4.5 Entry point consistency
The Presence Line's affordance is the same visual element and the same behavior the doctor will later find in Patient Hub, Visit Room, and Treatment Planning (per the strategy's cross-context consistency principle) — Home is simply the first place they learn it.

---

## 5. Specialty Intelligence on Home

The Command Center itself has no specialty-specific *sections* — the Now/Next/Notable/Presence structure is identical for every doctor. Personalization happens entirely through **what populates those zones**, following the plug-in model from the strategy (§5).

### 5.1 Reason tags speak the specialty's language
A psychiatrist's Notable item reads "PHQ-9 follow-up overdue"; an orthopedist's reads "ROM regression since last visit"; a dermatologist's reads "Lesion re-check due (6-week interval)." Same component, same position, same visual weight — the vocabulary is what shifts, sourced from each specialty's module data defined in the strategy's architecture.

### 5.2 Next's context line reflects specialty priorities
When Next shows a one-line reason for a specific visit, the *kind* of reason that qualifies as worth surfacing differs by specialty (a mood-tracking discontinuity matters for psychiatry; a missed imaging follow-up matters for orthopedics) — but the mechanism (a short, justified reason tag) is universal, so the doctor's reading pattern never changes across specialties.

### 5.3 A restrained specialty accent, not a re-skin
Per the strategy's color-as-signal principle, a small, consistent accent (not a full re-theme) can mark which specialty context Home is currently oriented toward — useful chiefly for doctors with mixed-specialty practices or shared/multi-doctor devices, so the page confirms "you're looking at your psychiatry day" without requiring a read of every line. This accent must stay subordinate to the urgency color system — specialty color identifies context, urgency color demands attention, and the two must never be confusable.

### 5.4 No specialty-exclusive Home content
Nothing appears on Home that only some specialties have — e.g., there is no "imaging queue" widget that only orthopedists see and psychiatrists don't. If a specialty needs a genuinely distinct surface, it belongs inside Patient Hub or Visit Room (Layer 2), never bolted onto Home as an exception. This is what keeps Home's architecture (§2) permanently a ceiling of six sections regardless of how many specialties the product eventually supports.

### 5.5 Multi-specialty and shared-schedule doctors
For a doctor practicing across specialties in one day, Next simply carries mixed reason-tag vocabularies in sequence — the architecture already supports this without modification, since specialty is a property of each item's content, not of the zone or the page.

---

## 6. Desktop Layout Strategy (Laptop)

### 6.1 Three spatial regions
The laptop layout resolves into three regions with a fixed spatial relationship, described here structurally (no visual design):

- **Navigation** — a persistent, narrow region for moving between the product's core places (Home, Patient Hub, Visit Room, Treatment Planning, per the strategy's shared skeleton). Low visual weight, always present, never competing with page content.
- **Main workspace** — the Command Center content itself (Sections A–D, F): the dominant region of the screen by area, where the Now/Next/Notable hierarchy plays out at full visual weight.
- **AI assistant** — the Presence Line's expanded/active state (§4.2) when the doctor engages it. In its ambient ("collapsed") state it is not a spatial region at all — just a slim affordance living at a consistent edge of the workspace. In its active state, it should behave as a **companion overlay/pane that appears beside the workspace and can be dismissed**, not a permanently reserved column that shrinks the workspace by default. This upholds "always available but not distracting" literally, at the layout level: availability costs a slim edge affordance, not a permanent third column.

### 6.2 RTL spatial resolution
Because the interface is Persian RTL, the reading-direction-first placement principle from the strategy determines the mirrored arrangement: navigation anchors to the side that leads reading flow (the right edge in RTL), main workspace occupies the dominant central-to-trailing area, and the AI Presence Line/overlay anchors to the trailing edge (left in RTL) so it never sits between navigation and the doctor's primary reading path. This isn't a mirror of an LTR layout — it's derived independently from where a Persian reader's eye naturally starts and ends.

### 6.3 Weight distribution, not equal thirds
The three regions are explicitly unequal: navigation is the thinnest (orientation only), the main workspace is the widest and carries nearly all visual weight in the default state, and the AI region claims space only when invoked. On a laptop viewport specifically — more constrained than a desktop monitor — this asymmetry matters more, not less: a permanently-reserved AI column at laptop width would force Now/Next/Notable to compress below their minimum legible density, which directly violates the "calm over crowding" principle. Collapsible-by-default is therefore a laptop-width requirement, not just a stylistic preference.

### 6.4 Overlay behavior, not tab-switching
When the AI region activates, the main workspace should remain visible (dimmed or slightly compressed, not replaced) so the doctor never loses their place in Now/Next/Notable while consulting AI — reinforcing the strategy's rule that AI sits *beside* the doctor's work, never *between* them and it. Closing the AI region returns the workspace to full weight instantly, with no re-loading or re-orientation cost.

### 6.5 Consistency as the payoff
Because this same three-region relationship (navigation / main workspace / collapsible AI companion) is reused across Patient Hub, Visit Room, and Treatment Planning per the strategy's cross-context consistency principle, mastering the Home layout on day one teaches the doctor the spatial grammar of the entire product — Home is the first lesson, not a special case.

---

## Summary Table

| Question | Answer |
|---|---|
| First 5 seconds | Orientation line → dominant Now patient → quiet Next rail → sparse Notable, nothing else competing |
| Emotional target | Relief through preparation — "it already thought about my day" |
| Sections | Orientation Strip, Now, Next, Notable, AI Presence Line, (optional) Practice Pulse — six, hard ceiling |
| Now / Next / Notable | One item / bounded ranked queue / capped resolvable list — strictly descending visual weight |
| AI presence | AI authors the page's content; the only visible AI element is one persistent Presence Line |
| Specialty personalization | Same structure everywhere; only vocabulary and a restrained accent shift per specialty |
| Laptop layout | Thin nav / dominant workspace / AI as a collapsible companion overlay, not a fixed third column |
