# Correspondence · Approved Result

**Status:** Approved by the operator on 2026-07-14 (Gate 1 and Gate 2).
**Nature:** Text-only contract. The disposable proof that earned this approval
was built outside this repository, never had a remote, and is deleted. No proof
source code, screenshots, or generated snapshots exist in this repository; the
clean implementation recreates the world from this contract and the exact
pinned sources.

**Binding law:** `docs/design-laws.md` — this contract yields to the
specification (`docs/superpowers/specs/2026-07-14-living-worlds-design.md`),
and both yield to the laws.

## Provenance

All fidelity works derive from the corpus at commit
`33c360659fed449fc849d4ed651bb3a856b76f12` — never main, never HEAD:

| Route | Source path at that SHA |
|:--|:--|
| `/houses/correspondence` | `corpus/marketing/landing-correspondence.html` |
| `/houses/correspondence/thread` | `corpus/product/chat-correspondence.html` |
| `/houses/correspondence/mat` | `corpus/product/notifications-the-mat.html` |
| `/houses/correspondence/desk` | `corpus/product/settings-the-desk.html` |
| `/houses/correspondence/api` | `corpus/content/api-reference-correspondence.html` |

Two routes are native elevation with no source path:

| Route | Origin |
|:--|:--|
| `/houses/correspondence/patterns` | The Pattern Book — the design system, extracted from the rooms |
| `/houses/correspondence/ledger` | The Ledger Between Boats — approved elevation brief and visual study |

## Approved route list

Seven routes inside one world layout (`/houses/correspondence` + children:
index, `thread`, `mat`, `desk`, `api`, `patterns`, `ledger`), plus `/atlas`
and a not-found page that speaks in the house's voice ("No page keeps this
address.") and offers the Atlas.

## Visual and behavioral invariants

1. **Fidelity first.** The five sourced rooms preserve the subject,
   silhouette, palette, copy, and page-specific chrome of their pinned
   sources. Each page keeps its own document silhouette; no universal app
   shell.
2. **One creed, verbatim, in three rooms:** "The desk keeps what you write;
   it is a principle, not a setting." — the landing's third feat, the desk's
   kept row, the API's refusal card.
3. **The feat trinity** on the landing is The boat · The bell · The desk, in
   that order (the order a letter meets them), each with a line-drawn glyph
   in the house's stroke style.
4. **Night is a second composition.** `html[data-theme="dark"]` recomposes
   every room via page-scoped custom properties; a pre-paint script in
   `index.html` stamps the resolved theme from `localStorage`
   (`vsh-correspondence-theme`) and the system preference, so there is no
   flash. Code is read by lamplight in both skies (the lamp does not change
   with the theme; the API page may deepen it slightly at night).
5. **Retention is a type.** `kept: true` and `queueUntilBoat: true` are
   literal types; the action union contains no member that could change them.
6. **Refusals hold everywhere:** no global notification badge, no live
   countdown pressure, no streak or engagement metric, no destructive draft
   action, no universal dashboard shell, no confetti or arrival animation.
7. **Accessibility is part of fidelity:** zero serious/critical axe
   violations in every room, both themes, both viewports; scrollable regions
   are keyboard-reachable and named; senders are named for the accessibility
   tree; corpus colors that failed AA were darkened minimally on the same hue
   (recorded below).
8. **No horizontal overflow at 390×844** anywhere; wide content scrolls
   inside its own container.
9. **Instruments are subordinate.** Fixed instrument pills sit bottom-left;
   every room's footer (and the thread's composer) carries clearance at
   narrow widths so the pills never sit on the last line.

## Typography roles and palette behavior

- Display: Fraunces (600/550 by role). Reading serif: EB Garamond, including
  italic registers. Chrome/labels: Avenir Next / system sans. Machine voice:
  ui-monospace stack with wide letter-spacing and uppercase for stitched
  captions.
- World tokens at `:root` with dark overrides, page-scoped refinements per
  room: `--paper #f6f3ea`, `--panel #fcfaf3`, `--line #d9d2c0`,
  `--line-soft #e8e2d1`, `--line-strong #c2bba8`, `--ink #26231b`,
  `--ink-2 #5d5747`, `--ink-3 #6c6658`, `--lapis #2b4d8f`,
  `--lapis-tint #e6ebf5`, `--green #275c3b`, `--green-soft #e1eee5`,
  `--cream-tint #f7f2e2`, `--madder #85301f`, `--madder-soft #f5e2dd`,
  `--sand #856829` (world-level, darkened from the mat's own `#8a6c2a` so
  sand can speak as small text at ≥4.5:1), `--sand-tint #f4eedb`, and the
  constant lamplight set `--night #16171c`, `--night-line #2b2c33`,
  `--lamp-ink #d8d3c6`, `--lamp-dim #868072`, `--lamp-blue #92a3bd`,
  `--lamp-green #83ac8f`, `--lamp-ember #cd8a6d`.
- Dark counterparts: paper `#121316`, panel `#1a1b1f`, line `#2b2c31`,
  ink `#e5dfd2`, ink-3 `#948d7c`, lapis `#92a3bd`, green `#6cae87`,
  sand `#c0a05e` on `#26200f`, madder `#cf7663` on `#2b1712`.
- Contrast corrections applied to corpus values (same hue, minimal move):
  light ink-3 → `#6c6658`; dark ink-3 → `#948d7c`; API lamp-dim → `#868072`;
  thread dark send-button text → `#14161a`; world sand → `#856829`.
