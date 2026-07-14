# Correspondence Living World Proof Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Produce a disposable, exact-provenance React proof that first becomes the five-work Correspondence world, then elevates it through approved cross-route continuity and one world-native authored moment, without placing rejected proof code or captures in the target repository.

**Architecture:** Build the proof as a local-only Git repository under `$CLAUDE_JOB_DIR/tmp/correspondence-living-world-proof` with no remote. Five native React routes share only theme, route metadata, and accessible mechanics during the fidelity phase; after operator approval, an explicitly approved elevation phase adds Correspondence state continuity and a Ledger route. The target repository receives only design and planning documents until the elevated proof passes operator review, at which point a separate clean-room implementation plan is written.

**Tech Stack:** Bun 1.3.14, React 19.2, React Router 7.13 or newer stable with Vite 8 support, Vite 8.1, strict TypeScript, CSS Modules, Vitest, Testing Library, Playwright, axe-core, Biome 2, oxlint.

**Authoritative specification:** `docs/superpowers/specs/2026-07-14-living-worlds-design.md`

**Binding law:** `docs/design-laws.md` — this plan yields to the specification, and both yield to the laws.

## Global Constraints

- Source provenance is always `33c360659fed449fc849d4ed651bb3a856b76f12`; never read candidate content from `main`, `HEAD`, or an unpinned URL.
- Proof source lives only under `$CLAUDE_JOB_DIR/tmp/correspondence-living-world-proof`; it must not be copied, committed, or merged into `frontend-design-system`.
- The proof Git repository has no remote. All commits are local disposable checkpoints.
- The main session owns composition, prose, typography, palette, responsive art direction, and visual approval. Workers may perform only mechanical setup, extraction, tests, captures, and review.
- Workers never commit or push. Every commit step in this plan is executed by the parent session only.
- Gate 1 must pass before any elevation code is written.
- Gate 2 must pass before any product implementation is written in the target repository.
- Rejected proof code, captures, snapshots, and translations must not enter target Git objects, branches, packages, registry output, or deployment.
- Correspondence remains one product world: boat, bell, mat, desk, ledger, letters, and `kept` retain consistent meaning across every route and state.
- Content is structural material. Do not substitute neutral placeholder copy.
- No gallery hero, card-grid browser, screenshot plates, adaptive gallery shell, or persistent frame.
- Atlas and system lens are summonable instruments and disappear completely when closed.
- Stable-looking shared machinery must remain visually subordinate to authored composition.
- Dark surfaces use neutral charcoal and muted moonlight accents; no brown or umber walls, electric blue or green, or saturation walls.
- No Tailwind inside the proof's authored Correspondence surfaces.
- Body copy has a 16px minimum unless an existing metadata role is deliberately smaller and remains readable.
- Reduced motion, keyboard access, focus restoration, direct deep links, and 390×844 mobile behavior are first-class.
- Lint completes with zero errors and zero warnings.
- Automated checks cannot approve taste; full-height operator review is the gate.

---

## File Structure

The proof repository is disposable and uses this structure:

```text
$CLAUDE_JOB_DIR/tmp/correspondence-living-world-proof/
├── .git/
├── index.html
├── package.json
├── bun.lock
├── tsconfig.json
├── vite.config.ts
├── vitest.config.ts
├── playwright.config.ts
├── proof-source.json
├── scripts/
│   └── check-provenance.ts
├── src/
│   ├── main.tsx
│   ├── router.tsx
│   ├── app.css
│   ├── test-setup.ts
│   ├── theme/
│   │   ├── theme.ts
│   │   ├── theme.test.ts
│   │   └── theme-toggle.tsx
│   ├── atlas/
│   │   ├── atlas-dialog.tsx
│   │   ├── atlas-dialog.module.css
│   │   └── atlas-dialog.test.tsx
│   ├── lens/
│   │   ├── system-lens.tsx
│   │   ├── system-lens.module.css
│   │   ├── system-lens.test.tsx
│   │   └── system-metadata.ts
│   └── worlds/correspondence/
│       ├── world-meta.ts
│       ├── world-layout.tsx
│       ├── world-layout.module.css
│       ├── fixtures.ts
│       ├── fidelity-contract.ts
│       ├── components/
│       │   ├── world-nav.tsx
│       │   ├── world-nav.module.css
│       │   ├── theme-control.tsx
│       │   └── instrument-triggers.tsx
│       ├── routes/
│       │   ├── landing/
│       │   │   ├── landing-page.tsx
│       │   │   ├── landing-page.module.css
│       │   │   └── landing-page.test.tsx
│       │   ├── thread/
│       │   │   ├── thread-page.tsx
│       │   │   ├── thread-page.module.css
│       │   │   └── thread-page.test.tsx
│       │   ├── mat/
│       │   │   ├── mat-page.tsx
│       │   │   ├── mat-page.module.css
│       │   │   └── mat-page.test.tsx
│       │   ├── desk/
│       │   │   ├── desk-page.tsx
│       │   │   ├── desk-page.module.css
│       │   │   └── desk-page.test.tsx
│       │   ├── api/
│       │   │   ├── api-page.tsx
│       │   │   ├── api-page.module.css
│       │   │   └── api-page.test.tsx
│       │   └── ledger/
│       │       ├── ledger-page.tsx
│       │       ├── ledger-page.module.css
│       │       └── ledger-page.test.tsx
│       └── state/
│           ├── correspondence-state.ts
│           ├── correspondence-reducer.ts
│           ├── correspondence-provider.tsx
│           └── correspondence-reducer.test.ts
├── tests/
│   ├── e2e/
│   │   ├── fidelity.spec.ts
│   │   ├── instruments.spec.ts
│   │   ├── accessibility.spec.ts
│   │   └── elevation.spec.ts
│   └── support/
│       ├── routes.ts
│       └── review-page.ts
├── review/
│   ├── fidelity/
│   │   └── report.md
│   └── elevation/
│       ├── ledger-study.html
│       └── report.md
└── elevation-brief.md
```

The target repository changes only after Gate 2:

```text
frontend-design-system/
└── docs/superpowers/
    ├── specs/
    │   └── 2026-07-14-correspondence-approved-result.md
    └── plans/
        └── 2026-07-14-correspondence-clean-implementation.md
```

## Shared Interfaces

These signatures are fixed for the proof. Tasks must not rename them independently.

```ts
export type ThemeMode = "system" | "light" | "dark";
export type ResolvedTheme = "light" | "dark";

export interface ThemeState {
  mode: ThemeMode;
  resolved: ResolvedTheme;
}

export interface ProofSource {
  repository: "victor-software-house/frontend-design-corpus";
  commit: "33c360659fed449fc849d4ed651bb3a856b76f12";
  files: readonly string[];
}

export type CorrespondenceRouteId =
  | "landing"
  | "thread"
  | "mat"
  | "desk"
  | "api"
  | "ledger";

export interface CorrespondenceRouteMeta {
  id: CorrespondenceRouteId;
  path: string;
  title: string;
  sourcePath: string | null;
  phase: "fidelity" | "elevation";
}

export type SystemLayer = "off" | "foundations" | "behavior" | "source";

export interface SystemAnnotation {
  id: string;
  layer: Exclude<SystemLayer, "off">;
  label: string;
  ownership: "foundation" | "behavior" | "registry" | "authored";
  source: string;
}
```

Elevation adds these fixed state signatures only after Gate 1:

