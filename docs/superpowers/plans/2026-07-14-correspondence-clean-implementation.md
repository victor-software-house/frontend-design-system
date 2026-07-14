# Correspondence Clean Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Recreate the operator-approved Correspondence living world — five fidelity rooms, the furniture design system with The Pattern Book, shared state, and The Ledger Between Boats — inside this repository, from the approved-result contract and the exact pinned sources only.

**Architecture:** A Vite + React SPA under `apps/worlds/` (new workspace app). One world layout mounts a state provider and the instruments; each room is a route with its own CSS Module and page-scoped custom properties over shared world tokens; fourteen furniture components are the design system; a deterministic Playwright matrix is the review instrument.

**Tech Stack:** Bun (package manager + test runner), React (latest), react-router (latest), Vite (latest), TypeScript, Vitest + jsdom + Testing Library, Playwright + @axe-core/playwright, Biome, oxlint.

**Binding law:** `docs/design-laws.md` — this plan yields to
`docs/superpowers/specs/2026-07-14-correspondence-approved-result.md`, which
yields to `docs/superpowers/specs/2026-07-14-living-worlds-design.md`, and all
yield to the laws.

## Global Constraints

- Clean room: never read, copy, or consult the deleted disposable proof; the
  inputs are the approved-result contract, the Living Worlds spec, the design
  laws, and the corpus at `33c360659fed449fc849d4ed651bb3a856b76f12`.
- Corpus checkout is pinned: clone into a sibling scratch directory and
  `git checkout 33c360659fed449fc849d4ed651bb3a856b76f12` — never main/HEAD.
  A provenance script fails the gate if the SHA or the five source files
  drift.
- Latest stable versions of every dependency, verified at install time
  (`bun add <pkg>` resolves latest; do not pin older majors).
- Theme storage key: `vsh-correspondence-theme`; pre-paint stamp script in
  `index.html` sets `data-theme-mode` and resolved `data-theme` on `<html>`.
- All copy, palette values, typography roles, refusals, and status language
  come verbatim from the approved-result contract; where the contract is
  silent, the pinned source is the authority; where both are silent, the
  design laws decide.
- Every route passes, in both themes and both viewports (1440×900, 390×844):
  zero horizontal overflow, zero serious/critical axe violations, zero
  console errors / page errors / failed requests / non-local requests.
