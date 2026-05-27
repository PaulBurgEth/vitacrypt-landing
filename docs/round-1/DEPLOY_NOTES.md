# VitaCrypt — Deploy notes for local agent

**Bundle date:** 2026-05-27
**Source:** the `d-combined` direction from the VitaCrypt landing project, post-research-response integration.
**What's inside:**

```
deploy/
├── index.html          ← landing page (replaces your current root index.html)
├── litepaper.html      ← technical brief / litepaper (linked from "Read paper")
├── assets/
│   ├── vitacrypt-logo-square.jpg
│   └── vitacrypt-logo-rect.png
├── DEPLOY_NOTES.md     ← this file
└── CHANGES_SUMMARY.md  ← what changed vs. the previous deploy
```

Asset paths inside `index.html` / `litepaper.html` use relative paths (`assets/vitacrypt-logo-square.jpg`) — drop the bundle at the **project root** and they resolve correctly.

---

## Step-by-step deployment (give this to the local agent)

> **Task for the agent:** replace the public-facing VitaCrypt landing with the contents of this bundle. The user (Paul) will be reviewing visually after each step.

### 1. Back up the current site

```bash
cd /Users/paulburg/Vibe_coding/VItaCrypt_landing/
git status                       # confirm clean working tree, or stash
git checkout -b prev/$(date +%Y%m%d-%H%M) HEAD     # checkpoint branch
git checkout -                                      # back to main/working branch
```

### 2. Drop in the new files

Unzip the bundle and copy:

```bash
cp deploy/index.html      ./index.html
cp deploy/litepaper.html  ./litepaper.html
mkdir -p ./assets
cp deploy/assets/*        ./assets/
```

If your project keeps the litepaper at a different path (e.g. `/paper/index.html`), update the **two** references inside `index.html`:

```html
<a href="litepaper.html">Read paper</a>
<a href="litepaper.html" class="btn btn-ghost">Read the litepaper</a>
```

…and the litepaper's "back to landing" link inside `litepaper.html`.

### 3. Smoke-test locally

```bash
npx serve .         # or python3 -m http.server 8080
```

Open `http://localhost:3000` (or `:8080`) and confirm:

- [ ] **Hero**: phone is visible on the right; "Unified profile" card + "Vitacrypt servers see" ciphertext card stack on its left. No overlap on the phone screen.
- [ ] **Ciphertext card** shows hex rows mutating every few hundred ms with a teal scan line sweeping top→bottom.
- [ ] **Facts ribbon** reads `6 · TFHE · 0 · 37M+` (NOT `5 · FHE · 0 · 30M+`).
- [ ] **Pipeline section** (section 03) — click each of the four tabs (`01 COLLECT` → `04 ACT`); the architecture diagram + mini-phone state respond.
- [ ] **Case study** (David, section 05) — avatar is a minimal SVG portrait silhouette, NOT a teal blob with a "D" letter. First paragraph mentions `MTHFR C677T (TT)` and `APOE ε3/ε4`.
- [ ] **Gallery** (section 06) — three cards titled `Tue · 06:42`, `Thu · 19:15`, `Sun · 09:30`. All three cards reference David, not three different personas. No "Every other app" comparison boxes inside each card (that lives in a single banner above).
- [ ] **Why now** (section 08) — KPIs: `FHE BENCHMARK ~300s`, `ENCRYPTED ACCURACY 96%`, `23ANDME FALLOUT 6.9M`, `CORE STACK Zama`.
- [ ] **Footer** ends with `ARCHITECTURE DESIGNED FOR HIPAA/GDPR ALIGNMENT · TARGETING ZAMA CONCRETE ML · PRE-MVP`.
- [ ] **Tweaks panel** (cog icon, bottom-right) — five palette swatches; each click re-skins the whole page; choice persists across reload.
- [ ] **Theme toggle** (moon icon in nav) — flips dark/light.
- [ ] No console errors on load or interaction.

### 4. Cross-browser pass

Recommended quick sweep:
- Chrome / Edge (Chromium) — primary
- Safari (macOS + iOS Simulator) — verify `backdrop-filter` and `aspect-ratio` (both shipped); verify `color-mix(in srgb, …)` (Safari 16.4+)
- Firefox — verify `-webkit-backdrop-filter` fallback shows blurred nav

The page targets evergreen browsers. If anything looks off in Safari < 16.4, the most likely culprit is `color-mix()` — a single-line polyfill is to add a fallback line above each `color-mix()` usage. The litepaper uses fewer of these than the landing.

### 5. Deploy

Whatever your existing deploy command is (`vercel deploy`, `netlify deploy --prod`, `git push origin main` if Netlify/Vercel auto-builds, `rsync`, etc.) — no special build step is needed. The bundle is static HTML/CSS/JS with no toolchain.

---

## What changed vs. previous deploy

Full detail in `CHANGES_SUMMARY.md`. One-line list:

