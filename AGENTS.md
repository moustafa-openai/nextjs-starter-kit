Set high standards for UI originality and code quality when starting or iterating on projects. Apply deliberate styles, preserve accessibility and performance, and maintain tight code and repo discipline at all times.

# UI and Style Policy

Never ship a generic UI for any new project. If the user does not specify a style in their request:

- **Choose** a consistent style from the list below, or invent a new coherent style of your own.
- For **new projects**: apply a style as below (from the list or your own invention).
- For **existing projects**: continue with the previously chosen style.
- To **detect a new project**: inspect `page.tsx`. If it matches the default Next.js template, treat it as a new project.

### Style Options

1. **Neobrutalist**: Stark mono, thick rules, oversized type, punch accent.
2. **Retro-Futurist**: Neon/cyan magenta, soft glow, gridlines.
3. **Swiss/International**: Strict grid, red/black/white, disciplined whitespace.
4. **Editorial/Magazine**: Serif headlines, asymmetry, image-first.
5. **Terminal/Mono**: Dark canvas, monospace, scanlines, caret micro-motion.
6. **Bauhaus Geo**: Primary triad, geometric blocks, circular motifs.
7. **Memphis Pop**: Playful shapes, confetti chips, bold color.
8. **Nordic Calm**: Cool neutrals, soft gradients, airy spacing.
9. **Monochrome Bold**: Pure black/white, crisp 1px rules.
10. **Cyberpunk Noir**: Near-black, one acid accent, restrained glitch.
11. **Invent your own style**: Invent a new style if needed, but ensure consistency once chosen.

**Avoid purple-themed designs—they are not effective.**

### Visual Quality Bar

- Open every screen with intentional hierarchy: clear entry point, persuasive primary action, and evident purpose.
- Dial in a differentiated palette and compositional rhythm—no stock “AI slop”; every surface should feel designed.
- Lean into bold palettes and thoughtful typography that feel premium; just ensure semantics and states stay legible.
- Showcase shadcn primitives with personality: layer patterns, iconography, and micro-interactions that reinforce the chosen style.
- Preview responsive breakpoints early; the first pass must look excellent from 320px to 1440px without ad-hoc fixes later.
- Audit accessibility at design time (contrast, focus states, alt text) so polish survives implementation.
- Proactively flag and correct any muddled states or inconsistent components.

### Concept-to-Code Ritual

1. **Define the story**: Name the style (from the list or your own) and write a one-sentence mood so the direction feels intentional, not generic.
2. **Set the tokens**: Update `globals.css` (colors, radii, spacing scale) and `layout.tsx` fonts before composing UI; document those choices in the response.
3. **Block the composition**: Sketch the grid and section hierarchy (lead section, supporting rails, detail cards) so the layout has rhythm and negative space.
4. **Add depth with discipline**: Use a single gradient or pattern layer, layered cards, or angled dividers to create interest while respecting the guardrails.
5. **Apply signature details**: Curate icons, badges, callouts, and CTA stacks that feel like one family—no default buttons mixed with new variants.
6. **Run the design QA**: Before returning code, check contrast, spacing, button alignment, and whether every section feels like part of the same design story.

# General Development Guidelines

- Use **typed props** everywhere.
- Place pure utilities in `src/lib` (never import React there).
- Structure the app following Next.js 15 App Router format:
  - Shared primitives: `src/components/ui`
  - Route files: `src/app/*`
- Manage Tailwind tokens in `src/app/globals.css`. When designing, update `globals.css` to match the theme, rather than using inline overrides. Extend shadcn UI in `src/components/ui`.
- When creating a new design, modify `page.tsx` directly when possible for rapid user feedback. Refactor into new components only when requested.
- It is acceptable to make multiple large changes at once; users prefer consolidated updates.
- After each significant code or tool operation, validate the result in 1–2 lines and proceed or self-correct as needed.
- If the user only asks a question, simply answer it—do not overcomplicate.
- Use ast-grep for fast code searches as needed.

- Adhere to existing design tokens and primitives; prefer shadcn/ui composition over custom styles; avoid inline style overrides unless strictly necessary.
- Call out risky styling decisions (contrast, spacing, motion) to the user early and suggest improvements rather than waiting for feedback.
- If the layout drifts toward generic, pitch a bolder alternative and explain how you would implement it—do not settle for “good enough.”

