# Decisions

## 2026-06-04 — Investor page added (investor.html) + site-wide integration

**What/where:** New public page `investor.html`, plus integration edits in `index.html`,
`platform.html`, `litepaper.html`, `sitemap.xml`. Backups of the four touched-or-cloned
sources in `docs/round-6/*.bak`.

**Why:** The owner believed an investor page already existed (research had been done "on
that topic"). It did not — only scattered traces (a `mailto:investors@` footer link, a form
dropdown option, a schema ContactPoint, one CTA line). The prior research
(`docs/research/RESEARCH_RESPONSE.md`, `docs/round-5/RESEARCH_FINDINGS.md`) was about
*investor-grade messaging for the landing copy*, not a standalone page. So we built one.

**Approach — clone, don't re-author:** `investor.html` is a clone of `platform.html`'s full
shell (head boilerplate, the entire inline `<style>`, nav, mobile drawer, footer, mob-secnav,
waitlist modal, all `<script>` logic); only the body content between drawer and footer was
replaced. Guarantees pixel-identical design + theme/drawer/modal behavior with zero CSS
duplication risk. Verified the cloned script is null-guarded for the hero-specific bits it no
longer has (`#heroProfile`, `#cipherStream`, `.side-stack`), so nothing throws.

**Page structure (public teaser, gated deck):** hero thesis → `#thesis` opportunity →
`#why` market/why-now (reuses the `kpi-grid`: Zama 96% benchmark, Superpower $30M, 23andMe
fallout, HIPRA) → `#moat` four differentiators (`grade-row`) → `#traction` honest pre-MVP
table (`vc-table` + `badge` labels) → `#founder` → `#raise` → `#contact` final CTA.

**Honest framing decisions (owner is a solo founder, no formal round):**
- No empty "Team" section. `#founder` reframes solo-build as capital efficiency and names the
  first hires the raise unlocks (FHE/crypto eng, data engineering, data analysis/science) —
  turns "no team" into a signal that the founder knows what the money buys.
- `#raise` is deliberately soft: no amount, no valuation, no cap table, no term sheet. "Round
  shape is flexible; the deck/data room is shared on request." Matches the owner's reality.
- Kept the site honesty rules: no "Live" status (litepaper labelled "Published"), no
  "compliant", Zama named as a *target*. New copy avoids em dashes per owner's writing rule.