```ts
export type LetterStatus = "draft" | "queued" | "crossed";

export interface CorrespondenceLetter {
  id: string;
  to: string;
  body: string;
  status: LetterStatus;
  departure: "09:00" | "16:00" | null;
  kept: true;
}

export interface MatNotice {
  id: string;
  kind: "letter" | "boat" | "desk";
  text: string;
  read: boolean;
}

export interface CorrespondenceSettings {
  theme: ThemeMode;
  bellForNewLetters: boolean;
  quietBeforeEight: boolean;
  queueUntilBoat: true;
}

export interface CorrespondenceState {
  letters: readonly CorrespondenceLetter[];
  notices: readonly MatNotice[];
  settings: CorrespondenceSettings;
}

export type CorrespondenceAction =
  | { type: "draftChanged"; id: string; body: string }
  | { type: "letterSealed"; id: string; departure: "09:00" | "16:00" }
  | { type: "boatCrossed"; departure: "09:00" | "16:00" }
  | { type: "noticeRead"; id: string }
  | { type: "noticesSwept" }
  | { type: "bellChanged"; enabled: boolean }
  | { type: "quietBeforeEightChanged"; enabled: boolean };
```

---

### Task 1: Establish the Disposable Proof and Provenance Gate

**Files:**
- Create: `$CLAUDE_JOB_DIR/tmp/frontend-design-corpus-33c3606/`
- Create: `$CLAUDE_JOB_DIR/tmp/correspondence-living-world-proof/package.json`
- Create: `$CLAUDE_JOB_DIR/tmp/correspondence-living-world-proof/tsconfig.json`
- Create: `$CLAUDE_JOB_DIR/tmp/correspondence-living-world-proof/vite.config.ts`
- Create: `$CLAUDE_JOB_DIR/tmp/correspondence-living-world-proof/vitest.config.ts`
- Create: `$CLAUDE_JOB_DIR/tmp/correspondence-living-world-proof/src/test-setup.ts`
- Create: `$CLAUDE_JOB_DIR/tmp/correspondence-living-world-proof/proof-source.json`
- Create: `$CLAUDE_JOB_DIR/tmp/correspondence-living-world-proof/scripts/check-provenance.ts`
- Create: `$CLAUDE_JOB_DIR/tmp/correspondence-living-world-proof/scripts/check-provenance.test.ts`

**Interfaces:**
- Consumes: exact source repository and commit from Global Constraints.
- Produces: a local-only proof repository, `ProofSource`, and `checkProvenance(sourceRoot: string, manifest: ProofSource): Promise<void>`.

- [ ] **Step 1: Resolve the exact source checkout**

Run:

```bash
SOURCE="$CLAUDE_JOB_DIR/tmp/frontend-design-corpus-33c3606"
if test -d "$SOURCE/.git"; then
  test "$(git -C "$SOURCE" remote get-url origin)" = \
    "git@github.com:victor-software-house/frontend-design-corpus.git"
else
  git clone --filter=blob:none --no-checkout \
    git@github.com:victor-software-house/frontend-design-corpus.git \
    "$SOURCE"
fi
git -C "$SOURCE" fetch origin 33c360659fed449fc849d4ed651bb3a856b76f12
git -C "$SOURCE" checkout --detach 33c360659fed449fc849d4ed651bb3a856b76f12
test "$(git -C "$SOURCE" rev-parse HEAD)" = \
  "33c360659fed449fc849d4ed651bb3a856b76f12"
```

Expected: detached HEAD at `33c360659fed449fc849d4ed651bb3a856b76f12`.

- [ ] **Step 2: Initialize a proof repository with no remote**

Run:

```bash
mkdir -p "$CLAUDE_JOB_DIR/tmp/correspondence-living-world-proof"
git -C "$CLAUDE_JOB_DIR/tmp/correspondence-living-world-proof" init -b main
test -z "$(git -C "$CLAUDE_JOB_DIR/tmp/correspondence-living-world-proof" remote)"
```

Expected: local `main` branch and no remote names.

- [ ] **Step 3: Create the package manifest and install pinned ranges**

Create `package.json`:

```json
{
  "name": "correspondence-living-world-proof",
  "private": true,
  "type": "module",
  "packageManager": "bun@1.3.14",
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "format:check": "biome check .",
    "lint": "oxlint --deny-warnings .",
    "typecheck": "tsc --noEmit",
    "test": "vitest run",
    "test:e2e": "playwright test",
    "check:provenance": "bun scripts/check-provenance.ts",
    "check": "bun run format:check && bun run lint && bun run typecheck && bun run test && bun run build"
  }
}
```

Run:

```bash
cd "$CLAUDE_JOB_DIR/tmp/correspondence-living-world-proof" && \
  bun add react@19.2 react-dom@19.2 react-router@^7.13 && \
  bun add -d vite@8.1 @vitejs/plugin-react typescript @types/bun \
    vitest jsdom @testing-library/react @testing-library/user-event \
    @testing-library/jest-dom @playwright/test @axe-core/playwright \
    @biomejs/biome@^2 oxlint
```

Create `vitest.config.ts`:

```ts
import { defineConfig } from "vitest/config";

export default defineConfig({
  test: {
    environment: "jsdom",
    setupFiles: ["./src/test-setup.ts"],
  },
});
```

Create `src/test-setup.ts`:

```ts
import "@testing-library/jest-dom/vitest";
```

Create `tsconfig.json`:

```json
{
  "compilerOptions": {
    "target": "ES2022",
    "lib": ["ES2022", "DOM", "DOM.Iterable"],
    "module": "ESNext",
    "moduleResolution": "Bundler",
    "resolveJsonModule": true,
    "jsx": "react-jsx",
    "strict": true,
    "noUncheckedIndexedAccess": true,
    "noEmit": true,
    "types": ["bun", "vitest/globals", "@testing-library/jest-dom"]
  },
  "include": ["src", "scripts", "tests", "vite.config.ts", "vitest.config.ts", "playwright.config.ts"]
}
```

Create `vite.config.ts`:

```ts
import react from "@vitejs/plugin-react";
import { defineConfig } from "vite";

export default defineConfig({
  plugins: [react()],
});
```

Expected: `bun.lock` created, `bun pm ls` exits zero, and Vitest loads DOM matchers.

- [ ] **Step 4: Write the failing provenance test**

Create `scripts/check-provenance.test.ts`:

```ts
import { describe, expect, test } from "vitest";
import { checkProvenance } from "./check-provenance";

const manifest = {
  repository: "victor-software-house/frontend-design-corpus",
  commit: "33c360659fed449fc849d4ed651bb3a856b76f12",
  files: [
    "corpus/marketing/landing-correspondence.html",
    "corpus/product/chat-correspondence.html",
    "corpus/product/notifications-the-mat.html",
    "corpus/product/settings-the-desk.html",
    "corpus/content/api-reference-correspondence.html"
  ]
} as const;

describe("checkProvenance", () => {
  test("rejects a checkout at the wrong commit", async () => {
    await expect(checkProvenance("/missing", manifest)).rejects.toThrow();
  });
});
```

Run:

```bash
bunx vitest run scripts/check-provenance.test.ts
```

Expected: FAIL because `checkProvenance` does not exist.

- [ ] **Step 5: Implement exact provenance validation**

Create `proof-source.json` with the same repository, commit, and five source paths shown above.

Create `scripts/check-provenance.ts`:

```ts
import { access, readFile } from "node:fs/promises";
import { join } from "node:path";

export interface ProofSource {
  repository: "victor-software-house/frontend-design-corpus";
  commit: "33c360659fed449fc849d4ed651bb3a856b76f12";
  files: readonly string[];
}

export async function checkProvenance(
  sourceRoot: string,
  manifest: ProofSource,
): Promise<void> {
  const process = Bun.spawn(["git", "-C", sourceRoot, "rev-parse", "HEAD"], {
    stdout: "pipe",
    stderr: "pipe",
  });
  const sha = (await new Response(process.stdout).text()).trim();
  const exitCode = await process.exited;

  if (exitCode !== 0 || sha !== manifest.commit) {
    throw new Error(`Expected ${manifest.commit}, received ${sha || "no checkout"}.`);
  }

  for (const relativePath of manifest.files) {
    await access(join(sourceRoot, relativePath));
  }
}

if (import.meta.main) {
  const sourceRoot = join(
    process.env.CLAUDE_JOB_DIR ?? "",
    "tmp/frontend-design-corpus-33c3606",
  );
  const manifest = JSON.parse(
    await readFile(new URL("../proof-source.json", import.meta.url), "utf8"),
  ) as ProofSource;
  await checkProvenance(sourceRoot, manifest);
  console.log(`Provenance verified: ${manifest.commit}`);
}
```

