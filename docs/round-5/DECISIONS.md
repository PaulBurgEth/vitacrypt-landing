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

## Round 5F — platform nav → litepaper-style TOC

Founder preferred the litepaper nav pattern (jump to exact section + return to
landing). Adopted on platform, executed cleaner than litepaper:
- Top tabs = the 4 pillars as IN-PAGE anchors (#unified/#evidence/#encrypted/
  #personalized) + "Read paper", in platform's pill-tab style (brand-consistent).
- Added an explicit "← Landing" back button (litepaper's `.back` affordance).
- Scroll-spy: active pillar highlights teal as you scroll — reuses the existing
  IntersectionObserver that drives the bottom mob-secnav; now also toggles the
  matching top tab (litepaper has no active state — this is the improvement).
- Moved `id="encrypted"` from a 0-height anchor span onto the section itself so
  the spy intersects reliably.

## Round 5G — honest status labels + clean drawer + system theme

Founder mobile review:
- **Theme follows the device now.** First visit with no stored choice reads
  `prefers-color-scheme`; manual toggle still wins + persists (index + platform).
  Litepaper is dark-only by design.
- **Mobile drawer (index) cleaned.** It had a duplicate "Platform" entry and
  gapped numbers (01,02,03,04,06,08,09 — 05/07 missing). Rebuilt as clean named
  links mirroring the top nav + a divider before the cross-page links. No numbers.
- **"Live" status labels removed** (solo builder, pre-MVP, nothing shipped):
  litepaper spec tables 9× "Live"→"MVP"; index fabric legend "Live API
  integration"→"Direct API · at MVP launch"; hero mock card "Live"→"Preview";
  roadmap "engine live"→"engine ships". (Small "· live" data-freshness strings
  remain inside the hero demo card, now labelled "Preview" — left intentionally.)
- **Duplicate-content scan** (index/platform/litepaper): no substantive
  duplication. Only shared markup is the waitlist form, footer, and JS (which
  should be identical). Platform pillar content is all net-new vs the landing.

## Session close — final state (2026-05-30)

Live: https://www.vitacrypt.xyz · /platform.html · /litepaper.html (all 200).
- Pages: `index.html` (hook landing + 4-pillar overview), `platform.html` (deep
  dive of the 4 pillars), `litepaper.html` (technical spec). `api/waitlist.js`
  serverless → Telegram.
- Waitlist form verified end-to-end in production (delivers to @paul_burg).
- Vercel env vars `TG_BOT_TOKEN` / `TG_CHAT_ID` set for **Production only** —
  Preview/Dev deployments will 500 on the form until those scopes are added.

### Updated known issues / debt (additions to the list above)
- **Env vars are Production-scope only.** Form works on the prod domain; a Vercel
  preview URL won't deliver until the vars are added to Preview/Dev.
- **Shared CSS/JS is duplicated across index/platform** (each page is
  self-contained, matching the litepaper pattern). A future shared `assets/
  shared.css` + `assets/app.js` extraction would remove the 3-way upkeep — the
  natural time is when the next big change touches all pages.
- **Litepaper `--ink-faint` (#515A86) still fails WCAG AA** (the 5C contrast fix
  only touched index/platform). Low traffic page; fix when convenient.
- Dead CSS remains on platform for components removed in 5E (`.pipe-tabs`,
  `.fabric-card`, etc.) — inert, kept in sync with index's stylesheet.