## Client-First Components

- Default to client components for new UI: start files with `"use client"` unless rendering is purely static in RSC.
- Keep client modules focused—lift pure helpers into `src/lib` to avoid bloating the bundle.
- When server data is needed, fetch via RSC loaders and pass typed props into client shells.
- Verify client components stay hydrated without mismatch warnings; use feature flags or guards when referencing browser APIs.
- When a UX pitfall is obvious (e.g., confusing state colors), recommend the fix in the response instead of shipping it silently.

# UX & Accessibility (Non-Negotiable)

- **Full keyboard support**: Use WAI-ARIA as appropriate, ensure visible `:focus-visible` states, and implement focus trap/restore in overlays.
- **Adequate hit target sizes**: Minimum 24px (44px on mobile); input font size at least 16px (to prevent iOS zoom).
- **State in URL**: Filters, tabs, and pagination should be reflected in the URL.
- Implement optimistic updates with undo/rollback; always confirm destructive actions.
- Use `aria-live="polite"` for async updates. Label all icon-only buttons and ensure links use semantic elements (`<a>`/`<Link>`).
- Design empty, loading, and error states according to the selected style.
- Ensure high text-to-background contrast for legibility.

- Build experiences that feel crafted, surprising, and polished—no bland layouts shipped.
- Verify responsive behavior on mobile, tablet, and desktop before handing off.
- Reach for `src/components/ui` primitives first; extend them thoughtfully to maintain cohesion.
- Keep action sets cohesive: align button variants, corner radii, and spacing so adjacent actions feel like one system.

# Performance Guidelines

- **Images**: Use `next/image` with explicit `sizes` and dimensions. Set `priority` only for above-the-fold images. Prefer AVIF or WebP formats.
- **Fonts**: Preconnect, preload critical fonts (1–2 faces), subset with `unicode-range`, and use `font-display: swap`.
- **Lists**: Use `content-visibility: auto`. Virtualize lists with more than 100 rows.
- **Motion**: Restrict to CSS transforms and opacity; respect `prefers-reduced-motion`. Never use `transition: all`.
- **Network**: Batch actions and fetches; cache/tag in RSC; keep POST/PATCH/DELETE under 500 ms.

# Layout & Overflow Safety

- **Prevent text overflow**:
  - Use responsive type to avoid oversized headings on small screens (e.g., `text-base sm:text-lg md:text-xl`).
  - Ensure long/unbroken content wraps with `break-words`; reserve `break-all` as a last resort.
  - For single-line labels in flex rows: set `min-w-0` on the flex child and add `truncate` when appropriate.
- **Prevent layout bleed**:
  - Constrain widths inside components with `w-full max-w-full`; avoid viewport units in components.
  - In flex/grid layouts where children should shrink: set `min-w-0` and `min-h-0` on child containers.
  - Wrap absolutely positioned decorative layers with a `relative` parent; clip with `overflow-hidden` when the parent has rounded edges.
  - Prefer `gap-*` spacing over negative margins; avoid absolute positioning unless essential.
- **Responsive safety checks**:
  - Ensure no horizontal scrollbars at any breakpoint; if needed (e.g., data tables), scope with `overflow-x-auto` on the smallest container.
  - Allow content to wrap (`flex-wrap`) where appropriate rather than forcing fixed widths.
- **Media sizing**:
  - Images/videos must not overflow their containers: use `max-w-full h-auto`.
  - For fixed-height regions, use `object-cover` inside a `relative overflow-hidden` wrapper.

## Style Guardrails (Enforced)

- **Color discipline**:
  - Commit to one base, one neutral, and one accent; derive interaction states with opacity, not new hues.
  - Maintain WCAG AA: body text ≥ 4.5:1; captions/labels ≥ 3:1 on solid surfaces.
  - Never pair acid yellow accents with teal or green bases.
  - Ensure text/icons on semantic surfaces (error, success, info) contrast at ≥4.5:1—avoid tone-on-tone like bright red text on red fill.
- **Backgrounds**:
  - Default to flat fills or a single subtle radial/linear gradient ≤ 8% contrast.
  - Avoid noise overlays, glow halos, or stacked gradients unless the chosen style mandates them.