- The ember (lapis primary) is spent once per page; dark theme darkens ember
  button text to `#14161a` rather than lightening the ground.

## The design system: furniture and The Pattern Book

Fourteen shared components live in the world
(`src/worlds/correspondence/furniture/`), extracted from the rooms' canonical
CSS and consuming world tokens so every piece composes under either sky:

Button (ember/quiet/bare) · FilterPill · LabelPill · MethodStamp
(get/post/delete = green/lapis/madder wax) · StitchedDivider · Whisper (the
quiet-save pill with the green ✔ mark; `held` state for cataloguing) ·
SwitchControl · SkyCard · NoticeCard (plain and raised sand registers) ·
CodeLamp with lamplight registers (Cm/K/S/E) · MessageBubble (tomaz left on
cream, elena right on lapis) · WordCard · KeptSeal · FeatCard ·
AttachmentChip · VoicedTable.

The rooms consume the furniture (landing feats, thread word card and
attachment, mat filters, desk switches/sky/whisper, API stamps). The
extraction was proven faithful: after the refactor, all room captures passed
pixel-for-pixel against the pre-refactor baselines.

`/houses/correspondence/patterns` — The Pattern Book — catalogues all
fourteen plates: each piece alone in every register and state, then at work
inside an in-context vignette, annotated in the house's voice, with a sticky
rail of plates. It passes the same review gates as the rooms.

## Shared state: commands and consequences

One reducer over one reality (types fixed in the Living Worlds plan):

- `draftChanged(id, body)` — the thread's composer is the kept draft; drafts
  are upserted, never lost.
- `letterSealed(id, departure "09:00" | "16:00")` — the thread's ember says
  "Seal for four" (accessible name "Seal for the four o'clock boat"); the
  API sandbox ("Post this letter from your desk") dispatches the same
  command and prints a deterministic 202 with `departure`,
  `status: "queued"`, `kept: true`.
- `boatCrossed(departure)` — moves only that boat's queued letters to
  crossed; writes exactly one boat notice ("The … boat crossed; its letters
  are with their readers.") and, only if the bell is on, one letter notice.
- `noticeRead(id)` / `noticesSwept` — the Mat's TODAY group and sweep;
  sweeping marks read, never deletes.
- `bellChanged(enabled)` / `quietBeforeEightChanged(enabled)` — the only two
  desk rows that write shared settings. There is no `keptChanged` and no
  `queueUntilBoatChanged`.

State lives in a provider mounted at the world layout for the session;
cross-route journeys travel through the Atlas (client-side navigation).

## The Ledger Between Boats

One day, not an infinite history, ruled by two named crossings (09:00 and
16:00) drawn as sand-colored stitched rules off a single quiet vertical
thread. Letters are kept objects placed by status: draft on dashed cream
("kept as draft"), queued on lapis ("kept for four" / "kept for nine", with
the literal departure in the manners line), crossed under the green second
mark ("✔✔ crossed"). The interval is titled "The day between" and states
that waiting protects the work. Deterministic review controls ("Advance the
nine/four o'clock boat") advance one boat each, inside a dashed review fence.
It is not a feed, timeline, board, dashboard, or chart.

## Atlas and lens boundaries

- The Atlas opens over any page (⌘K/Ctrl+K, ignored inside editable fields)
  without moving underlying content, traps focus, returns focus on close,
  and lists every world route from the route registry ("Studies · not yet
  admitted" for the rest). It is the world's traveler; journeys that must
  keep state use it.
- The system lens defaults off, answers in structured layers (foundations /
  behavior / source), marks the sourced rooms with their corpus paths and
  the patterns and ledger routes as authored, and leaves zero artifacts in
  the DOM when closed.
- Fixed-position elements cannot be represented truthfully in stitched
  full-page captures; review tooling neutralizes them (and fixed paper
  washes, and the thread's internal scroll) at capture time only, after axe
  runs against the real layout.

## Rejected alternatives

- "Kept" as the landing's third feat heading (weak progression; replaced by
  The desk completing the trinity) — operator-directed.
- Instrument pills overlapping room footers (subordinated with clearance).
- A send button in the thread (the composer never says send now).
- Countdown, badge, streak, chart, or feed idioms anywhere, including the
  Ledger.
- `--sand #8a6c2a` as small text (4.44:1 — fails AA; darkened to `#856829`).
- Persisting shared state (deferred; provider state is session-scoped in the
  proof era).

## Mechanical gate at approval

- Provenance verified against the exact SHA on every run.
- Biome format/check: clean. oxlint `--deny-warnings`: clean. Two tsc
  projects: clean. Production build: clean.
- 44 unit tests across 14 files (reducer invariants, furniture variants,
  every room, the Pattern Book, the Ledger, world meta, theme, provenance).
- 37 Playwright tests: 28 deterministic full-page captures (7 routes × 2
  themes × 2 viewports) each also gated on zero horizontal overflow, zero
  serious/critical axe violations, zero console errors/page errors/failed
  or external requests; instrument journeys; accessibility journeys; two
  cross-route elevation journeys. Whole suite deterministic across two
  consecutive runs.

## Operator approvals

- Gate 1 (fidelity + design system after one directed rework): 2026-07-14.
- Elevation brief and Ledger visual study: 2026-07-14.
- Gate 2 (elevated world): 2026-07-14.