- Review captures neutralize fixed-position elements (instrument pills,
  fixed paper washes, the thread's internal scroll) at capture time only,
  after axe runs against the real layout.
- Conventional Commits; no AI attribution anywhere.

## File Structure

```
apps/worlds/
├── package.json, tsconfig.json, tsconfig.scripts.json, vite.config.ts,
│   vitest.config.ts, playwright.config.ts, biome.json, index.html
├── proof-source.json               # SHA + five source paths
├── scripts/check-provenance.ts     # + .test.ts
├── src/
│   ├── main.tsx, router.tsx, app.css, test-setup.ts
│   ├── theme/theme.ts, theme/theme-toggle.tsx (+ tests)
│   ├── atlas/atlas-dialog.tsx (+ module css)
│   ├── lens/system-metadata.ts, lens/system-lens.tsx (+ module css)
│   └── worlds/correspondence/
│       ├── world.css               # tokens, fonts, dark overrides
│       ├── world-layout.tsx        # provider + instruments + outlet
│       ├── world-meta.ts, fidelity-contract.ts (+ test)
│       ├── test-support.tsx        # renderInWorld helper
│       ├── fonts/*.woff2           # extracted from corpus base64
│       ├── state/correspondence-state.ts, correspondence-reducer.ts,
│       │   correspondence-provider.tsx (+ reducer test)
│       ├── furniture/              # 14 components + furniture.module.css
│       │   └── index.ts, furniture.test.tsx
│       ├── components/theme-control.tsx, use-reveal.ts,
│       │   instrument-triggers.tsx
│       └── routes/{landing,thread,mat,desk,api,patterns,ledger}/
│           └── <room>-page.tsx + .module.css + .test.tsx
└── tests/
    ├── support/routes.ts, support/review-page.ts
    └── e2e/fidelity.spec.ts, instruments.spec.ts,
        accessibility.spec.ts, elevation.spec.ts
```

---

### Task 1: Scaffold, provenance gate, and theme foundation

**Files:**
- Create: `apps/worlds/package.json`, `tsconfig.json`,
  `tsconfig.scripts.json`, `vite.config.ts`, `vitest.config.ts`,
  `biome.json`, `index.html`, `src/app.css`, `src/main.tsx`,
  `src/test-setup.ts`
- Create: `apps/worlds/proof-source.json`,
  `scripts/check-provenance.ts`, `scripts/check-provenance.test.ts`
- Create: `src/theme/theme.ts`, `src/theme/theme-toggle.tsx`,
  `src/theme/theme.test.ts`

**Interfaces:**
- Produces: `bun run check` chain (`check:provenance`, `format:check`,
  `lint`, `typecheck`, `test`, `build`); `ThemeMode = "system" | "light" |
  "dark"`, `resolveTheme(mode, prefersDark): "light" | "dark"`,
  `readThemeMode()`, `writeThemeMode(mode)`, `useThemeMode(): [ThemeMode,
  (m: ThemeMode) => void]`.

- [ ] **Step 1: Scaffold with latest stable deps**

```bash
mkdir -p apps/worlds && cd apps/worlds
bun init -y
bun add react react-dom react-router
bun add -d vite @vitejs/plugin-react typescript @types/react @types/react-dom \
  vitest jsdom @testing-library/react @testing-library/jest-dom \
  @playwright/test @axe-core/playwright @biomejs/biome oxlint @types/bun
bunx playwright install chromium
```

`package.json` scripts:

```json
{
  "dev": "vite",
  "build": "vite build",
  "format:check": "biome check .",
  "lint": "oxlint --deny-warnings .",
  "typecheck": "tsc --noEmit -p tsconfig.json && tsc --noEmit -p tsconfig.scripts.json",
  "test": "vitest run",
  "test:e2e": "playwright test",
  "check:provenance": "bun scripts/check-provenance.ts",
  "check": "bun run check:provenance && bun run format:check && bun run lint && bun run typecheck && bun run test && bun run build && bun run test:e2e"
}
```

Split tsconfigs because `@types/bun` and `vite/client` disagree on
`import.meta`: `tsconfig.json` types `["vitest/globals", "vite/client"]`
including `src` and `tests`; `tsconfig.scripts.json` types
`["bun", "vitest/globals"]` including `scripts`. `vitest.config.ts` sets
`environment: "jsdom"`, `globals: true`,
`setupFiles: ["./src/test-setup.ts"]`,
`include: ["src/**/*.test.{ts,tsx}", "scripts/**/*.test.ts"]`.
`src/test-setup.ts` imports `@testing-library/jest-dom/vitest` and stubs
`window.matchMedia`.

- [ ] **Step 2: Pin the corpus and write the failing provenance test**

```bash
git clone <corpus-remote> ../.scratch/frontend-design-corpus-33c3606
git -C ../.scratch/frontend-design-corpus-33c3606 checkout 33c360659fed449fc849d4ed651bb3a856b76f12
```

`proof-source.json`:

```json
{
  "sourceRoot": "../.scratch/frontend-design-corpus-33c3606",
  "sha": "33c360659fed449fc849d4ed651bb3a856b76f12",
  "files": [
    "corpus/marketing/landing-correspondence.html",
    "corpus/product/chat-correspondence.html",
    "corpus/product/notifications-the-mat.html",
    "corpus/product/settings-the-desk.html",
    "corpus/content/api-reference-correspondence.html"
  ]
}
```

`scripts/check-provenance.test.ts`:

```ts
import { describe, expect, test } from "vitest";
import proofSource from "../proof-source.json";

describe("proof-source manifest", () => {
  test("pins the exact SHA and the five sources", () => {
    expect(proofSource.sha).toBe("33c360659fed449fc849d4ed651bb3a856b76f12");
    expect(proofSource.files).toHaveLength(5);
  });
});
```

- [ ] **Step 3: Implement `scripts/check-provenance.ts`**

Use `Bun.spawn(["git", "-C", sourceRoot, "rev-parse", "HEAD"])`, compare to
`sha`, then `stat` each listed file; exit 1 with a plain sentence on any
mismatch, print `Provenance verified: <sha>` on success.

- [ ] **Step 4: Theme foundation, test-first**

`src/theme/theme.test.ts`:

```ts
import { resolveTheme } from "./theme";

describe("resolveTheme", () => {
  test("system follows the sky", () => {
    expect(resolveTheme("system", true)).toBe("dark");
    expect(resolveTheme("system", false)).toBe("light");
  });
  test("explicit modes win", () => {
    expect(resolveTheme("dark", false)).toBe("dark");
    expect(resolveTheme("light", true)).toBe("light");
  });
});
```

Implement `theme.ts` with `try/catch`-guarded
`globalThis.localStorage?.getItem/setItem("vsh-correspondence-theme", …)`.
`theme-toggle.tsx` exports `useThemeMode()` which applies
`document.documentElement.dataset.theme/themeMode` and tracks
`matchMedia("(prefers-color-scheme: dark)")` changes while in system mode.
`index.html` gets the pre-paint stamp script (same key, sets both dataset
attributes before first paint).

- [ ] **Step 5: Verify and commit**

```bash
bun run check:provenance && bun run format:check && bun run lint && \
  bun run typecheck && bun run test && bun run build
git add apps/worlds && git commit -m "feat: scaffold Correspondence world with provenance and theme gates"
```

---

### Task 2: World tokens, route registry, state, and furniture

**Files:**
- Create: `src/worlds/correspondence/world.css`, `fonts/*.woff2`
- Create: `fidelity-contract.ts`, `world-meta.ts`, `world-meta.test.ts`
- Create: `state/correspondence-state.ts`, `state/correspondence-reducer.ts`,
  `state/correspondence-provider.tsx`, `state/correspondence-reducer.test.ts`
- Create: `furniture/` (14 components, `furniture.module.css`, `index.ts`,
  `furniture.test.tsx`), `test-support.tsx`
- Create: `components/theme-control.tsx`, `components/use-reveal.ts`
- Create: `world-layout.tsx`, `src/router.tsx`

**Interfaces:**
- Consumes: `ThemeMode`, `useThemeMode` from Task 1.
- Produces: `CORRESPONDENCE_ROUTES` (seven entries; fidelity entries carry
  `sourcePath`, `patterns`/`ledger` carry `sourcePath: null`,
  `phase: "elevation"`); `correspondenceReducer`,
  `initialCorrespondenceState`, `CorrespondenceProvider`,
  `useCorrespondence()`; furniture exports `Button, FilterPill, LabelPill,
  MethodStamp, StitchedDivider, Whisper, SwitchControl, SkyCard, NoticeCard,
  CodeLamp, Cm, K, S, E, MessageBubble, WordCard, KeptSeal, FeatCard,
  AttachmentChip, VoicedTable`; `renderInWorld(page)` test helper.

- [ ] **Step 1: World tokens exactly as contracted**

`world.css` `:root` and `html[data-theme="dark"]` blocks copy the palette
table from the approved-result contract verbatim (including
`--sand: #856829` at world level with the recorded rationale comment, the
constant lamplight set, and `--serif-weight` 400/450). `@font-face` for
Fraunces (400–700) and EB Garamond (400–800, normal + italic) from woff2
files extracted out of the corpus pages' base64 (decode with `base64 -d`;
never reference external hosts).