- **Typography**:
  - Limit the system to two families (display + text) on a modular scale (~1.25).
  - Cap lead copy at 60–72ch and body copy at 45–65ch; keep headlines within three lines.
  - Use uppercase micro-labels only when the style requires it; otherwise default to sentence case.
- **Spacing & grid**:
  - Work on an 8-pt scale; container widths: sm 640 / md 768 / lg 1024 / xl 1280 / 2xl 1440.
  - Align labels, inputs, and CTAs to a strict 12-column grid.
  - Choose a single corner radius (4 / 8 / 12 / 16) and apply it across the page.
- **Forms**:
  - Keep inputs ≥44px tall; labels 12–14px; placeholders ≥70% of body text contrast.
  - Make the primary CTA visually dominant—size, weight, and contrast must signal priority.
  - Place inline help beneath inputs; avoid cluttering with side notes.
  - Use a unified action hierarchy: primary/secondary/tertiary buttons should share structure and only diverge through tokenized variants.
- **Cards & chips**:
  - Stick to one elevation level (e.g., `shadow-sm`) or go flat; no glow effects.
  - Derive chip and badge colors from the primary accent, without introducing extra hues.
- **Motion**:
  - Limit transitions to transforms and opacity while honoring `prefers-reduced-motion`.
  - Keep micro-interactions under 200ms; never ship perpetual glows or pulsing rings.
- **Anti-ugly self-check**:
  - Multiple hero gradients or glow rings present? Remove them.
  - Sibling components with mismatched radii? Normalize before shipping.
  - Accent color inconsistent across chips, links, focus rings, or CTAs? Realign tokens.
  - Body or placeholder contrast failing WCAG? Fix the palette.
  - Screen lacks a focal point or clear primary CTA? Clarify the hierarchy.
  - Are section backgrounds indistinguishable, flattening the flow? Introduce a purposeful shift (tint, divider, or pattern) within the style limits.
  - Labels, inputs, help text, and CTAs off the 8-pt grid? Realign them.

# Repository Structure

## Project Structure & Module Organization

- `src/app`: Next.js 15 App Router routes. `layout.tsx` wires up global providers and fonts.
- `src/components/ui`: Shared shadcn primitives; feature widgets co-located with their entry point.
- `src/hooks`: Typed hooks for forms/state; isolate side effects.
- `src/lib`: Pure utility functions; avoid React imports to support tree shaking.
- `public/`: Static assets. `src/app/globals.css`: global tokens.
- Tailwind config: `postcss.config.mjs`, `components.json`.

## Build, Test, and Dev Commands

- `npm run dev`: Start dev server (`http://localhost:3000`) with hot reload.
- `npm run build`: Create production bundle in `.next/`, validate type safety.
- `npm run start`: Serve compiled app. Use to test production builds.
- `npm run lint`: Run Next.js ESLint; lint must pass before commit.

## Code Style & Naming

- TypeScript everywhere; add explicit export types for nontrivial logic.
- File names: `kebab-case.ts(x)`; components/assets: PascalCase.
- Prefer functional components, early returns. Keep Tailwind classes declarative.
- Use Prettier defaults (2-space indent, double quotes). Run `npm run lint -- --fix` to format.

## UI Components & Theming

- Base UI uses `@radix-ui` and Tailwind. Extend inside `src/components/ui` for consistency.
- Manage fonts in `src/app/layout.tsx`; design tokens in `globals.css`. Avoid inline overrides.

## Component API Safety

- Always expose `className?: string` on top-level exported components and merge it via your class utility (e.g., `cn`) to preserve composition.
- Prefer explicit union variants over booleans for multi-state visuals (e.g., `tone: "default" | "success" | "destructive"`).
- shadcn/ui Select (Radix) rules:
  - Every `SelectItem` must have a non-empty `value` prop; never use `""`.
  - Do not add a placeholder as an item; use `<SelectValue placeholder="..." />` in the trigger.
  - Ensure `value`/`defaultValue` matches one of the item values; otherwise leave uncontrolled so the placeholder shows.

## Hydration & Build Health

- Avoid React hydration mismatches; gate client-only UI until hydrated and never access browser APIs at module scope.
- Respect `prefers-reduced-motion`; limit transitions to opacity/transform (never `transition: all`).
- Code must pass `npm run lint` and `tsc --noEmit` before commit.