- [ ] **Step 6: Run the provenance and baseline checks**

Run:

```bash
bunx vitest run scripts/check-provenance.test.ts
bun run check:provenance
test -z "$(git remote)"
```

Expected:

```text
1 pass
Provenance verified: 33c360659fed449fc849d4ed651bb3a856b76f12
```

- [ ] **Step 7: Parent-only local commit**

```bash
git add .
git commit -m "chore: establish disposable proof provenance"
```

Expected: one local commit and no remote.

---

### Task 2: Build Theme, Route, and Test Foundations Without a Gallery Shell

**Files:**
- Create: `index.html`
- Create: `src/main.tsx`
- Create: `src/router.tsx`
- Create: `src/app.css`
- Create: `src/theme/theme.ts`
- Create: `src/theme/theme.test.ts`
- Create: `src/theme/theme-toggle.tsx`
- Create: `src/worlds/correspondence/world-meta.ts`
- Create: `src/worlds/correspondence/world-meta.test.ts`
- Create: `src/worlds/correspondence/world-layout.tsx`
- Create: `src/worlds/correspondence/world-layout.module.css`
- Create: `src/worlds/correspondence/components/world-nav.tsx`
- Create: `src/worlds/correspondence/components/world-nav.module.css`
- Create: `src/worlds/correspondence/components/theme-control.tsx`
- Create: `src/worlds/correspondence/components/instrument-triggers.tsx`

**Interfaces:**
- Consumes: `ThemeMode`, `ResolvedTheme`, `CorrespondenceRouteMeta` from Shared Interfaces.
- Produces: `resolveTheme(mode, prefersDark)`, `readThemeMode()`, `writeThemeMode(mode)`, route metadata, and a layout that adds no background, panel, or spacing around route content.

- [ ] **Step 1: Write failing theme tests**

Create `src/theme/theme.test.ts`:

```ts
import { describe, expect, test } from "vitest";
import { resolveTheme } from "./theme";

describe("resolveTheme", () => {
  test.each([
    ["light", false, "light"],
    ["light", true, "light"],
    ["dark", false, "dark"],
    ["dark", true, "dark"],
    ["system", false, "light"],
    ["system", true, "dark"],
  ] as const)("resolves %s with prefersDark=%s", (mode, prefersDark, expected) => {
    expect(resolveTheme(mode, prefersDark)).toBe(expected);
  });
});
```

Run:

```bash
bunx vitest run src/theme/theme.test.ts
```

Expected: FAIL because `resolveTheme` does not exist.

- [ ] **Step 2: Implement the minimal theme contract**

Create `src/theme/theme.ts`:

```ts
export type ThemeMode = "system" | "light" | "dark";
export type ResolvedTheme = "light" | "dark";

export const THEME_STORAGE_KEY = "vsh-correspondence-theme";

export function resolveTheme(
  mode: ThemeMode,
  prefersDark: boolean,
): ResolvedTheme {
  return mode === "system" ? (prefersDark ? "dark" : "light") : mode;
}

export function readThemeMode(): ThemeMode {
  const value = localStorage.getItem(THEME_STORAGE_KEY);
  return value === "light" || value === "dark" || value === "system"
    ? value
    : "system";
}

export function writeThemeMode(mode: ThemeMode): void {
  localStorage.setItem(THEME_STORAGE_KEY, mode);
}
```

Place this script before the module entry in `index.html` so theme is stamped before React and CSS render:

```html
<script>
  (() => {
    const key = "vsh-correspondence-theme";
    const saved = localStorage.getItem(key);
    const mode = saved === "light" || saved === "dark" || saved === "system"
      ? saved
      : "system";
    const prefersDark = matchMedia("(prefers-color-scheme: dark)").matches;
    document.documentElement.dataset.themeMode = mode;
    document.documentElement.dataset.theme = mode === "system"
      ? (prefersDark ? "dark" : "light")
      : mode;
  })();
</script>
```

- [ ] **Step 3: Define exact fidelity routes**

Create `world-meta.ts`:

```ts
import type { CorrespondenceRouteMeta } from "./fidelity-contract";

export const CORRESPONDENCE_ROUTES = [
  {
    id: "landing",
    path: "/houses/correspondence",
    title: "Correspondence",
    sourcePath: "corpus/marketing/landing-correspondence.html",
    phase: "fidelity",
  },
  {
    id: "thread",
    path: "/houses/correspondence/thread",
    title: "Elena and Tomaz",
    sourcePath: "corpus/product/chat-correspondence.html",
    phase: "fidelity",
  },
  {
    id: "mat",
    path: "/houses/correspondence/mat",
    title: "The Mat",
    sourcePath: "corpus/product/notifications-the-mat.html",
    phase: "fidelity",
  },
  {
    id: "desk",
    path: "/houses/correspondence/desk",
    title: "The Desk",
    sourcePath: "corpus/product/settings-the-desk.html",
    phase: "fidelity",
  },
  {
    id: "api",
    path: "/houses/correspondence/api",
    title: "The Correspondence API",
    sourcePath: "corpus/content/api-reference-correspondence.html",
    phase: "fidelity",
  },
] as const satisfies readonly CorrespondenceRouteMeta[];
```

Create `fidelity-contract.ts` with the shared route types exactly as specified above.

Create `world-meta.test.ts`:

```ts
import { describe, expect, test } from "vitest";
import proofSource from "../../../proof-source.json";
import { CORRESPONDENCE_ROUTES } from "./world-meta";

describe("CORRESPONDENCE_ROUTES", () => {
  test("has unique ids and paths", () => {
    expect(new Set(CORRESPONDENCE_ROUTES.map((route) => route.id)).size).toBe(
      CORRESPONDENCE_ROUTES.length,
    );
    expect(new Set(CORRESPONDENCE_ROUTES.map((route) => route.path)).size).toBe(
      CORRESPONDENCE_ROUTES.length,
    );
  });

  test("maps every fidelity route to an exact manifest source", () => {
    const sources = new Set(proofSource.files);
    for (const route of CORRESPONDENCE_ROUTES) {
      expect(route.phase).toBe("fidelity");
      expect(route.sourcePath).not.toBeNull();
      expect(sources.has(route.sourcePath as string)).toBe(true);
    }
  });
});
```

- [ ] **Step 4: Build the shell as invisible machinery**

`world-layout.tsx` must render:

```tsx
export function CorrespondenceWorldLayout() {
  return (
    <>
      <InstrumentTriggers />
      <Outlet />
    </>
  );
}
```

It must not introduce a wrapper background, max width, card, border, drop shadow, or global content padding. Each route owns its own composition.

`WorldNav` is rendered only by routes whose pinned source already contains navigation. It uses Correspondence nouns and direct route links; it is not a universal gallery navbar.

- [ ] **Step 5: Register routes and direct deep links**

Create `router.tsx` with a `createBrowserRouter` route tree. `/` redirects to `/houses/correspondence`. Unknown routes render a precise text link to `/atlas` without a branded error illustration.

- [ ] **Step 6: Run foundation checks**

Run:

```bash
bunx vitest run src/theme/theme.test.ts src/worlds/correspondence/world-meta.test.ts
bun run typecheck
bun run build
```