**Integration:** "Investors" added to the desktop nav + mobile drawer on index + platform;
nav link on litepaper (its dark shell has no column footer). "For investors" added to the
Resources footer column on index + platform (investor.html links out to platform instead).
New `#investors` block on the homepage between `#why` and the final CTA (reuses `kpi-grid`,
links to investor.html). The shared `openForm()` gained a ~3-line audience-preselect: a CTA
with `data-audience="investor"` opens the waitlist modal pre-set to the Investor role (added
to investor.html's own script copy only; index/platform homepage CTAs route to investor.html
instead of opening the modal, so they don't need it).

**Verified (local preview, port 3000):** no console errors; all 7 investor sections render;
no broken in-page anchors; scrollspy highlights correctly; light+dark theme parity; the hero
"Request the deck" CTA opens the modal with audience pre-set to "investor"; homepage block
renders with 3 KPIs + 2 CTAs; investor.html linked from index (5×), platform (3×), litepaper
(1×), sitemap (1×). Not deployed — `vercel --prod` left to the owner.

**Fragile / future:** `#founder` and `#raise` are placeholders for real numbers the owner may
later want (still no public figures by design). If section `id`s on investor.html are renamed,
update its nav/drawer/mob-secnav/footer anchors together. Em-dash-free copy on the new page is
a slight punctuation departure from the older pages (which use em dashes) — intentional.

## 2026-05-30 — Theme consistency round 2: "how it works" diagram + polish

Extends the inversion work to the `#pipeline` "How VitaCrypt works" diagram and two small polishes.
Files: `index.html` (renders the diagram) + `platform.html` (same CSS mirrored, but diagram markup
is dormant there — kept in sync).

- **Diagram device nodes invert** like the hero phones: `.arch-node .frame` background
  `var(--surface)`→**`var(--device-bg)`**; skeleton `.bar` →`color-mix(var(--device-ink) 14%)`;
  `.arch-node:not(.server) .frame` added to the scoped accent-override so in-frame `--teal`
  uses the opposite-theme accent. Result: "your device" reads the same in hero and diagram.
- **Server node de-bugged + reframed as infrastructure**: `.arch-node.server .frame` had a
  hardcoded dark gradient `#11161E→#080B11` (stuck dark on light theme). Now
  `linear-gradient(var(--surface), var(--surface-2))` — theme-matched, kept OUT of the device
  accent scope. Net metaphor: your devices always invert/pop; the blind server always recedes
  into the page (dark box on dark, light box on light).
- **theme-color meta follows the toggle** (Polish A): added `syncThemeColor()` called in
  `setTheme()` + once at init; flips the `<meta name=theme-color>` `#06080C`↔`#F4EFE5`. The
  in-page toggle uses data-theme/localStorage (not OS pref), so a CSS-media meta wouldn't work.
- **Primary CTA contrast** (Polish B): `html[data-theme="light"] .btn-glow { color:#FFFFFF }`.
  Fixed near-black label on `var(--teal)` dropped to ~3.8 contrast on light theme's darker teal;
  white ≈5.0 (AA). Arrow SVG uses `currentColor`, follows automatically. Dark theme unchanged.

Verified on fresh loads both themes (computed styles): dark → light device frames `#ECE6D9`
+ dark server `#121A3F`; light → dark device frames `#050818` + light server `#FFFFFF`; btn-glow
white on light / `rgb(2,17,13)` on dark; meta flips; 0 elements stuck at `#11161E`.

Trade-off: during the diagram's active-node animation, the active frame border is the device
accent while its label (on the page bg) is the page accent — both read as "active", acceptable.
Not verified by screenshot: the preview tool only renders the page top, so the mid-page diagram
was validated via computed-style probes, not a captured image.

## 2026-05-30 — Device mockups invert vs page theme

**What/where:** `index.html` `<style>` block. Added a device (phone) palette that
inverts relative to the page theme: dark page → light app screen, light page → dark
app screen. Previously every phone mockup used a fixed `--device-bg: #050818` plus
hardcoded light text (`#F4F5F2`, `rgba(244,245,242,…)`, `rgba(255,255,255,…)`), so the
app interface stayed dark in both themes.

Implementation:
- New CSS vars per theme block: `--device-bg`, `--device-frame-grad`, `--device-edge-1/2`,
  `--device-ink`, `--device-ink-rgb`, `--device-fill-rgb`. `:root` (dark page) holds the
  LIGHT device values; `html[data-theme="light"]` holds the DARK (original) values.
- Device accents (`--teal`, `--amber`, `--rose`) are re-scoped to the opposite theme's
  values via `.phone .screen, .pipe-phone .mini-screen, .case-device .mini-screen, .gal-screen`
  so accent text stays legible on the inverted background.
- All hardcoded device colors replaced with these vars. Applies to all 4 mockups:
  hero `.phone`, `.pipe-phone`, `.case-device`, `.gal-screen`.
- Side fixes (theme-match, not invert): `.arch-node .frame .content .bar` and the two
  avatar silhouettes (`.case-profile`, `.gallery-banner`) were fixed-light and vanished on
  the light theme; now use `color-mix(var(--ink), …)`.

**Alternatives considered:** CSS `filter: invert()` on the device (rejected — wrecks brand
colors); editing each hardcoded value inline without scoped accent override (rejected —
~50 edits and no clean accent handling).

**Trade-offs / fragile:**
- `.btn-glow` keeps fixed dark text `#02110D` on `var(--teal)`; slightly low contrast on the
  darker light-theme teal. Left as-is (intentional bright-button label, out of scope).
- Green-teal "encrypted" tints (`rgba(45,212,191,…)`) left literal; legible on both screens.
- **platform.html:** the same device CSS+vars fix was applied for stylesheet parity, BUT
  platform.html contains no phone markup in its `<body>` (all `.phone`/`.mini-screen`/`.gal-screen`
  references live only inside `<style>`). So the change is dormant there — no visible effect. Its
  own theme toggle works correctly (verified: light page renders light, zero stuck-dark elements).
  litepaper.html has no device mockups and no theme toggle.

---

## 2026-06-07 — investor.html + litepaper.html: reposition off FHE-first, "now vs target" honesty

**What changed (copy/emphasis only, no CSS/JS, no redesign):**
- **investor.html** reworked to match the updated investor one-pager (source of truth for
  positioning). Pillar hierarchy enforced everywhere: (1) unified holistic profile,
  (2) evidence-checked guidance, (3) actionable whole-profile insights, (4) private-by-design
  (FHE) as the trust layer, not the headline.
  - Hero, meta/title/OG/Twitter, Schema descriptions: lead with the unified profile; FHE demoted
    to a supporting clause.
  - §01 Opportunity now leads with fragmentation ("no one unifies it"); 23andMe breach moved to a
    supporting "Trust is the unlock" point. §02 KPI grid swapped to one-pager tiles
    (`6 in 1 / $30M / 6.9M / ~0.5s`); dropped the FHE-96% and HIPRA tiles. §03 Moat pillars
    reordered to the hierarchy; proof paragraph now leads with our own ~0.5s staging proof and
    cites Zama #95 (96%) only as external corroboration.
  - §04 "Traction" replaced with **"What works today (honest status)"**: an 8-row capability table
    (Genetics/Environment/Surveys/Recommendations/Health-score Live; Labs Wired; Wearables Partial;
    FHE Demonstrated) plus a **Now vs Target** mini-table.
  - §05 founder reframed (Paul Burg as founder/operator, per one-pager §05 v2; no paulburg.com About,
    no founding year, no past-company names, no services catalog, no Web3/ReFi). §06 raise aligned to
    one-pager use-of-funds + Q4 2026 milestone; **dollar/% figures deliberately kept off the public
    page** (brief instruction; they live in deck/data room).
- **litepaper.html** honesty patch (v0.1.0 → v0.1.1): §02 invariant relabeled "Target invariant"
  with a current-state note; stage-table Compute row shows target vs MVP; §01 device-bound custody
  reframed as target (server-side today); §05 vs-wearables overclaim cells softened (incl. the
  un-briefed "we don't hold the keys" cell, same boundary); new **Implementation status: now vs
  target** table after §02; §08 adds a resolved item confirming the one-slice staging blind-compute proof.

**Honesty boundaries enforced (both pages):** system never called "blind"/"zero-knowledge" as a whole
(master key server-side, LLM/OCR on plaintext); FHE model labelled synthetic tech demo, not a validated
PRS; Zama #95 only external evidence; no em-dashes in copy.

**Contacts consolidated to verified set:** `investors@vitacrypt.xyz`, `t.me/paul_burg`, `vitacrypt.xyz`.
Removed `press@` (user request) and `hello@`; Telegram `vita_crypt` → `paul_burg`. Left `marketing@`
in the waitlist-form plumbing and the X/Twitter social link (flagged, not part of the verified contact set).

**Out of scope / fragile:** index.html and platform.html still lead with FHE and contain the same
overclaim phrases ("blind server", "key only you hold", "without ever seeing your data"); their footers
now diverge from investor.html's cleaned contacts. Recommend a follow-up pass. Backups of both edited
files saved to `docs/round-7/*.bak`.

**Alternatives considered:** publishing the round figures on the public page (rejected per brief default,
confirm with Paul); switching all contact emails to office@paulburg.com (rejected — superseded by the
updated brief/one-pager which use investors@vitacrypt.xyz corporate).

---

## 2026-06-07 — index.html (home): reposition off FHE-first, "vision + honest tense"

**What & why:** the home page was the most FHE/blind-first surface on the site and carried the most
present-tense overclaims (the same ones removed from app/investor/litepaper), which both contradicted
the now-honest investor.html/litepaper and created a DD/legal risk for a trust product. Reworked to
lead with the unified profile + evidence (FHE as the trust layer), keeping the page ambitious and
forward-looking. Copy/emphasis only, no redesign, no CSS/JS structural changes.

**Calibration chosen (P1): "vision + honest tense"** (user decision). Did NOT downgrade the page to
MVP-reality. Instead: moved unprovable crypto/privacy guarantees from present-fact to design/future
tense ("by design", "built to", "target"); removed falsifiable stats ("0 bytes ever decrypted",
"End-to-end encrypted" badge, "FHE end-to-end", "we can't read it", "0 bytes readable", "never sent");
stated boldly the three things that are true today (unified profile live, evidence-cited recommendations
live, the ~0.5s one-slice staging proof). No now/target tables on the consumer page (kept it punchy).

**P2 reposition (unified-first):** title/meta/OG/Twitter/Schema lead with the profile; hero H1
"Your whole health, in one intelligent profile"; pillars retitled "One unified profile" and reordered
so Private is 4th; thesis #01 now leads with fragmentation; diff #05 reordered (Data scope first,
Encryption last); why-now #09 KPIs swapped to the one-pager set (6 in 1 / $30M / 6.9M / ~0.5s), with
Zama 96% kept only as external corroboration in the lede; the #04 architecture walkthrough reframed as
"how VitaCrypt is built to work" with one honest proof line (animation untouched); roadmap NOW node
updated to reflect the working product (4 of 6 streams + evidence live) without overpromising blind.