1. **Hero stage redesigned.** Phone now has a sibling "Unified profile" card (showing the 6 source streams + 94/100 completion bar) and a "Vitacrypt servers see" ciphertext card (live-mutating hex with scan line). Cards sit cleanly to the left of the phone — no overlap.
2. **Avatars replaced.** The teal-gradient "D" letter is gone in both the case study and the gallery banner; replaced by an SVG portrait silhouette on a subtle dotted-noise dark circle.
3. **Gallery rewritten.** Was three personas (David / Elena / Marcus) with awkward "Every other app" comparison boxes inside each card. Now one person (David) across three different moments of the same week (`Tue 06:42` / `Thu 19:15` / `Sun 09:30`) — three different signal combinations, three different prescriptions. The "vs every other app" line is absorbed into a single banner above the cards.
4. **Research-driven copy fixes** (full source map below):
   - `"30M+ peer-reviewed studies"` → `"37M+ PubMed records + Cochrane reviews"` (per NLM open-data portal)
   - `"5 layers"` → `"6 layers"` (added PROMIS-grade survey as the sixth source)
   - `"FHE"` badge → `"TFHE"` with `"Zama Concrete ML target stack"` attribution
   - `"Live demo"` nav pill → `"Walkthrough"`
   - Hero subhead now explicitly says: *encrypted on your device under a key only you hold, analyzed on a blind server that never sees plaintext. Targeting the Zama Concrete ML / TFHE stack.*
   - Thesis P2 now names Apple shipping BFV on every iOS 18 device as the "this is consumer-grade now" proof point.
   - David case study lead-genetic story rewritten to MTHFR C677T (TT) → blunted folate-to-5MTHF + APOE ε3/ε4 → PM2.5 cardiovascular sensitivity. (The earlier vague "folate-cycle variant" framing is gone.)
   - Why-Now KPIs replaced. Was generic TAM stats; now: `FHE BENCHMARK ~300s` (Zama bounty #95), `ENCRYPTED ACCURACY 96%` (matches plaintext baseline on 1000 Genomes), `23ANDME FALLOUT 6.9M users / $30M settlement / £2.31M ICO fine / Chapter 11 / $305M sale`, `CORE STACK Zama`.
   - Footer bottom-line: `"HIPAA-READY PATH · GDPR-COMPATIBLE"` → `"ARCHITECTURE DESIGNED FOR HIPAA/GDPR ALIGNMENT · PRE-MVP"`.
5. **Litepaper Q&A section sharpened.** Q02 (300s/96%) resolved with a direct link to `github.com/zama-ai/bounty-and-grant-program/issues/95`. Q04 (30M vs 37M) corrected. Q05 (zkSNARKs) sharpened: ornamental at MVP, FHE already gives correctness guarantee. Reference [3] now cites the bounty submission directly instead of "source pending verification."

---

## Things deliberately NOT in this bundle (ask before adding)

Five items from the research response that would meaningfully improve the page but were left out to keep this deploy focused — talk to Paul before pulling them in:

1. **Pillar 3 · Live research processing** — a new section showing the daily PubMed / GWAS Catalog / ClinVar watch. The research response calls this the most under-told differentiator. Would slot between "Data fabric" and "Pipeline."
2. **Terra API** mention in the data-fabric section — credible answer to "how will you integrate 20 sources?"
3. **PROMIS** as the named survey instrument (NIH-developed, free, validated) — only consumer health product to use it.
4. **Competitor table refresh** — add Superpower ($30M Series A, April 2025), the Function-v-Superpower lawsuit, Lifebit. Drop generic "wearables / DNA / labs" silos.
5. **Apple iOS 18 Swift HE** namedrop in the hero/thesis area (currently only in thesis P2) — could be sharper as a hero-adjacent stat ("Apple shipped this same primitive on every iOS 18 device").

If/when Paul approves any of these, the litepaper sections H1–H6 of the research response have the copy ready.

---

## File-by-file source map

| File in bundle | Replaces | Lines touched vs previous |
|---|---|---|
| `index.html` | `/index.html` at project root | hero stage (full restructure), facts ribbon, thesis P2, nav pill, gallery section (full rewrite), case-study avatar + first paragraph + insight box, KPI grid (full replace), footer brand copy + bottom line, ~280 lines of new CSS at end of `<style>`, ~25 lines of new JS at end of `<script>` |
| `litepaper.html` | `/litepaper.html` (or wherever your litepaper lives) | Q&A section (Q02, Q04, Q05), citation pipeline paragraph (37M correction), reference [3] |
| `assets/vitacrypt-logo-square.jpg` | unchanged | unchanged |
| `assets/vitacrypt-logo-rect.png` | unchanged | unchanged |

No new dependencies. No build step. No backend changes. No environment variables.

---

## Rollback

If anything looks off post-deploy:

```bash
git checkout prev/<date-stamp>     # the backup branch from step 1
# OR
git revert <commit-of-this-deploy>
```

The bundle adds no migrations or persisted state — pure static-asset replacement.

---

## Questions

If the local agent is unsure about any of:
- Where exactly the existing `index.html` lives
- Whether the litepaper path needs updating
- Whether to commit the `prev/<date>` branch upstream
- A failed smoke-test bullet

…stop, ask Paul, do not improvise.