Expected: all pass; build emits a deep-link-capable SPA bundle.

- [ ] **Step 7: Parent-only local commit**

```bash
git add index.html src package.json bun.lock tsconfig.json vite.config.ts vitest.config.ts
git commit -m "feat: establish invisible world foundations"
```

---

### Task 3: Translate the Correspondence Landing Page at Fidelity

**Files:**
- Create: `src/worlds/correspondence/fixtures.ts`
- Create: `src/worlds/correspondence/routes/landing/landing-page.tsx`
- Create: `src/worlds/correspondence/routes/landing/landing-page.module.css`
- Create: `src/worlds/correspondence/routes/landing/landing-page.test.tsx`

**Interfaces:**
- Consumes: exact source `corpus/marketing/landing-correspondence.html`, `WorldNav`, `ThemeControl`.
- Produces: `LandingPage`, canonical Correspondence copy fixtures, and a live route whose embedded thread links to the full thread route.

- [ ] **Step 1: Write the semantic fidelity test**

Create `landing-page.test.tsx`:

```tsx
import { render, screen } from "@testing-library/react";
import { MemoryRouter } from "react-router";
import { describe, expect, test } from "vitest";
import { LandingPage } from "./landing-page";

describe("LandingPage", () => {
  test("leads with the operating belief and preserves the three product principles", () => {
    render(<LandingPage />, { wrapper: MemoryRouter });

    expect(
      screen.getByRole("heading", {
        level: 1,
        name: "Write to someone, not at everyone.",
      }),
    ).toBeInTheDocument();
    expect(screen.getByRole("heading", { name: "The boat" })).toBeInTheDocument();
    expect(screen.getByRole("heading", { name: "The bell" })).toBeInTheDocument();
    expect(screen.getByRole("heading", { name: "Kept" })).toBeInTheDocument();
    expect(screen.getByText(/Feeds are weather; letters are climate/)).toBeInTheDocument();
    expect(screen.getByRole("link", { name: /open the thread/i })).toHaveAttribute(
      "href",
      "/houses/correspondence/thread",
    );
  });
});
```

Run:

```bash
bunx vitest run src/worlds/correspondence/routes/landing/landing-page.test.tsx
```

Expected: FAIL because `LandingPage` does not exist.

- [ ] **Step 2: Main session authors the React composition**

Translate the exact source content and hierarchy into semantic React. Preserve:

- the two-line hero proposition
- the live Elena/Tomaz product window
- the creed band as the dominant interruption
- boat, bell, and kept as three distinct principles
- the testimonial as human proof
- three desks as pricing furniture
- the closing boat schedule

Do not introduce reusable cards, bento grids, gradient backgrounds, glass surfaces, screenshot images, or a gallery introduction.

The main session writes the complete CSS Module after comparing the full-height source render and live proof side by side. The sewn visual rhythm—quiet paper, creed rupture, proof, furniture, invitation—must remain legible at both 1440×900 and 390×844.

- [ ] **Step 3: Add explicit system metadata without visible labels**

The route may stamp only genuine shared machinery:

```tsx
<main
  data-system-foundation="correspondence-paper"
  data-system-source="corpus/marketing/landing-correspondence.html"
>
```

Do not annotate the creed band, product window, or pricing composition as generic components.

- [ ] **Step 4: Run route checks**

Run:

```bash
bunx vitest run src/worlds/correspondence/routes/landing/landing-page.test.tsx
bun run typecheck
bun run build
```

Expected: all pass.

- [ ] **Step 5: Parent-only local commit**

```bash
git add src/worlds/correspondence/fixtures.ts src/worlds/correspondence/routes/landing
git commit -m "feat: translate Correspondence landing at fidelity"
```

---

### Task 4: Translate the Elena and Tomaz Thread at Fidelity

**Files:**
- Create: `src/worlds/correspondence/routes/thread/thread-page.tsx`
- Create: `src/worlds/correspondence/routes/thread/thread-page.module.css`
- Create: `src/worlds/correspondence/routes/thread/thread-page.test.tsx`

**Interfaces:**
- Consumes: canonical thread fixtures from `fixtures.ts` and exact source `corpus/product/chat-correspondence.html`.
- Produces: `ThreadPage` with semantic message sequence, word specimen, attachment, receipts, and composer.

- [ ] **Step 1: Write the semantic thread test**

```tsx
import { render, screen } from "@testing-library/react";
import { MemoryRouter } from "react-router";
import { describe, expect, test } from "vitest";
import { ThreadPage } from "./thread-page";

describe("ThreadPage", () => {
  test("preserves the translation problem as the thread's organizing subject", () => {
    render(<ThreadPage />, { wrapper: MemoryRouter });

    expect(screen.getByRole("heading", { name: "Tomaz Aleixo" })).toBeInTheDocument();
    expect(screen.getByText("sarremanso")).toBeInTheDocument();
    expect(screen.getByText(/grief that has found its chores/i)).toBeInTheDocument();
    expect(screen.getByText("almanaque-1963-margem.jpg")).toBeInTheDocument();
    expect(screen.getByRole("textbox", { name: "Message" })).toBeInTheDocument();
    expect(screen.getByText(/letters queue until the boat goes/i)).toBeInTheDocument();
  });
});
```

Run and expect failure before implementation.

- [ ] **Step 2: Main session authors the thread composition**

Preserve:

- asymmetric letters rather than generic chat bubbles
- the receipt language `reached the island`
- the word specimen as the page's authored moment
- the attachment as evidence, not decoration
- the composer and boat schedule as one closure
- substantial conversation length; do not reduce it to a demo snippet

Use an ordered semantic message list. Sender identity must not rely on color alone. Mobile keeps the reading sequence and composer without horizontal overflow.

- [ ] **Step 3: Run route checks and commit locally**

```bash
bunx vitest run src/worlds/correspondence/routes/thread/thread-page.test.tsx
bun run typecheck
bun run build
git add src/worlds/correspondence/routes/thread
git commit -m "feat: translate Correspondence thread at fidelity"
```

Expected: tests and build pass; local commit only.

---

### Task 5: Translate The Mat at Fidelity

**Files:**
- Create: `src/worlds/correspondence/routes/mat/mat-page.tsx`
- Create: `src/worlds/correspondence/routes/mat/mat-page.module.css`
- Create: `src/worlds/correspondence/routes/mat/mat-page.test.tsx`

**Interfaces:**
- Consumes: exact source `corpus/product/notifications-the-mat.html` and static fidelity fixtures.
- Produces: `MatPage` with date groups, notice kinds, filters, sweep action, read state, and authored empty state.

- [ ] **Step 1: Write the interaction test**

```tsx
import { render, screen } from "@testing-library/react";
import userEvent from "@testing-library/user-event";
import { describe, expect, test } from "vitest";
import { MatPage } from "./mat-page";

describe("MatPage", () => {
  test("filters notices and sweeps to the authored empty state", async () => {
    const user = userEvent.setup();
    render(<MatPage />);

    await user.click(screen.getByRole("button", { name: "The boat" }));
    expect(screen.getAllByTestId("notice-kind-boat").length).toBeGreaterThan(0);

    await user.click(screen.getByRole("button", { name: /Sweep the mat/i }));
    expect(screen.getByText(/The mat is bare/)).toBeInTheDocument();
    expect(screen.getByText(/the bell is keeping nothing from you/i)).toBeInTheDocument();
  });
});
```

Run and expect failure before implementation.

- [ ] **Step 2: Main session authors The Mat**

Preserve:

- grouped datelines
- instrumental marks whose shape or label carries notice kind
- quiet unread treatment
- sand treatment for boat notices
- deliberate sweep action
- filter behavior
- empty state in the world's voice

