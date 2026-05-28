# Round 3 — Decisions & Tradeoffs

Date: 2026-05-28

Context: mobile UX was painful (long monotonous scroll, dense text stacks), and the lending lacked thematic visual identity beyond generic pulse dots. Round 3 attacks both without touching the desktop visual baseline.

## Architectural decisions

### Palette promoted to `:root`, switcher removed (commit 4A)
- `:root` previously held bio-crypto teal palette, but live site rendered royal-navy/coral via `applyPalette(storedPal || 'royal')` in JS.
- Promoted the royal palette values directly into `:root`. Updated `[data-theme="light"]` from bone+teal to royal-light (navy + coral on bone) for theme coherence.
- Removed the entire tweaks-panel surface (CSS + HTML + JS, ~250 lines).
- Rationale: the panel was a dev tool that leaked into production; user wanted a single locked direction. The 4 alternative palettes are still preserved in git history if needed.
- Risk: light theme reads differently now (bright navy/coral on bone vs muted teal/indigo before). Watch user feedback.

### Heartbeat tempo replaces independent pulse loops (commit 4C)
- Renamed `@keyframes pulse` → `heartbeat`; 5 existing usages updated. All 5 dots now share the same 1Hz cycle.
- Added a module-level rAF that writes `--hr-pulse` (0..1 sin) to `:root`; the new SVG waveform reads this var for opacity coupling.
- Rationale: synchronized "breathing" reads as biological/intentional vs the prior randomized pulse soup. Single rAF is cheaper than 5 independent CSS animations only if those animations were unsynchronized — kept the CSS keyframes for legacy elements, added rAF only for new ones.
- Fragile: if rAF doesn't fire (e.g. browser tab is backgrounded), `--hr-pulse` freezes. Acceptable degradation.

### Reduced motion is enforced at multiple layers (commit 4A + 4C)
- CSS layer: `@media (prefers-reduced-motion: reduce)` blanket-overrides animation-duration, iteration-count, transition-duration to ~0.
- JS layer: `const RM = matchMedia('(prefers-reduced-motion: reduce)').matches` is checked at the top of each animation IIFE; early-return prevents work from even starting.
- Rationale: CSS-only would still let rAF loops run wastefully. JS-only would not cover keyframe animations. Belt-and-suspenders.

### Mobile transforms are media-query-scoped, JS feature-detects (commit 4B)
- Every mobile layout change lives inside `@media (max-width: 768px)` or `(max-width: 900px)` for litepaper TOC.
- JS uses `matchMedia(...).matches` checks before binding mobile-only behavior (pipeline autoplay touch-pause).
- Rationale: zero risk of desktop regression. Designer can A/B by toggling viewport width alone.

### Desktop polish gated by `(hover: hover)` (commit 4D)
- Magnetic cursor and parallax bind to `matchMedia('(min-width:1024px) and (hover: hover)')`, not just min-width.
- Rationale: a hybrid iPad in tablet mode hits 1024px but is touch-first; cursor pull would be confusing without an actual cursor.

### A6 parallax uses CSS var fallback to preserve baseline
- `.phone` transform now uses `calc(8deg + var(--phone-rx, 0deg))`. When vars unset (initial load, reduced motion), it falls back to the original `rotateX(8deg) rotateY(-12deg) rotateZ(2deg)`.
- Rationale: zero static-state visual difference from baseline.

### E4 lattice canvas appended via JS, not inline markup
- `.pipe-arch` already has a `.ct` element (the FHE lattice representation). I append a `<canvas>` on top via JS rather than replace the static `.ct` — preserves fallback for reduced motion + mobile.

## Skipped from plan

- M6 progressive disclosure was not applied to litepaper.html paragraphs. Litepaper is a technical document where readers expect dense text; clamping would feel paternalistic. Index-side coverage (3 paragraphs in thesis + case) is sufficient.
- Mobile homomorphic-addition demo widget (out of scope per user choice).
- Capillary fill effect on `.src-bar` (out of scope per user choice).

## Known fragile areas

- The `theme-scramble-overlay` re-binds the `#themeToggle` click via `cloneNode + replaceChild`. The original handler (which only swapped `data-theme`) is replaced. If any other code attaches to `#themeToggle` later, it will be lost. Current code structure has no other listener attached, but worth knowing if the file evolves.
- The hero typewriter assumes h1 is a single readable text string. If h1 ever gains complex markup beyond `<span class="accent">`, the type-restore step will be wrong.
- Lattice canvas runs at ~30fps; on low-end hardware it may stutter. Could move to OffscreenCanvas if needed.

## Backup

Originals copied to `docs/round-3/index.html.bak` and `docs/round-3/litepaper.html.bak` before any edits in this round.

## File map of changes

- `index.html`: all 4 commits touched this file. Net delta: +636 / -319 lines.
- `litepaper.html`: commits 4A and 4B touched this. Net delta: +46 / -4 lines.
- `docs/round-3/`: created with backups + this doc.