- [ ] **Step 2: Fixed state types and failing reducer tests**

`correspondence-state.ts` copies the type block from the approved-result
contract's "Shared state" section together with the Living Worlds plan's
fixed signatures (`LetterStatus`, `CorrespondenceLetter` with `kept: true`,
`MatNotice`, `CorrespondenceSettings` with `queueUntilBoat: true`,
`CorrespondenceState`, `CorrespondenceAction` — exactly seven action
members). Reducer tests (write first, run, expect failure):

```ts
import {
  correspondenceReducer,
  initialCorrespondenceState,
} from "./correspondence-reducer";

describe("correspondenceReducer", () => {
  test("keeps every draft", () => {
    const state = correspondenceReducer(initialCorrespondenceState, {
      type: "draftChanged", id: "draft-elena-1", body: "The sentence stays.",
    });
    expect(state.letters[0]).toMatchObject({
      id: "draft-elena-1", body: "The sentence stays.",
      status: "draft", kept: true,
    });
  });
  test("queues sealed letters for a named boat", () => {
    const state = correspondenceReducer(initialCorrespondenceState, {
      type: "letterSealed", id: "draft-elena-1", departure: "16:00",
    });
    expect(state.letters[0]).toMatchObject({
      status: "queued", departure: "16:00", kept: true,
    });
  });
  test("moves only the selected boat and writes one Mat notice", () => {
    const state = correspondenceReducer(initialCorrespondenceState, {
      type: "boatCrossed", departure: "16:00",
    });
    expect(
      state.letters.filter((l) => l.status === "crossed"),
    ).toHaveLength(1);
    expect(state.notices.filter((n) => n.kind === "boat")).toHaveLength(1);
  });
  test("never exposes an action that disables kept or boat batching", () => {
    const actionTypes = [
      "draftChanged", "letterSealed", "boatCrossed", "noticeRead",
      "noticesSwept", "bellChanged", "quietBeforeEightChanged",
    ];
    expect(actionTypes).not.toContain("keptChanged");
    expect(actionTypes).not.toContain("queueUntilBoatChanged");
  });
});
```