Do not use a generic notification list, red count badge, or icon-only categories.

- [ ] **Step 3: Run checks and commit locally**

```bash
bunx vitest run src/worlds/correspondence/routes/mat/mat-page.test.tsx
bun run typecheck
bun run build
git add src/worlds/correspondence/routes/mat
git commit -m "feat: translate The Mat at fidelity"
```

---

### Task 6: Translate The Desk at Fidelity

**Files:**
- Create: `src/worlds/correspondence/routes/desk/desk-page.tsx`
- Create: `src/worlds/correspondence/routes/desk/desk-page.module.css`
- Create: `src/worlds/correspondence/routes/desk/desk-page.test.tsx`

**Interfaces:**
- Consumes: exact source `corpus/product/settings-the-desk.html` and static settings fixtures.
- Produces: `DeskPage` with theme choice, letter size, boat, bell, ledger, kept principle, download action, and isolated irreversible drawer.

- [ ] **Step 1: Write the product-philosophy test**

```tsx
import { render, screen } from "@testing-library/react";
import userEvent from "@testing-library/user-event";
import { describe, expect, test } from "vitest";
import { DeskPage } from "./desk-page";

describe("DeskPage", () => {
  test("keeps product principles out of configurable controls", async () => {
    const user = userEvent.setup();
    render(<DeskPage />);

    expect(screen.getByText(/Your drafts are kept\. Always\./)).toBeInTheDocument();
    expect(screen.queryByRole("switch", { name: /drafts are kept/i })).not.toBeInTheDocument();

    await user.click(screen.getByRole("switch", { name: "Ring for new letters" }));
    expect(screen.getByRole("status")).toHaveTextContent("kept");
  });
});
```

Run and expect failure before implementation.

- [ ] **Step 2: Main session authors The Desk**

Preserve the section sequence and distinctions:

- The sky
- The boat
- The bell
- The ledger
- Kept
- The drawer that locks

Save-on-touch announces `kept` once through a polite live region. The irreversible action remains spatially isolated and stated in full. Do not convert every row into a card or expose `kept` as a switch.

- [ ] **Step 3: Run checks and commit locally**

```bash
bunx vitest run src/worlds/correspondence/routes/desk/desk-page.test.tsx
bun run typecheck
bun run build
git add src/worlds/correspondence/routes/desk
git commit -m "feat: translate The Desk at fidelity"
```

---

### Task 7: Translate the Correspondence API at Fidelity

**Files:**
- Create: `src/worlds/correspondence/routes/api/api-page.tsx`
- Create: `src/worlds/correspondence/routes/api/api-page.module.css`
- Create: `src/worlds/correspondence/routes/api/api-page.test.tsx`

**Interfaces:**
- Consumes: exact source `corpus/content/api-reference-correspondence.html` and canonical endpoint fixtures.
- Produces: `ApiPage` with sidebar navigation, endpoint sections, code examples, tables, refusal semantics, and error voices.

- [ ] **Step 1: Write the API semantics test**

```tsx
import { render, screen } from "@testing-library/react";
import { describe, expect, test } from "vitest";
import { ApiPage } from "./api-page";

describe("ApiPage", () => {
  test("preserves Correspondence behavior in the protocol", () => {
    render(<ApiPage />);

    expect(screen.getByRole("heading", { name: "The Correspondence API" })).toBeInTheDocument();
    expect(screen.getByText(/202 Accepted, never 200/i)).toBeInTheDocument();
    expect(screen.getByRole("heading", { name: "DELETE /v1/drafts/:id" })).toBeInTheDocument();
    expect(screen.getByText(/this endpoint refuses/i)).toBeInTheDocument();
    expect(screen.getByText("THE_DESK_KEEPS")).toBeInTheDocument();
    expect(screen.getByText(/kept is present on every error/i)).toBeInTheDocument();
  });
});
```

Run and expect failure before implementation.

- [ ] **Step 2: Main session authors the API composition**

Preserve:

- slow-version promise
- ledger authentication
- boat semantics
- `202` distinction
- drafts and refusal
- errors with voices table
- boat schedule closure
- code blocks that remain night surfaces in both themes

Documentation clarity outranks metaphor. Every renamed concept must improve the mental model and remain technically precise.

- [ ] **Step 3: Run checks and commit locally**

```bash
bunx vitest run src/worlds/correspondence/routes/api/api-page.test.tsx
bun run typecheck
bun run build
git add src/worlds/correspondence/routes/api
git commit -m "feat: translate Correspondence API at fidelity"
```

---

### Task 8: Add the Disappearing Atlas and System Lens

**Files:**
- Create: `src/atlas/atlas-dialog.tsx`
- Create: `src/atlas/atlas-dialog.module.css`
- Create: `src/atlas/atlas-dialog.test.tsx`
- Create: `src/lens/system-lens.tsx`
- Create: `src/lens/system-lens.module.css`
- Create: `src/lens/system-lens.test.tsx`
- Create: `src/lens/system-metadata.ts`
- Modify: `src/worlds/correspondence/components/instrument-triggers.tsx`
- Modify: `src/router.tsx`

**Interfaces:**
- Consumes: route metadata and `SystemAnnotation`.
- Produces: `AtlasDialog`, `SystemLens`, `useAtlasShortcut()`, and `/atlas` semantic route.

- [ ] **Step 1: Write Atlas keyboard and focus tests**

```tsx
import { render, screen } from "@testing-library/react";
import userEvent from "@testing-library/user-event";
import { describe, expect, test } from "vitest";
import { AtlasDialog } from "./atlas-dialog";

describe("AtlasDialog", () => {
  test("opens by command shortcut, closes, and restores focus", async () => {
    const user = userEvent.setup();
    render(<AtlasDialog />);

    const trigger = screen.getByRole("button", { name: "Open Atlas" });
    trigger.focus();
    await user.keyboard("{Control>}k{/Control}");
    expect(screen.getByRole("dialog", { name: "Atlas" })).toBeVisible();

    await user.keyboard("{Escape}");
    expect(screen.queryByRole("dialog", { name: "Atlas" })).not.toBeInTheDocument();
    expect(trigger).toHaveFocus();
  });

  test("does not intercept the shortcut inside an editable control", async () => {
    const user = userEvent.setup();
    render(<><AtlasDialog /><textarea aria-label="Message" /></>);
    await user.click(screen.getByRole("textbox", { name: "Message" }));
    await user.keyboard("{Control>}k{/Control}");
    expect(screen.queryByRole("dialog", { name: "Atlas" })).not.toBeInTheDocument();
  });
});
```

- [ ] **Step 2: Write system-lens boundary tests**

```tsx
import { render, screen } from "@testing-library/react";
import { describe, expect, test } from "vitest";
import { SystemLens } from "./system-lens";

describe("SystemLens", () => {
  test("starts off and never calls authored moments reusable", () => {
    render(<SystemLens />);
    expect(screen.queryByRole("complementary", { name: "System lens" })).not.toBeInTheDocument();
    expect(screen.queryByText(/creed band/i)).not.toBeInTheDocument();
  });
});
```

Run both tests and expect failure before implementation.

- [ ] **Step 3: Implement the Atlas as a text instrument**

The dialog lists the current Correspondence routes as text links. Future houses and studies may appear as disabled text marked `not yet admitted`; do not invent screenshot cards.

The dedicated `/atlas` route renders the same semantic list without dialog behavior.

- [ ] **Step 4: Implement lens layers from declared metadata only**

`system-metadata.ts` exports a static annotation list. The lens may describe theme, focus, route state, semantic palette, and owned behavior. It must explicitly mark world-local work as `authored` rather than offering it for reuse.

The closed lens leaves no wrapper class, page padding, overlay, or persistent rail.

- [ ] **Step 5: Run checks and commit locally**