**P3 coherence/hygiene:** contacts consolidated to the verified set (investors@vitacrypt.xyz,
t.me/paul_burg) — removed press@ and hello@, Telegram vita_crypt→paul_burg, Schema contactPoint trimmed
to investors@; kept X and marketing@ (waitlist plumbing) by prior agreement. Em-dashes removed from all
visible copy (remaining ones are CSS/JS/HTML comments only). Investor teaser made coherent with the
updated investor.html.

**Verification:** completeness-gate grep clean (remaining hits are comments or explicit design/target
tense); tag balance OK (section/aside/footer/header 1:1); no console errors; all in-page anchors resolve;
desktop+mobile render confirmed (hero, pillars, diff, why-now, investor teaser, footer); theme toggle and
waitlist form intact. Backup at docs/round-7/index.html.*.bak.

**Out of scope / flagged:** platform.html still leads with FHE and carries the same overclaim phrases —
recommend a follow-up pass. Brand tagline "read by no one" was retired in favor of the profile headline
(can be restored as a small design-tense sub-line if desired).

---

## 2026-06-07 — platform.html reposition + site-wide shared-block coherence

**What & why:** final page (platform.html) was still FHE-first with present-tense overclaims, old
contacts, and pillar order ending on Personalized; litepaper still showed hello@. Brought the whole
site to one coherent state (positioning hierarchy, honesty boundaries, contacts, footer blurb).