- [ ] **Step 3: Implement the pure reducer and provider**

Initial state: `draft-elena-1` (a draft to Tomaz Aleixo, index 0), one
letter queued for `16:00`, one queued for `09:00`; empty notices; settings
`{ theme: "system", bellForNewLetters: true, quietBeforeEight: true,
queueUntilBoat: true }`. `boatCrossed` crosses only the named boat's queued
letters, appends exactly one boat notice reading "The nine/four o'clock boat
crossed; its letters are with their readers.", and appends one letter notice
("A letter from Tomaz came in with the boat.") only when
`bellForNewLetters`. `noticesSwept` marks read; nothing ever deletes.
Provider wraps `useReducer`; `useCorrespondence()` throws outside the
provider. `world-layout.tsx` renders
`<CorrespondenceProvider><InstrumentTriggers /><Outlet /></CorrespondenceProvider>`
(InstrumentTriggers arrives in Task 5; stub as fragment until then).

- [ ] **Step 4: Furniture, test-first, per the contract's fourteen pieces**

One `furniture.module.css` cabinet consuming world tokens with light-canon
fallbacks. Components and canonical registers (all from the contract):
`Button` (ember = lapis ground, paper text, dark text `#14161a`; quiet =
panel + line; bare = underlined lapis word), `FilterPill` (aria-pressed,
`on` = lapis-tint), `LabelPill` (mono), `MethodStamp` (renders the method
uppercased; get/post/delete = green/lapis/madder wax), `StitchedDivider`
(mono caption between hairlines), `Whisper` (fixed bottom-center pill,
green ✔ mark + italic text, `held` prop pins it in flow),
`SwitchControl` (role=switch, 42×24, lapis when checked, optional
`behavior` marker), `SkyCard` (role=radio card, lapis border when chosen,
optional `behavior`), `NoticeCard` (glyph + line + italic quote + when +
unread dot; `raised` = sand register), `CodeLamp` (`pre` with
`tabIndex={0}`, `role="region"`, `aria-label`; registers `Cm/K/S/E` =
dim/blue/green/ember; `min-width: 0; max-width: 100%` so it stays a scroll
container as a flex or grid item), `MessageBubble` (tomaz left on cream
`2px 14px 14px 14px`, elena right on lapis `14px 2px 14px 14px`, sr-only
sender name, green tick), `WordCard` (italic specimen, gloss, uppercase
status line), `KeptSeal` (green ✔ + text), `FeatCard` (landing canon:
panel, line border, `padding: 30px 30px 26px`, block svg, Fraunces h3 21px
weight 550 `margin-top: 18px`, p 16px ink-2 `margin-top: 10px`, `em` ink —
top-margins only, no bottom-margin resets), `AttachmentChip` (ruled leaf +
name + mono provenance), `VoicedTable` (mono uppercase heads, voiced cells).