```bash
bunx vitest run src/atlas src/lens
bun run typecheck
bun run build
git add src/atlas src/lens src/router.tsx src/worlds/correspondence/components/instrument-triggers.tsx
git commit -m "feat: add disappearing Atlas and system lens"
```

---

### Task 9: Build the Deterministic Fidelity Review Matrix

**Files:**
- Create: `playwright.config.ts`
- Create: `tests/support/routes.ts`
- Create: `tests/support/review-page.ts`
- Create: `tests/e2e/fidelity.spec.ts`
- Create: `tests/e2e/instruments.spec.ts`
- Create: `tests/e2e/accessibility.spec.ts`
- Create: `review/fidelity/report.md`

**Interfaces:**
- Consumes: five fidelity routes, theme contract, Atlas, system lens.
- Produces: deterministic screenshots outside target history, runtime/a11y reports, and a complete fidelity gate packet.

- [ ] **Step 1: Configure the review server and define the exact matrix**

Create `playwright.config.ts`:

```ts
import { defineConfig, devices } from "@playwright/test";

export default defineConfig({
  testDir: "./tests/e2e",
  outputDir: "./review/test-results",
  fullyParallel: false,
  retries: 0,
  use: {
    baseURL: "http://127.0.0.1:41735",
    locale: "en-US",
    timezoneId: "America/Sao_Paulo",
    trace: "retain-on-failure",
  },
  projects: [
    {
      name: "chromium",
      use: { ...devices["Desktop Chrome"] },
    },
  ],
  webServer: {
    command: "bun run dev -- --host 127.0.0.1 --port 41735",
    url: "http://127.0.0.1:41735/houses/correspondence",
    reuseExistingServer: false,
    timeout: 120_000,
  },
});
```

Create `tests/support/routes.ts`:

```ts
export const FIDELITY_ROUTES = [
  "/houses/correspondence",
  "/houses/correspondence/thread",
  "/houses/correspondence/mat",
  "/houses/correspondence/desk",
  "/houses/correspondence/api",
] as const;

export const VIEWPORTS = {
  desktop: { width: 1440, height: 900 },
  mobile: { width: 390, height: 844 },
} as const;

export const THEMES = ["light", "dark"] as const;
```

- [ ] **Step 2: Implement deterministic page preparation**

Create `tests/support/review-page.ts`:

```ts
import type { Page } from "@playwright/test";

export async function prepareReviewPage(
  page: Page,
  theme: "light" | "dark",
): Promise<void> {
  await page.addInitScript((selectedTheme) => {
    localStorage.setItem("vsh-correspondence-theme", selectedTheme);
    Object.defineProperty(Date, "now", { value: () => 1_784_009_600_000 });
  }, theme);
  await page.emulateMedia({ reducedMotion: "reduce", colorScheme: theme });
}

export async function settlePage(page: Page): Promise<void> {
  await page.evaluate(async () => {
    await document.fonts.ready;
  });
  await page.waitForLoadState("networkidle");
}
```

- [ ] **Step 3: Write full-page fidelity captures and overflow assertions**

`fidelity.spec.ts` iterates every route, theme, and viewport. For each case it asserts:

```ts
const overflow = await page.evaluate(
  () => document.documentElement.scrollWidth - document.documentElement.clientWidth,
);
expect(overflow).toBeLessThanOrEqual(0);
await expect(page).toHaveScreenshot(`${name}.png`, {
  fullPage: true,
  animations: "disabled",
});
```

Expected matrix: `5 routes × 2 themes × 2 viewports = 20` full-page captures.

- [ ] **Step 4: Add runtime and accessibility failures**

Each test case installs these collectors before navigation:

```ts
const failures: string[] = [];
page.on("console", (message) => {
  if (message.type() === "error") failures.push(`console: ${message.text()}`);
});
page.on("pageerror", (error) => failures.push(`page: ${error.message}`));
page.on("requestfailed", (request) => {
  failures.push(`request: ${request.url()} ${request.failure()?.errorText ?? "failed"}`);
});
page.on("response", (response) => {
  if (response.status() >= 400) failures.push(`http: ${response.status()} ${response.url()}`);
  const url = new URL(response.url());
  if (url.hostname !== "127.0.0.1") failures.push(`external: ${response.url()}`);
});
```

Run axe after the page settles:

```ts
const results = await new AxeBuilder({ page }).analyze();
expect(
  results.violations.filter((violation) =>
    violation.impact === "serious" || violation.impact === "critical"
  ),
).toEqual([]);
expect(failures).toEqual([]);
```

This fails on console errors, page errors, request failures, responses with status `>=400`, external runtime requests, and serious or critical axe violations.

- [ ] **Step 5: Verify Atlas and lens behavior**

`instruments.spec.ts` confirms:

- Atlas opens and closes without moving underlying content.
- Focus returns to its trigger.
- Lens defaults off.
- Each lens layer has a structured text representation.
- Closing the lens removes every overlay and page annotation class.
- `⌘K` or `Ctrl+K` is ignored inside the thread composer.

- [ ] **Step 6: Run the complete mechanical gate**

Run:

```bash
bun run check:provenance
bun run format:check
bun run lint
bun run typecheck
bun run test
bun run build
bunx playwright install chromium
bun run test:e2e
```

Expected:

- provenance exact
- zero lint warnings
- all unit tests pass
- production build passes
- 20 fidelity captures generated
- zero overflow
- zero console, page, request, or HTTP failures
- zero serious or critical axe violations

- [ ] **Step 7: Main session performs full-height visual review**

Compare each live route against its exact pinned source in both themes and both viewports. Record in `review/fidelity/report.md` for each route:

```markdown
## <route>

- Source read: <subject, tension, authored moment>
- Fidelity verdict: pass | rework | reject
- Preserved: <specific evidence>
- Lost or weakened: <specific evidence>
- Responsive verdict: <specific evidence>
- Dark verdict: <specific evidence>
```

No `pass` may cite automated scores as visual evidence.

- [ ] **Step 8: Parent-only local commit**

```bash
git add playwright.config.ts tests review/fidelity/report.md
git commit -m "test: complete Correspondence fidelity matrix"
```

---

### Task 10: Operator Fidelity Gate

**Files:**
- Modify after approval only: `review/fidelity/report.md`

**Interfaces:**
- Consumes: live proof, 20 captures, runtime report, accessibility report, main-session visual review.
- Produces: one operator decision: `approve fidelity`, `rework fidelity`, or `reject proof`.

- [ ] **Step 1: Present the live proof, not a montage**

Open the local Correspondence landing route in Chromium. Provide direct links for the five routes and the Atlas. Do not open a separate review dashboard.

- [ ] **Step 2: Request the fidelity decision**

The decision prompt is exactly:

```text
Does this live React world preserve the original Correspondence quality, voice, and distinct page silhouettes strongly enough to begin elevation?
```

Options:

- Approve fidelity
- Rework fidelity
- Reject proof

- [ ] **Step 3: Enforce the decision**

If `rework fidelity`, return to the rejected route task and repeat Task 9.

If `reject proof`:

```bash
rm -rf "$CLAUDE_JOB_DIR/tmp/correspondence-living-world-proof"
```

Then stop. Do not write elevation code or copy any proof file into the target repository.

If `approve fidelity`, append the decision and date to `review/fidelity/report.md` and create the parent-only local tag:

```bash
git add review/fidelity/report.md
git commit -m "docs: record fidelity approval"
git tag fidelity-approved
```

Expected: Gate 1 is explicit and reproducible.

---

### Task 11: Author and Approve the Correspondence Elevation Brief

**Files:**
- Create: `elevation-brief.md`
- Create: `review/elevation/ledger-study.html`

**Interfaces:**
- Consumes: approved fidelity proof and corpus laws.
- Produces: an operator-approved elevation contract before any elevation code.

