# Round 5 — Decisions & Known Issues

## Round 5C (audit-driven hardening + mobile compaction)

Three parallel audit agents (SEO, performance/code, UX/a11y) reviewed the live
site. Findings actioned across commits 5C.1–5C.4.

### Decisions
- **Mobile compaction strategy.** Chose *progressive disclosure* (display:none
  accordions + carousel) over cutting content. Rationale: investors skim — let
  them see the 9-section skeleton fast and expand only what interests them.
  - Fabric (heaviest section) → horizontal scroll-snap carousel (matches the
    hero/pipeline pattern) instead of an accordion: it keeps all six sources
    visible while collapsing ~6 stacked cards into one swipeable row.
  - Reusable `.m-collapse` (`data-mcollapse="Label"`) uses `display:none`, not
    `max-height`. Why: collapsed elements with card chrome (padding/border) leak
    their padding box under `max-height:0`, and CSS-grid items don't collapse
    their track. `display:none` is bulletproof; the mFade keyframe covers the
    expand. Tradeoff: no collapse animation (acceptable; instant under reduced
    motion anyway).
  - Thesis `.thesis-grid` forced to `display:block` on mobile so `.right`
    collapses in normal flow (it was a grid item).
- **`--ink-faint` contrast.** Bumped dark `#515A86`→`#7A85B5` (2.93→5.43:1) and
  light `#8E8B7E`→`#6E6B5E` (2.98→4.67:1) to pass WCAG AA while staying muted.
- **Theme toggle.** Removed the `cloneNode/replaceChild` hack (it detached the
  node `tBtn`/`drawerThemeToggle` referenced, desyncing the drawer toggle and
  skipping the scramble). Single `toggleTheme()` handler now owns the scramble;
  overlay created once and reused.
- **Magnetic cursor.** Refactored from N global `mousemove` listeners (one per
  button, each doing `getBoundingClientRect` per move) to one rAF-throttled
  delegated listener that skips offscreen buttons. Hover-lift folded into a
  `--lift` custom prop so it composes with the magnetic transform.
- **Fonts.** Google Fonts switched from render-blocking `<link rel=stylesheet>`
  to `preload + onload swap` with `<noscript>` fallback.
- **Mobile paint.** Dropped `backdrop-filter: blur()` on the scroll-repainting
  fixed bars (`nav.top`, `.mob-secnav`) ≤768px; solid 94–96% bg instead.
- **CORS.** Moved from a static (www-only, mismatched) `vercel.json` header to a
  per-request origin allowlist (apex + www) in the function; OPTIONS → 204.
- **`.vercelignore`.** Excludes `docs/` (keeps internal `RESEARCH_FINDINGS.md`,
  `*.bak`, and prompt docs out of the public deploy), `placeholder/`, README.

### Dead code removed (0 markup refs, ~55 lines)
`.hero-device`, `.hero-annot` family, `.gal-card .contrast` block, stale
`.hero .dissolve` mobile-order rule. Plus the 7 Round-4 slop animations cut in
5B.1.

## KNOWN ISSUES / TECH DEBT

- **Waitlist rate limiting is best-effort only.** The in-memory sliding window
  (5 req / 10 min / IP) lives in module scope and is **per warm instance, not
  shared across Vercel's distributed runtime**. Verified in prod: a 7-request
  burst all returned 200 because Vercel routed them across instances. It blunts
  a naive single-instance loop but will NOT stop a distributed flood. Realistic
  worst case = founder's Telegram chat spammed (no data/cost catastrophe; bot
  token can be rotated instantly). **Proper fix:** Vercel KV / Upstash Redis for
  shared state — deferred (needs provisioning + a dependency). Revisit if abuse
  appears.
- **Heading hierarchy** still jumps h2→h4/h5 in a few sections (card headings).
  Flagged by SEO audit as a minor a11y/SEO lint. Not fixed because the design
  CSS targets those tags (`.fabric-card h4`, etc.); a correct fix means
  retagging + updating selectors. Low ROI vs risk; deferred.
- **Screenshot tooling** (preview MCP) returned blank frames intermittently this
  session; DOM/computed-style assertions + production curl smoke tests were used
  as the source of truth instead.

## Production verification (post-5C deploy)
- `GET /` → 200
- `OPTIONS /api/waitlist` → 204
- valid `POST` → `{"ok":true}` (Telegram delivered)
- missing consent → 400
- `GET /docs/round-5/RESEARCH_FINDINGS.md` → 404 (internal notes not deployed)
- `document.fonts.status` → "loaded"; hero h1 renders in Geist; no console errors

## Round 5E — platform.html corrected (deep-dive, not a copy)

5D v1 was wrong: it copied the whole landing into platform.html + a divergent
navbar. Rebuilt 5E as genuine раскрытие:
- Each pillar = NET-NEW content (mechanics + honest MVP-vs-roadmap scope +
  evidence with real citations), not the landing's components.
- Live demo / David case stay on the home page; platform links to them
  (index.html#pipeline, #case) — no duplication.
- Nav made identical to the main page (cross-page hrefs); mobile drawer keeps
  in-page pillar links.
- Removed the orphaned pipeline JS (stages/applyStage/startAutoplay/pipeObs +
  dead M4 swipe block) so there is no observe(null); other hero-bound JS already
  guards null.
- New scoped CSS: vc-steps, vc-table+badges, evidence-grading, citation sample,
  encryption-boundary diagram, evidence cards.
- DROPPED the earlier "trim main to teasers" idea — main never deeply covered
  the pillars, so there was nothing to move; main stays as the hook.