`furniture.test.tsx` asserts every component and every variant: the three
button registers and their roles, pressed state on FilterPill, all three
stamps (uppercase text), divider caption, Whisper silent/heard, switch both
states, radio both states, notice plain + raised, lamp focusable with note,
both correspondents named, word card, seal, feat heading level 3,
attachment provenance, table role + label. `test-support.tsx` exports
`renderInWorld` (MemoryRouter + CorrespondenceProvider).

- [ ] **Step 5: Route registry and router**

`fidelity-contract.ts` defines `CorrespondenceRouteId`
(`"landing" | "thread" | "mat" | "desk" | "api" | "patterns" | "ledger"`)
and `CorrespondenceRouteMeta { id; path; title; sourcePath: string | null;
phase: "fidelity" | "elevation" }`. `world-meta.ts` lists the seven routes
with the contract's paths and titles. `world-meta.test.ts`: unique
ids/paths; every fidelity route's `sourcePath` is in the provenance
manifest; elevation routes have `sourcePath: null`; exactly five fidelity
rooms. `src/router.tsx`: `/` redirects to `/houses/correspondence`; world
layout children index/thread/mat/desk/api/patterns/ledger; `/atlas`; `*` →
"No page keeps this address." with a link to the Atlas.

- [ ] **Step 6: Verify and commit**

```bash
bun run test && bun run typecheck && bun run build
git add apps/worlds && git commit -m "feat: Correspondence tokens, state, and furniture"
```

---

### Task 3: The five fidelity rooms, translated from the pinned sources

**Files:**
- Create: `routes/landing/landing-page.{tsx,module.css,test.tsx}`
- Create: `routes/thread/thread-page.{tsx,module.css,test.tsx}`
- Create: `routes/mat/mat-page.{tsx,module.css,test.tsx}`
- Create: `routes/desk/desk-page.{tsx,module.css,test.tsx}`
- Create: `routes/api/api-page.{tsx,module.css,test.tsx}`

**Interfaces:**
- Consumes: furniture, `useThemeMode`, `useCorrespondence`, `useReveal`.
- Produces: five routes whose accessible structure the e2e matrix (Task 6)
  and the elevation journeys (Task 7) assert.

For each room, in order (landing → thread → mat → desk → api):

- [ ] **Step 1: Read the pinned source in full** (make a base64-elided
  readable copy first: `perl -pe 's/base64,[A-Za-z0-9+\/=]+/base64,ELIDED/g'`).
  List its subject, silhouette, palette, copy, and page-specific chrome.

- [ ] **Step 2: Write the room's failing unit test.** Assert the accessible
  skeleton with real copy from the source, e.g. for the landing:

```tsx
renderInWorld(<LandingPage />);
expect(
  screen.getByRole("heading", { level: 1, name: /write to someone,\s*not at everyone\./i }),
).toBeInTheDocument();
expect(screen.getByRole("heading", { name: "The boat" })).toBeInTheDocument();
expect(screen.getByRole("heading", { name: "The bell" })).toBeInTheDocument();
expect(
  screen.getByRole("heading", { name: "The desk", level: 3 }),
).toBeInTheDocument();
expect(
  screen.getByRole("link", { name: /open the thread/i }),
).toHaveAttribute("href", "/houses/correspondence/thread");
```