- [ ] **Step 1: Main session writes the exact elevation brief**

Create `elevation-brief.md` with this fixed intent:

```markdown
# Correspondence Elevation Brief

## Beliefs extended

- Letters move on a schedule rather than a feed.
- Everything written is kept.
- The bell may be configured; retention may not.
- Product, settings, notifications, and API describe one reality.

## Cross-route continuity

1. A draft written in the thread remains after route changes.
2. Sealing the draft queues it for the next 09:00 or 16:00 boat.
3. The API sandbox uses the same command and returns the same departure.
4. Advancing the deterministic boat scenario moves queued letters to crossed.
5. The Mat receives one boat notice and, when enabled, one letter notice.
6. The Desk changes bell behavior but cannot disable `kept` or boat batching.

## New authored moment: The Ledger Between Boats

A new `/houses/correspondence/ledger` route shows one day divided by the two crossings. Letters occupy the interval in which they were written, queued, and crossed. It is not a feed, activity timeline, kanban board, or analytics chart. The composition makes waiting visible as protected time.

## Refusals

- no global notification badge
- no live countdown pressure
- no streak or engagement metric
- no destructive draft action
- no universal dashboard shell
- no confetti or arrival animation
```

- [ ] **Step 2: Produce one static visual study outside the target repository**

The main session authors `review/elevation/ledger-study.html` as a self-contained disposable study. Its semantic skeleton is:

```html
<main aria-labelledby="ledger-title">
  <header>
    <p>Correspondence · the ledger</p>
    <h1 id="ledger-title">Between the boats</h1>
    <p>What is written is kept. What is sealed waits without asking the day to hurry.</p>
  </header>
  <section aria-labelledby="morning-crossing">
    <h2 id="morning-crossing">The nine o'clock boat</h2>
    <article data-status="crossed">A crossed letter with its reader and receipt.</article>
  </section>
  <section aria-labelledby="between-crossings">
    <h2 id="between-crossings">The day between</h2>
    <article data-status="draft">One kept draft.</article>
    <article data-status="queued">One letter kept for four.</article>
  </section>
  <section aria-labelledby="afternoon-crossing">
    <h2 id="afternoon-crossing">The four o'clock boat</h2>
    <p>The crossing is named; no countdown applies pressure.</p>
  </section>
</main>
```

The main session owns the complete visual composition. The study must make the two crossings, one draft, one queued letter, and one crossed letter legible without resembling a feed, timeline component, project board, or analytics chart.

- [ ] **Step 3: Request the elevation-design decision**

Ask:

```text
Does The Ledger Between Boats feel like Correspondence discovering more of itself rather than an external feature added to it?
```

Options:

- Approve elevation design
- Rework elevation design
- Reject elevation design

No state or Ledger implementation begins until approved.

- [ ] **Step 4: Parent-only local commit after approval**

```bash
git add elevation-brief.md review/elevation/ledger-study.html
git commit -m "docs: approve Correspondence elevation brief"
```

---

### Task 12: Implement Shared Correspondence State After Elevation Approval

**Files:**
- Create: `src/worlds/correspondence/state/correspondence-state.ts`
- Create: `src/worlds/correspondence/state/correspondence-reducer.ts`
- Create: `src/worlds/correspondence/state/correspondence-provider.tsx`
- Create: `src/worlds/correspondence/state/correspondence-reducer.test.ts`
- Modify: `src/worlds/correspondence/world-layout.tsx`

**Interfaces:**
- Consumes: approved elevation brief and fixed elevation state types.
- Produces: `initialCorrespondenceState`, `correspondenceReducer`, `CorrespondenceProvider`, and `useCorrespondence()`.

- [ ] **Step 1: Write reducer tests first**

```ts
import { describe, expect, test } from "vitest";
import {
  correspondenceReducer,
  initialCorrespondenceState,
} from "./correspondence-reducer";

describe("correspondenceReducer", () => {
  test("keeps every draft", () => {
    const state = correspondenceReducer(initialCorrespondenceState, {
      type: "draftChanged",
      id: "draft-elena-1",
      body: "The sentence stays.",
    });

    expect(state.letters[0]).toMatchObject({
      id: "draft-elena-1",
      body: "The sentence stays.",
      status: "draft",
      kept: true,
    });
  });

  test("queues sealed letters for a named boat", () => {
    const state = correspondenceReducer(initialCorrespondenceState, {
      type: "letterSealed",
      id: "draft-elena-1",
      departure: "16:00",
    });

    expect(state.letters[0]).toMatchObject({
      status: "queued",
      departure: "16:00",
      kept: true,
    });
  });

  test("moves only the selected boat and writes one Mat notice", () => {
    const state = correspondenceReducer(initialCorrespondenceState, {
      type: "boatCrossed",
      departure: "16:00",
    });

    expect(state.letters.filter((letter) => letter.status === "crossed")).toHaveLength(1);
    expect(state.notices.filter((notice) => notice.kind === "boat")).toHaveLength(1);
  });

  test("never exposes an action that disables kept or boat batching", () => {
    const actionTypes = [
      "draftChanged",
      "letterSealed",
      "boatCrossed",
      "noticeRead",
      "noticesSwept",
      "bellChanged",
      "quietBeforeEightChanged",
    ];
    expect(actionTypes).not.toContain("keptChanged");
    expect(actionTypes).not.toContain("queueUntilBoatChanged");
  });
});
```

Run and expect failure before implementation.

- [ ] **Step 2: Implement the minimal pure reducer**

Implement exactly the action union from Shared Interfaces. Preserve `kept: true` and `queueUntilBoat: true` as literal types and runtime invariants.

Do not persist state yet; provider state lasts for the current proof session so tests reset deterministically.

- [ ] **Step 3: Add the provider to the world layout**

```tsx
export function CorrespondenceWorldLayout() {
  return (
    <CorrespondenceProvider>
      <InstrumentTriggers />
      <Outlet />
    </CorrespondenceProvider>
  );
}
```

- [ ] **Step 4: Run checks and commit locally**

```bash
bunx vitest run src/worlds/correspondence/state
bun run typecheck
bun run build
git add src/worlds/correspondence/state src/worlds/correspondence/world-layout.tsx
git commit -m "feat: connect Correspondence state across routes"
```

---

### Task 13: Elevate the Five Routes and Add The Ledger Between Boats

**Files:**
- Create: `src/worlds/correspondence/routes/ledger/ledger-page.tsx`
- Create: `src/worlds/correspondence/routes/ledger/ledger-page.module.css`
- Create: `src/worlds/correspondence/routes/ledger/ledger-page.test.tsx`
- Modify: `src/worlds/correspondence/routes/thread/thread-page.tsx`
- Modify: `src/worlds/correspondence/routes/mat/mat-page.tsx`
- Modify: `src/worlds/correspondence/routes/desk/desk-page.tsx`
- Modify: `src/worlds/correspondence/routes/api/api-page.tsx`
- Modify: `src/worlds/correspondence/world-meta.ts`
- Modify: `src/router.tsx`

**Interfaces:**
- Consumes: `useCorrespondence()` and approved elevation brief.
- Produces: live cross-route state and `LedgerPage` at `/houses/correspondence/ledger`.

- [ ] **Step 1: Write the cross-route elevation test**

Create `tests/e2e/elevation.spec.ts`:

```ts
import { expect, test } from "@playwright/test";

test("a kept letter moves through the Correspondence world", async ({ page }) => {
  await page.goto("/houses/correspondence/thread");
  const composer = page.getByRole("textbox", { name: "Message" });
  await composer.fill("The sentence stays between boats.");
  await page.getByRole("button", { name: "Seal for the four o'clock boat" }).click();

  await page.goto("/houses/correspondence/ledger");
  await expect(page.getByText("The sentence stays between boats.")).toContainText("16:00");
  await expect(page.getByText("kept")).toBeVisible();

  await page.getByRole("button", { name: "Advance the four o'clock boat" }).click();
  await page.goto("/houses/correspondence/mat");
  await expect(page.getByText(/the four o'clock boat crossed/i)).toBeVisible();
});
```