**platform.html (vision+honest-tense, copy/emphasis; one block move):**
- Reposition unified-first: title/meta/OG/Twitter/Schema lead with the unified profile + evidence,
  FHE as enabler; hero H1 "Four pillars. One unified profile."; hero lead reordered (privacy last).
- **Pillar order changed to Unified → Cited → Personalized → Private** (Private last), matching the
  home-page pillar-card link order (#unified/#evidence/#personalized/#encrypted) and the hierarchy.
  The Personalized `<section>` was moved above the Encrypted `<section>`; **anchors (#personalized,
  #encrypted) preserved** so home links and scroll-spy still resolve. Renumbered Pillar 0X eyebrows,
  nav, drawer, mob-secnav, quick-jump; "Encrypted compute" relabelled "Private by design".
- Honesty retense: "our servers never see your plaintext", "never six plaintext feeds", "Plaintext
  never leaves the device", "matching happens under FHE / model never sees your genotype", "we compute
  without ever decrypting it / stays locked", "Plaintext never leaves here", "A breach yields
  mathematical noise", "evaluated … under FHE" → all moved to design/target tense ("built to",
  "Target:", "by design"). Private pillar now states the honest status plainly (blind compute proven
  on one slice ~0.5s, server-side master key today, synthetic FHE model not a PRS, production-wide +
  device-bound custody on roadmap; Zama #95 framed as external corroboration). Footer brand blurb,
  contacts (investors@ + t.me/paul_burg; removed hello@/press@; vita_crypt→paul_burg), Schema
  contactPoint trimmed to investors@. All visible-copy em-dashes removed (0 left).

**litepaper.html:** all hello@ → investors@vitacrypt.xyz (byline + CTA); `<title>` em-dash → "·".
Body em-dashes left (technical DD doc, out of scope by agreement).

**Coherent end state (all 4 pages):** contacts = investors@vitacrypt.xyz + t.me/paul_burg only
(no hello@/press@/vita_crypt anywhere); footer brand blurb identical on index/investor/platform;
unified-first titles/positioning; no present-tense whole-system blind claims; X + marketing@ kept.

**Verification:** site-wide overclaim gate clean (remaining hits all design/target tense); contacts
grep clean; platform anchors resolve and match index links; em-dash visible=0 (platform); tags
balanced + JSON-LD valid (platform, litepaper); no console errors; platform hero + pillar order +
footer render confirmed in browser; litepaper title/contacts/version confirmed. Backups in docs/round-7/.

**Still flagged (out of scope):** litepaper body em-dashes (~50, technical doc); secondary files
img/og-preview-template.html and placeholder/index.html (not public-facing).

### Extended sweep (non-page surfaces), same day
Audited every other repo surface for stale FHE-first / overclaims / old contacts and fixed the
public ones:
- **sitemap.xml:** image:titles updated to the new page titles (removed "Powered by FHE" + em-dashes);
  all `lastmod` bumped to 2026-06-07.
- **llms.txt** (AI-crawler description): rewritten unified-first with the honest privacy framing
  (blind compute proven on one slice; not yet end-to-end blind; master key server-side); contacts set.
- **img/og-preview-template.html** (source for the shared OG social card): headline → "Your whole
  health, in one intelligent profile", tagline + badges → Unified profile / Evidence-checked / Private
  by design. **NOTE: the deployed `img/og-preview.jpg` is a pre-rendered binary and must be
  regenerated (1200×630 screenshot of the template) for the change to show on social shares.**
- **README.md:** description + comparison table updated unified-first; removed the "Your data stays
  encrypted… even we can't see it" overclaim.
- **api/waitlist.js:** clean (uses marketing@ as the waitlist fallback, kept by design; the "—" in the
  Telegram notification is an internal team message, not site copy).

**Still flagged / not touched:**
- **`img/og-preview.jpg`** — binary needs regeneration to reflect the template (every social share
  currently still shows the FHE-first card).
- **placeholder/index.html** — a SEPARATE "Coming Soon" deploy (own `.vercel/`), still uses
  `t.me/vita_crypt` and posts to `formsubmit.co/waitlist@vitacrypt.xyz`. Left untouched pending
  confirmation it is still live / which domain it serves.
- **ENHANCED_LANDING_PROMPT.md, LANDING_PAGE_PROMPT.md, docs/research/** — internal historical
  spec/prompt docs, not user-facing; intentionally not edited.
- "blind computing (Nillion)" references in litepaper/platform are a legitimate roadmap mention, kept.

### Shipped to production (same day)
- Deployed `vitacrypt-landing` to Vercel production via `vercel --prod`; aliased to vitacrypt.xyz.
  Verified live: all four titles unified-first, "read by no one"/"0 bytes"/stale contacts gone,
  platform pillars 01 Unified / 02 Cited / 03 Personalized / 04 Private by design, sitemap titles updated.
- **OG image regenerated**: headless-Chrome render of the updated og-preview-template.html at 1200x630
  → img/og-preview.jpg (now "Your whole health, in one intelligent profile" + Unified/Evidence-checked/
  Private-by-design badges). Live at origin+CDN (102,685 bytes, replacing the old FHE-first 65,943-byte card).
  NOTE: social platforms (X/LinkedIn/Telegram/FB) cache OG images their side; existing previews need a
  re-scrape via each platform's debugger to refresh.
- Committed to branch investor-page (3c63411). Not pushed to main/remote (local rollback point).
- placeholder/index.html telegram fixed in-repo but the separate `vitacrypt-placeholder` project was NOT
  redeployed (unknown live domain) — deploy it separately if it is in use.

## 2026-06-07 — Source framing: drop the fixed count, "more is better / any health signal"

**What & why:** approach update — removed the numbers that cap how many health-data sources exist.
New principle: the unified profile is built to ingest **any** health signal and **grows over time**
(the more it holds, the better it reads the person), not a fixed set. Per re-sent investor + litepaper
briefs (Pillar 1 now adds "and more / any health signal, not a fixed set"; litepaper adds a "Source
framing (principle, not a fixed count)" note).

**Edits (copy/emphasis only, all 4 pages + llms.txt):**
- Removed every count-as-ceiling: "six streams / Six biological streams / 6 in 1 / 6 data layers /
  spans six streams / 4 of 6 / all 6 / Four of six / six layers / six plaintext feeds / six sources".
  Replaced with "any health signal / every signal / more is better / grows over time", keeping the
  plain factual source lists (genetics, wearables, labs, microbiome, environment, lifestyle, and more).
- KPI tile `UNIFICATION 6 in 1` → `All in 1` (index + investor); facts tile `6` → `All-in-1`;
  hero badge `6 data layers unified` → `Any signal, unified`; data-fabric H2 → "Every health signal.
  One queryable profile."; litepaper §04 H2 → "Health signals, unified into one profile." (+ table
  header Stream→Signal); both now-vs-target "Unified profile" rows → "core streams live / more
  signals, deeper coverage, any health data".
- Softened absolutes: "Nothing on the market / no current product unifies it" → "We have not found a
  product that unifies it this way".
- Demo-UI snapshot numbers (a demo profile's "6 sources synced", "Engineer · 6 sources", "+6" cites)
  left intact — they are factual demo state, not ceiling claims.

**Verified (local):** count-ceiling grep empty across all pages + llms.txt; source lists intact;
absolutes gone; KPI/facts values render with no overflow ("All-in-1" fits); em-dash visible copy 0;
contacts unchanged; tags balanced; JSON-LD valid; no console errors. Backups in docs/round-8/.

**NOT yet deployed.** Production (vitacrypt.xyz) still serves the prior "6 in 1 / six streams" copy
until a `vercel --prod` redeploy (pending explicit go). One-pager PDF reconciliation flagged: if the
public PDF still shows "6 in 1", align it to the same principle.

## 2026-06-10 — v2 brief pass: stage wording, AI-driven, private-compute hire, founder bio v2

**What & why:** applied the three v2 briefs (Investor Page Update Brief v2, Litepaper Patch Brief v2,
Sitewide Coherence Brief). The v1 work (unified-first repositioning, now/target blocks) was already
in place from 2026-06-07; this pass delivers the v2 deltas and residual cleanups.

**Edits:**
- Stage wording site-wide: every "pre-MVP" / "MVP in development" → "working product, pre-launch"
  (litepaper status: "Working MVP, pre-launch"). Hero badges, KPI tiles, footers, llms.txt,
  placeholder. The Q4 2026 date is now phrased "first design-partner cohort" everywhere
  (index roadmap node "Private beta" → "First design-partner cohort"). 2027 roadmap dates kept
  (Paul's call, consistent with litepaper §07).
- "AI-driven" added to pillar 2 / canonical one-liner across all meta descriptions, OG/Twitter
  tags, JSON-LD, hero lead-punches, investor moat card (now also names PubMed / Europe PMC /
  OpenAlex, publication-type grading, ClinVar / dbSNP / GWAS Catalog), llms.txt, README.
- Investor §05: hire /01 relabeled "Private-compute engineering" (old "FHE / cryptography engineer"
  label retired per v2) with senior-crypto-review + independent-audit wording; hire /02 dropped
  "on-device". Founder lede rewritten per v2 (SF 2019 origin, revived when FHE matured, tRWI,
  public goods, environmental engineering five-year specialist degree, postgraduate ecology
  research, longevity throughline, LinkedIn link). §06 milestone card gained the one allowed GTM
  sentence (biohacker/longevity communities, founding-member pricing).
- Residual overclaims cleaned on index: "You hold the only key" card → "Your data, your key, by
  design" with target-custody framing; data-fabric "pulled together on the device... before
  anything is encrypted" → target-architecture framing; hero lead + thesis retensed ("built to
  encrypt / built so"); privacy-moat footnote → demonstrated-on-one-slice pattern; walkthrough
  stage 02 strings prefixed "In the target design / Target design:". Hero profile card label
  "Unified profile · on device" → "Unified profile".
- investor §03 proof: "the server never seeing it" → "the server never holding it" (ciphertext is
  visible to the server; the key is not).
- Litepaper bumped v0.1.1 → v0.1.2 (7 spots), dateModified 2026-06-10; og/twitter descriptions
  de-overclaimed ("encrypted ... stack" → "privacy-first ... targeting Zama Concrete ML / TFHE").
- README: "Six streams" → "Any health signal"; "The only consumer app" → "We have not found
  another product"; em-dashes removed. placeholder: fabricated "10,000+ waitlist" + "Fully
  encrypted" dropped.
- Kept per decision: marketing@vitacrypt.xyz as the waitlist contact (Paul confirmed; Rule 6's ban
  targets paulburg.com branding); demo-UI snapshot numbers; Nillion roadmap mentions.

**Flagged, not changed:** litepaper §04 badges mark Wearables/Labs/Microbiome "MVP" while the
investor traction table says Partial/Wired — possible incoherence for the next litepaper iteration
(the v2 litepaper brief's edit list did not cover §04).

**Verified (local):** banned-phrase greps empty (pre-MVP, MVP in development, you hold the only key,
zero-knowledge, fully encrypted, FHE / cryptography engineer); 0.1.1 count 0, 0.1.2 count 7;
AI-driven present on all 4 pages + llms.txt + README; all 4 pages render via local serve with zero
console errors; pipeline walkthrough buttons work after the JS-string edits.

**Deployed to production 2026-06-10** (explicit go from Paul): `vercel --prod` →
`vitacrypt-landing-82y28u8nl` (dpl_2efNEGGSTfwnRqxTFbDsS6294FGM). Post-deploy verification via curl
against vitacrypt.xyz: banned-wording sweep empty on all 4 pages; "Private-compute engineering" +
8× "AI-driven" on /investor.html; litepaper serves v0.1.2 + "Working MVP, pre-launch"; meta
descriptions 148/148/158/168 chars; JSON-LD on all 4 pages parses as valid JSON. The OG image
binary did not change (no regeneration needed); og:description text did — link-preview re-scrape
per platform still pending (tracked in KNOWN_ISSUES.md). ARCHITECTURE.md synced (stage wording +
status-label guardrail updated from the retired "No Live status / pre-MVP" rule).

## 2026-06-11 — Delta pass: clinical framing (v2.2 / Rule 7) + revenue levers (v2.3)

**What & why:** Paul re-sent the investor-page and sitewide briefs; a formal diff confirmed the only
additions are the clinical-framing rule (roadmap framing only; "a clinical advisor joins ahead of
the IRB validation study planned for 2027"; no defensive wording) and the revenue-levers rule
(guided-data-acquisition / test-partnership / supplement commerce stay investor-material only;
the public site speaks only of the subscription).

**Edits (6):**
- index Labs & hormones: dropped "We don't resell tests." (Paul's call: the sentence actively
  foreclosed the test-partnership lever that now lives in the investor memo; the site stays silent
  instead) → "Bring your own results from Quest, Labcorp, Function, Superpower."
- index case study de-medicalized: "Three prescriptions" → "Three protocols";
  "re-prescribes daily" → "re-personalizes daily"; "changes the prescription" → "changes the
  protocol" (an unvalidated app must not imply it prescribes).
- Clinical-advisor roadmap line added where IRB framing is carried: index roadmap Q1 2027 node
  ("; a clinical advisor joins ahead of the study") and investor §04 honest-scope foot-note
  ("A clinical advisor joins ahead of the IRB validation study planned for 2027.").
- No defensive wording existed anywhere (verified); supplements appear only as product guidance /
  demo-UI (supplement log, 5-MTHF in the case study), not commerce — left intact. litepaper FDA
  mentions are factual (Apple Watch ECG cleared; OpenFDA API) — left intact.

**Out of scope, flagged:** litepaper §07 could carry the clinical-advisor line too, but no new
litepaper brief was issued and the sitewide brief excludes /litepaper — next litepaper iteration.

**Verified (local):** "clinical advisor" present on index + investor; resell/prescri greps empty;
full banlist regression grep empty; both pages render with zero console errors ("re-personalizes
daily" confirmed in DOM; hidden-by-default pill, hence checked via innerHTML).

**Deployed to production 2026-06-11** (explicit go from Paul): `vercel --prod` →
`vitacrypt-landing-rdw32mmly`. Post-deploy curl verification against vitacrypt.xyz: clinical-advisor
line present on index (1×) and investor (1×); resell/prescri greps empty; "Three protocols",
"re-personalizes daily" and "Bring your own results from Quest" all serving. Meta/OG tags were not
touched in this pass, so no link-preview re-scrape is needed for it (the 2026-06-10 re-scrape item
in KNOWN_ISSUES still stands on its own).