and equivalents per room: thread — `heading "Tomaz Aleixo"`, word card
"sarremanso", attachment name, `textbox "Message"`, the manners line; mat —
sweep button with waiting count, four filter pills, `notice-kind-*`
test ids, the bare state when filtered empty; desk — `radiogroup
"Lighting"`, five labeled switches minus the kept row (which asserts the
absence of a switch and the presence of the seal), quiet-save whisper text;
api — sidebar `navigation "Reference"`, endpoint headings whose accessible
names include the space ("POST /v1/letters" — put `{" "}` between stamp and
path), refusal text `THE_DESK_KEEPS`, errors table.

- [ ] **Step 3: Translate the room at fidelity.** Authored JSX + CSS Module
  with page-scoped custom properties refining the world tokens; consume
  furniture where the contract lists it (landing feats via `FeatCard` with
  the trinity The boat · The bell · The desk and the creed sentence verbatim
  on the third card; thread word card via `WordCard` and attachment via
  `AttachmentChip`; mat filters via `FilterPill`; desk switches/sky/whisper
  via `SwitchControl`/`SkyCard`/`Whisper` with the sky cards genuinely
  driving `useThemeMode`; api stamps via `MethodStamp`). Apply the
  contract's contrast corrections and the creed verbatim in the desk's kept
  row and the API's refusal card. Room-specific constraints: the thread is
  a `100dvh` flex room with internal scroll (`<main tabIndex={0}
  aria-label="The thread">`), a re-authored mobile header (identity row
  with ellipsized meta; pill + theme control wrap to a second row at
  ≤560px), and the composer's ember reads "Seal for four" with accessible
  name "Seal for the four o'clock boat" (wired in Task 7 — render the
  button now, dispatch later); every room's footer takes
  `padding-bottom: 64px` at ≤700px (thread: composer `56px`) so the
  instrument pills never sit on the last line.

- [ ] **Step 4: Run the room's test, then the whole unit suite.** Fix until
  green.

- [ ] **Step 5: Commit per room**, e.g.
  `git commit -m "feat: translate Correspondence landing at fidelity"`.

---

### Task 4: The Pattern Book

**Files:**
- Create: `routes/patterns/patterns-page.{tsx,module.css,test.tsx}`

**Interfaces:**
- Consumes: all furniture exports, `ThemeControl`.
- Produces: `/houses/correspondence/patterns` with fourteen plates the e2e
  matrix captures.

- [ ] **Step 1: Failing test** — h1 "The Pattern Book", `navigation
  "Pattern book"`, all fourteen plate headings at level 2 (Buttons, Pills &
  stamps, Stitched dividers, Whispers, Switches, Sky cards, Notice cards,
  Code lamps, Message bubbles, Word cards, Kept seals, Feat cards,
  Attachment leaves, Voiced tables), furniture at work (sarremanso,
  THE_DESK_KEEPS, checked Day radio, ≥2 switches).

- [ ] **Step 2: Build the book.** Documentary silhouette: sticky rail of
  plates + main column; each plate = Fraunces h2, a voice paragraph in the
  house's register, a specimen board (each register labeled in mono), and
  an in-context vignette on a dashed fence. Whisper specimens use `held`.
  Mobile: the grid column must be `minmax(0, 1fr)` so lamps stay scroll
  containers. Footer carries the instrument clearance block.

- [ ] **Step 3: Green, then commit**
  `git commit -m "feat: The Pattern Book — the house's furniture catalogued in context"`.

---

### Task 5: Atlas and system lens

**Files:**
- Create: `src/atlas/atlas-dialog.tsx` (+ module css) — `AtlasDialog` and
  `AtlasIndex`
- Create: `src/lens/system-metadata.ts`, `src/lens/system-lens.tsx`
  (+ module css)
- Create: `components/instrument-triggers.tsx` (replace the Task 2 stub)

**Interfaces:**
- Consumes: `CORRESPONDENCE_ROUTES`.
- Produces: `button "Open Atlas"` / `dialog "Atlas"` with route links by
  title; `button "System lens"` / `complementary "System lens"` with
  foundations/behavior/source layers; both leave zero DOM artifacts closed.