Run and expect failure before implementation.

- [ ] **Step 2: Connect the thread composer**

Draft changes dispatch `draftChanged`. The seal action dispatches `letterSealed` and states the named departure. The composer never says `send now`.

- [ ] **Step 3: Connect the API sandbox**

The `POST /v1/letters` example gains an interactive local request panel that dispatches the same `letterSealed` command and renders a deterministic `202` response containing `departure`, `status: "queued"`, and `kept: true`.

Do not alter the reference text or imply the public API is live.

- [ ] **Step 4: Connect The Mat and The Desk**

The Mat reads notices from shared state and retains its fidelity filters and empty state. The Desk writes only bell and quiet-hours settings; `kept` remains an invariant without a control.

- [ ] **Step 5: Main session authors The Ledger Between Boats**

The route contains:

- one day, not an infinite history
- two named crossings: 09:00 and 16:00
- letters placed by status and departure
- direct status language: draft, kept for the boat, crossed
- an explicit statement that waiting protects the work between departures
- deterministic review controls to advance one boat

It must not resemble a feed, project board, generic timeline, dashboard, or analytics chart. The visual study approved in Task 11 is the composition contract.

- [ ] **Step 6: Register Ledger as elevation-only metadata**

Add:

```ts
{
  id: "ledger",
  path: "/houses/correspondence/ledger",
  title: "The Ledger Between Boats",
  sourcePath: null,
  phase: "elevation",
}
```

The system lens marks it `authored`, not registry-owned.

- [ ] **Step 7: Run checks and commit locally**

```bash
bunx vitest run src/worlds/correspondence
bun run typecheck
bun run build
bunx playwright test tests/e2e/elevation.spec.ts
git add src/worlds/correspondence src/router.tsx tests/e2e/elevation.spec.ts
git commit -m "feat: elevate Correspondence between boats"
```

---

### Task 14: Complete the Elevation Review Matrix

**Files:**
- Modify: `tests/support/routes.ts`
- Create: `review/elevation/report.md`
- Modify: `tests/e2e/elevation.spec.ts`

**Interfaces:**
- Consumes: six-route elevated world and fidelity tag.
- Produces: side-by-side fidelity/elevation evidence and Gate 2 packet.

- [ ] **Step 1: Add Ledger to the route matrix**

Expected elevated matrix: `6 routes × 2 themes × 2 viewports = 24` full-page captures.

- [ ] **Step 2: Run the full elevated mechanical gate**

```bash
bun run check:provenance
bun run format:check
bun run lint
bun run typecheck
bun run test
bun run build
bun run test:e2e
```

Expected: all pass, zero warnings, zero overflow, zero runtime failures, zero serious or critical axe violations.

- [ ] **Step 3: Main session compares three states of the work**

For every original route, compare:

1. exact pinned source
2. fidelity-approved proof at tag `fidelity-approved`
3. elevated proof at current HEAD

Record in `review/elevation/report.md`:

```markdown
## <route>

- Original belief preserved: <specific evidence>
- Fidelity proof preserved: <specific evidence>
- Elevation added: <specific evidence>
- Why it belongs only to Correspondence: <specific evidence>
- External trend language introduced: none | <reject>
- Verdict: pass | rework | reject
```

For Ledger, record the source beliefs it extends rather than claiming pinned-page provenance.

- [ ] **Step 4: Parent-only local commit**

```bash
git add tests review/elevation/report.md
git commit -m "test: complete Correspondence elevation matrix"
```

---

### Task 15: Operator Elevation Gate and Clean-Room Handoff

**Files:**
- Modify after approval: `review/elevation/report.md`
- Create after approval in target repo: `docs/superpowers/specs/2026-07-14-correspondence-approved-result.md`
- Create after approval in target repo: `docs/superpowers/plans/2026-07-14-correspondence-clean-implementation.md`

**Interfaces:**
- Consumes: approved Living Worlds spec, fidelity tag, elevated proof, 24 captures, reports, and live route journey.
- Produces: one Gate 2 decision and, only after approval, a text-only clean-room contract plus a separate implementation plan.

- [ ] **Step 1: Present the elevated live world directly**

Open the Correspondence landing route. Demonstrate one journey:

1. read the landing proposition
2. open Elena and Tomaz
3. draft and seal a letter for 16:00
4. inspect it in The Ledger Between Boats
5. advance the boat
6. read the Mat notice
7. change the bell at The Desk
8. invoke the same behavior through the API sandbox
9. open and close Atlas
10. open and close every system-lens layer

No review dashboard or screenshot montage is the primary presentation.

- [ ] **Step 2: Request the Gate 2 decision**

Ask exactly:

```text
Has Correspondence first become the original world and then elevated itself from inside its own beliefs strongly enough to deserve clean implementation?
```

Options:

- Approve elevated world
- Rework elevation
- Reject elevated world

- [ ] **Step 3: Enforce rejection hygiene**

If `rework elevation`, return to Tasks 11 through 14. Fidelity remains the comparison baseline.

If `reject elevated world`, remove the disposable proof and stop:

```bash
rm -rf "$CLAUDE_JOB_DIR/tmp/correspondence-living-world-proof"
```

Do not copy code, CSS, fixtures, captures, or snapshots into the target repository.

- [ ] **Step 4: Record approval as text only**

After `approve elevated world`, create `docs/superpowers/specs/2026-07-14-correspondence-approved-result.md` in the target repository. It records:

- approved route list
- approved visual and behavioral invariants
- exact source paths and SHA for the five fidelity works
- Ledger as native elevation with no source path
- approved typography roles and palette behavior
- shared state commands and consequences
- Atlas and lens boundaries
- rejected alternatives
- full mechanical gate counts
- operator approval date

It contains no proof source code, screenshots, or generated snapshots.

- [ ] **Step 5: Write the separate clean implementation plan**

Invoke `superpowers:writing-plans` against the approved-result spec. That plan must recreate the approved world from the text contract and exact pinned sources in the target repository without copying proof files or Git history.

- [ ] **Step 6: Commit and push target documentation**

From `frontend-design-system`:

```bash
git add docs/superpowers/specs/2026-07-14-correspondence-approved-result.md \
  docs/superpowers/plans/2026-07-14-correspondence-clean-implementation.md
git commit -m "docs: approve Correspondence living world"
git push
```

Expected: target history contains contracts and plans only; proof code remains disposable and remote-free.

- [ ] **Step 7: Delete the proof only after the clean plan is durable**

```bash
test -f docs/superpowers/specs/2026-07-14-correspondence-approved-result.md
test -f docs/superpowers/plans/2026-07-14-correspondence-clean-implementation.md
rm -rf "$CLAUDE_JOB_DIR/tmp/correspondence-living-world-proof"
```

Expected: approved behavior is durable as text, disposable implementation is gone, and the target is ready for clean-room execution.

---

## Plan Completion Contract

This plan is complete when:

- the exact source SHA is verified
- five fidelity routes exist only in the disposable proof
- Atlas and system lens disappear completely when closed
- 20 fidelity captures and all mechanical checks pass
- the operator approves Gate 1
- the elevation brief and Ledger study are approved before elevation code
- shared state proves one Correspondence reality across routes
- 24 elevated captures and all mechanical checks pass
- the operator approves Gate 2
- target Git contains only the approved-result text contract and the next clean implementation plan
- disposable proof code, captures, snapshots, and Git history are deleted after handoff
