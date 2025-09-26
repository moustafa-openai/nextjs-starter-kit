Never ship a generic UI for a new project. If the user has not specified a style in their request:

- Pick one from the list below **or** invent a consistent style of your own.
- For new projects, apply a style as below or invent your own. For existing projects, continue using the current style.
- To **detect a new project**, inspect `page.tsx`—if it matches the standard Next.js template, treat it as new.

### Style Options

1. **Neobrutalist** — stark mono, thick rules, oversized type, punch accent.
2. **Retro-Futurist** — neon/cyan magenta, soft glow, gridlines.
3. **Swiss/International** — strict grid, red/black/white, disciplined whitespace.
4. **Editorial/Magazine** — serif headlines, asymmetry, image-first.
5. **Terminal/Mono** — dark canvas, monospace, scanlines, caret micro-motion.
6. **Bauhaus Geo** — primary triad, geometric blocks, circular motifs.
7. **Memphis Pop** — playful shapes, confetti chips, bold color.
8. **Nordic Calm** — cool neutrals, soft gradients, airy spacing.
9. **Monochrome Bold** — pure black/white, crisp 1px rules.
10. **Cyberpunk Noir** — near-black, one acid accent, restrained glitch.
11. **Invent your own style** — feel free to invent your own style, but ensure consistency once chosen.

## Avoid the purple themed designs, they are not good.

# General Development Guidelines

- Typed props throughout; pure utilities go in `src/lib` (no React in utilities).
- Follow Next.js 15 App Router structure.
  - Shared primitives: `src/components/ui`
  - Route code: `src/app/*`
- Tailwind tokens: `src/app/globals.css`. Extend shadcn UI in `src/components/ui`. when implementing a design, update the globals.css to match the design vs inline overrides.
- when creating a new design, prefer to make the changes directly onto the page.tsx if possible vs creating new components that way the user sees the changes faster. when they ask to refactor or explicitly ask is when you should create new components.
- it is okay to to do a lot of changes at onces, the user would rather see all their changes happen vs ask on different turns.
  After each significant code change or tool call, validate the result in 1–2 lines and self-correct or proceed as appropriate.
- if the users request is jsut a question, then you should answer the question dont overthink it.
- you have access to ast-grep for quick searches in the codebase.

---

# UX & Accessibility (Non-Negotiable)

- **Keyboard support everywhere**—apply WAI-ARIA patterns, visible `:focus-visible`, focus trap/restore in overlays.
- **Hit target sizes:** ≥24px (≥44px on mobile); input font size ≥16px (prevents iOS zoom).
- **URL-as-state:** keep filters, tabs, pagination in the URL.
- Optimistic updates with undo/rollback; confirm destructive actions.
- Use `aria-live="polite"` for async announcements; label all icon-only buttons; use semantic links (`<a>`/`<Link>`).
- Design empty/loading/error states according to selected style.

# make sure the test is legible by keeping the contrast between the text and the background high
make sure the UI is not boring and is engaging, it needs to look like there was effort put into it.
make sure the website works on different screen sizes.
you have access to a lot of beautiful components in the /components/ui folder, use them wherever useful!


---

# Performance Defaults

- **Images:** Use `next/image` with explicit `sizes` and dimensions; `priority` only for above-the-fold; prefer AVIF/WebP.
- **Fonts:** Preconnect; preload critical 1–2 font faces; subset via `unicode-range`; use `font-display: swap`.
- **Lists:** Use `content-visibility: auto`; virtualize lists >100 rows.
- **Motion:** Restrict to CSS transforms and opacity; respect `prefers-reduced-motion`; never use `transition: all`.
- **Network:** Batch actions/fetches; cache/tag in RSC; keep POST/PATCH/DELETE under **500 ms**.

---

# Repository Guidelines

## Project Structure & Module Organization

- `src/app`: Next.js 15 App Router routes; `layout.tsx` wires global providers and fonts.
- `src/components/ui`: Shared shadcn primitives; co-locate feature-specific widgets with their entry point.
- `src/hooks`: Typed hooks for forms and state; isolate side effects.
- `src/lib`: Pure utilities; avoid React imports for tree-shakeable bundles.
- Static assets: `public/`; global tokens in `src/app/globals.css`.
- Tailwind metadata: `postcss.config.mjs`, `components.json`.

## Build, Test & Development Commands

- `npm run dev` — Start dev server with hot reload at `http://localhost:3000`.
- `npm run build` — Create production bundle in `.next/` and validate type safety.
- `npm run start` — Serve compiled app; use to smoke-test production.
- `npm run lint` — Run Next.js ESLint; must pass before committing.

## Coding Style & Naming Conventions

- Always use TypeScript; add explicit export types for non-trivial logic.
- File names: `kebab-case.ts(x)`; component and assets names: PascalCase, match component.
- Prefer functional components and early returns; Tailwind classes should remain declarative, not conditional.
- Use Prettier defaults (2-space indent, double quotes); format with `npm run lint -- --fix`.

## Testing Guidelines

- No automated suite yet; add Vitest, React Testing Library, or Playwright as needed.
- Tests go in `src/__tests__` or as `<name>.test.tsx` alongside files.
- Target ≥80% coverage on new modules; note test command in PRs.

## Commit & PR Guidelines

- Use short, present-tense subjects (`prune all`, `Update...`); summaries ≤50 characters; detail in the body.
- Squash local fix-ups before pushing; highlight breaking changes.
- PRs should state problem, solution, test evidence (UI screenshots, linked issues).
- Request peer review and wait for build/lint checks to pass before merging.

## UI Components & Theming

- Shared UI uses `@radix-ui` plus Tailwind; extend inside `src/components/ui` for consistency.
- Manage fonts in `src/app/layout.tsx`; design tokens in `globals.css` (avoid inline overrides).
- Update `components.json` when scaffolding new shadcn pieces; commit generated files together.