- [ ] **Step 1: Failing e2e** (`tests/e2e/instruments.spec.ts`): Atlas opens
  without moving the h1's bounding box, closes on Escape returning focus to
  the trigger; ⌘K is ignored inside the thread composer; the lens defaults
  off, each layer button reveals list items, and after "Close lens" a DOM
  query for `[class*='drawer'], [data-lens]` counts zero.

- [ ] **Step 2: Implement.** Fixed pill triggers bottom-left
  (`data-system-behavior="atlas"` / `"lens"` — the capture tooling and the
  lens both key on these). Atlas: manual focus trap, Escape, backdrop
  click, `isEditable` guard on the shortcut, route list from the registry
  plus "Studies · not yet admitted". Lens `source` layer marks the five
  sourced rooms with their corpus paths and everything else as authored.

- [ ] **Step 3: Green, then commit**
  `git commit -m "feat: disappearing Atlas and system lens"`.

---

### Task 6: The deterministic review matrix

**Files:**
- Create: `tests/support/routes.ts`, `tests/support/review-page.ts`
- Create: `tests/e2e/fidelity.spec.ts`, `tests/e2e/accessibility.spec.ts`
- Create: `playwright.config.ts`

**Interfaces:**
- Consumes: all seven routes.
- Produces: 28 deterministic baselines and the gates every later change
  must hold.

- [ ] **Step 1: Support files.** `routes.ts`: `FIDELITY_ROUTES` (five),
  `PATTERN_ROUTES` (patterns, ledger — ledger arrives in Task 7; add its
  entry then), `VIEWPORTS` 1440×900 / 390×844, `THEMES` light/dark.
  `review-page.ts`: `prepareReviewPage` seeds the theme key only when
  absent, freezes `Date.now`, emulates reduced motion + color scheme;
  `settlePage` awaits fonts + network idle; `collectFailures` gathers
  console errors, page errors, failed requests, HTTP ≥ 400, and any
  non-`127.0.0.1` host. `playwright.config.ts`: fixed port, chromium only,
  `outputDir ./review/test-results`, timezone `America/Sao_Paulo`,
  webServer `bun run dev -- --host 127.0.0.1 --port <port>`.

- [ ] **Step 2: The matrix spec.** For every route × theme × viewport:
  assert overflow ≤ 0; run axe and assert no serious/critical violations;
  assert the failure collector is empty; then, capture-only, inject styles
  hiding the instrument trigger buttons, setting fixed paper washes to
  `background-attachment: scroll`, and flattening the thread's chat
  viewport — and take the full-page screenshot. Accessibility spec: theme
  persists across navigation; the desk's night sky darkens the whole world;
  deep links land; not-found offers the Atlas.

- [ ] **Step 3: Generate baselines, prove determinism.** Run
  `bunx playwright test --update-snapshots`, then the plain suite **twice**;
  all green both times. Read every capture at full height against its
  pinned source and write `review/fidelity/report.md` with per-route
  verdicts (Source read / Fidelity verdict / Preserved / Lost or weakened /
  Responsive verdict / Dark verdict); automated scores are gates, not
  visual evidence.

- [ ] **Step 4: Commit**
  `git commit -m "test: complete Correspondence fidelity matrix"`.

---

### Task 7: Elevation — live state across rooms and The Ledger Between Boats

**Files:**
- Create: `routes/ledger/ledger-page.{tsx,module.css,test.tsx}`
- Modify: `routes/thread/thread-page.tsx` (composer dispatches),
  `routes/mat/mat-page.tsx` (TODAY group, shared sweep),
  `routes/desk/desk-page.tsx` (bell + quiet write shared settings),
  `routes/api/api-page.tsx` (sandbox), `tests/support/routes.ts` (ledger)
- Create: `tests/e2e/elevation.spec.ts`

**Interfaces:**
- Consumes: `useCorrespondence()`, furniture, the Atlas (for state-keeping
  navigation in tests).
- Produces: the approved cross-route reality and
  `/houses/correspondence/ledger`.

- [ ] **Step 1: Failing elevation e2e.** Two journeys, navigating between
  rooms **through the Atlas** (provider state is in-memory; a full reload
  would reset it):

```ts
test("a kept letter moves through the Correspondence world", async ({ page }) => {
  await prepareReviewPage(page, "light");
  await page.goto("/houses/correspondence/thread");
  await settlePage(page);
  await page.getByRole("textbox", { name: "Message" })
    .fill("The sentence stays between boats.");
  await page.getByRole("button", { name: "Seal for the four o'clock boat" }).click();
  await travelViaAtlas(page, "The Ledger Between Boats");
  const sealed = page.locator("article", { hasText: "The sentence stays between boats." });
  await expect(sealed).toContainText("16:00");
  await expect(sealed).toContainText("kept");
  await page.getByRole("button", { name: "Advance the four o'clock boat" }).click();
  await expect(sealed).toContainText("crossed");
  await travelViaAtlas(page, "The Mat");
  await expect(page.getByText(/the four o'clock boat crossed/i)).toBeVisible();
});
```

and the bell journey: desk → switch "Ring for new letters" off → advance
the nine o'clock boat at the ledger → the Mat shows the boat notice and
zero letter notices.

- [ ] **Step 2: Connect the rooms.** Thread: the composer is the kept draft
  (`value` from the draft letter, `draftChanged` on change); the ember
  dispatches `letterSealed` for `16:00`; it never says send now. Mat: a
  "TODAY · THE BOAT WENT OUT" group renders shared notices in the mat's own
  row idiom when present, honors the filters, counts unread into the sweep
  button, `noticeRead` on click, `noticesSwept` on sweep. Desk: the ring
  and quiet rows read and write shared settings; every other switch stays
  desk-local; the kept row still has no control. API: one quiet button
  "Post this letter from your desk" under the POST card dispatches
  `draftChanged` + `letterSealed` for a sandbox letter and reveals the
  deterministic 202 lamp (`status: "queued"`, `departure: "16:00"`,
  `kept: true`) with a manners note; the reference text is untouched.

- [ ] **Step 3: Author The Ledger Between Boats** per the contract's Ledger
  section: single quiet thread, two sand crossings, "The day between" with
  the protected-time sentence, letter cards by status (dashed cream draft /
  lapis queued with the literal departure in the manners line / green
  crossed), review fence with the two advance buttons. Ledger unit test:
  the three level-2 headings; "kept as draft", "kept for four", "kept for
  nine" all visible; advancing four crosses only four; no countdown or
  destructive control (query `/remaining|counts? down|until departure/i`
  and `/delete|discard|remove/i` buttons — both absent). Register the route
  in `world-meta.ts` and `PATTERN_ROUTES`.

- [ ] **Step 4: Regenerate only the moved baselines** (thread, api, ledger
  are expected to move; landing, mat, desk, patterns must not — treat any
  other diff as a regression). Read the new frames at full height. Run the
  whole suite twice. Append the three-state comparison to
  `review/elevation/report.md` (Original belief preserved / Fidelity proof
  preserved / Elevation added / Why it belongs only to Correspondence /
  External trend language / Verdict, per route; the Ledger records the
  beliefs it extends).

- [ ] **Step 5: Commit**

```bash
git add apps/worlds
git commit -m "feat: elevate Correspondence between boats"
```

---

### Task 8: Final gate and operator review

- [ ] **Step 1: Run the complete gate**

```bash
cd apps/worlds && bun run check
```

Expected: provenance verified; format, lint, both tsconfigs clean; all unit
tests green; build clean; all e2e green with 28 baselines, twice.

- [ ] **Step 2: Present the live world** at the dev server and walk the
  approved journey (landing → thread seal → ledger → advance → mat → desk
  bell → api sandbox → Atlas → lens). Ask the operator to confirm the clean
  implementation matches the approved-result contract. On approval:

```bash
git add apps/worlds docs
git commit -m "feat: Correspondence living world, clean implementation"
git push
```

---

## Plan Completion Contract

This plan is complete when the seven routes exist in this repository, every
invariant in the approved-result contract holds, the 28-capture matrix and
all mechanical checks pass deterministically twice, and the operator has
confirmed the clean implementation against the contract.
