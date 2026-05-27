# VitaCrypt landing — changes summary

**Bundle:** 2026-05-27 deploy
**Scope:** d-combined direction, post research-response integration

---

## 1. Hero stage — fully restructured

**Before:**
- Single iPhone mockup on the right side of the hero, surrounded by empty space.
- Two text annotations ("What you see" / "What our servers see · Mathematical noise") floating to the sides — no actual visual of ciphertext.

**After:**
- A two-column "stage" in the hero's right column.
  - **Left column** (200px wide) stacks two cards:
    - **Unified Profile** — header "ON DEVICE · LIVE", a `94/100` completion meter, "6 sources, synced 2m ago", six source rows with icons and animated fill bars (Genetics · 23andMe, Apple Watch, PurpleAir, LabCorp, Tiny Health, Daily check-in). Source bars animate from 0 → fill on first scroll-into-view.
    - **Vitacrypt servers see** — five rows of monospace hex (`9F3B C24E 7AD1 …`) that mutate 2–3 chars every ~320 ms; a teal scan-line gradient sweeps top→bottom every 3.2 s; footer reads "Mathematical noise · 0 bytes readable".
  - **Right column** holds the iPhone mockup (same content as before, slightly smaller — 310 max-width vs 460).
- Zero overlap on the phone screen. Verified at desktop widths ≥ 1280 px.

**Why:**
1. The "what our servers see" claim is now a visible thing instead of a text annotation.
2. The empty space next to the phone is now the most product-dense surface on the page (the unified profile shows the data-fabric story Paul flagged as the most valuable part of the product).

---

## 2. Avatars — replaced

The teal-gradient circle with a white "D" letter is gone in two places:
- **Case study** (section 05 · `#case`) — David's profile card.
- **Gallery banner** (section 06 · `#insights`) — David's identity row above the three cards.

**Replacement:** a minimal SVG portrait silhouette (head + shoulders) on a dark gradient circle with a subtle dotted-noise texture and a 1px teal-tinted ring. Privacy-themed (anonymized person), not a generic identicon.

---

## 3. Gallery section (06 · Insights) — full rewrite

**Before:**
- Three persona cards: David (Sunday), Elena (Tuesday), Marcus (Saturday).
- Each card contained an "Every other app says X · VitaCrypt says Y" comparison box.
- The cross-persona narrative was scattered.

**After:**
- One persona: David, across three different moments of the same week.
  - `Tue · 06:42` — Air × MTHFR × HRV (the existing scenario, with PM2.5 forecast + 5-MTHF protocol)
  - `Thu · 19:15` — Caffeine × CYP1A2 slow × sleep debt (his espresso clears 8h slower than average; cutoff + Mg glycinate + zone-2 only)
  - `Sun · 09:30` — Training × ApoE × ApoB (weekly reset; swap HIIT → zone-2 + heavy lifting; monounsaturated-fat-forward diet)
- A **single banner** above the cards carries the "every other app · same advice all week → VitaCrypt · re-prescribes daily" comparison. Inside each card the comparison boxes are removed entirely; the cards just show signals + action + outcome.
- A new **week strip** at the top of the section shows all three moments in one row, so the user can see the whole week at a glance before scrolling through the cards.
- Section title updated: "One person. Three different combinations. Three different prescriptions."

---

## 4. Research-driven copy fixes

Sourced from the `RESEARCH_RESPONSE.md` document Paul attached. One-line audit list:

| Location | Was | Now |
|---|---|---|
| Nav pill (`#pipeline` link) | `Live demo` | `Walkthrough` |
| Facts ribbon (1) | `5 · Layers` | `6 · Layers` (added survey) |
| Facts ribbon (2) | `FHE · Compute on ciphertext · Zama Concrete ML stack` | `TFHE · Compute on ciphertext · Zama Concrete ML target stack` |
| Facts ribbon (4) | `30M+ · Peer-reviewed studies cross-referenced per recommendation` | `37M+ · PubMed records + Cochrane reviews cross-referenced` |
| Hero subhead | "...analyzed under encryption, with insights only your device can open." | "...encrypted on your device under a key only you hold, analyzed on a blind server that never sees plaintext. Targeting the Zama Concrete ML / TFHE stack." |
| Thesis P2 | Generic FHE explainer. | Now names Apple shipping BFV on every iOS 18 device. |
| Case study P1 | "...carries a folate-cycle variant that handicaps inflammation clearance..." | "...carries an MTHFR C677T (TT) variant that blunts folate-to-5MTHF conversion (his homocysteine runs high on a 'normal' diet), and an APOE ε3/ε4 that makes him cardiovascularly sensitive to the air his commute pipes into his lungs." |
| Case study insight box | Generic MTHFR + PM2.5 + HRV. | Adds "vascular system your APOE 3/4 already taxes"; specifies *methylated* folate (5-MTHF). |
| Why now eyebrow + lede | "Three curves meet here. FHE went from theoretical to clinically viable..." | "Three curves met in the last eighteen months. Apple shipped BFV in iOS 18 / Zama hit 96% in ~300s on a single CPU / 23andMe finished teaching consumers that 'encrypt at rest' isn't a privacy story." |
| KPI #1 | `TAM · DIGITAL HEALTH · $280B · Grand View Research` | `FHE BENCHMARK · ~300s · Zama bounty #95, 1000 Genomes` |
| KPI #2 | `TAM · LONGEVITY · $610B · BofA` | `ENCRYPTED ACCURACY · 96% · Matches plaintext baseline · Zama public benchmark` |
| KPI #3 | `FHE THROUGHPUT · ~300s · Zama benchmark` | `23ANDME FALLOUT · 6.9M users · $30M settlement, £2.31M ICO fine, Chapter 11, sold $305M (2023–25)` |
| KPI #4 | `CORE STACK · Zama · Concrete ML — production FHE framework` | `CORE STACK · Zama · Concrete ML · TFHE · production FHE framework targeted for MVP` |
| Footer brand blurb | "Private health intelligence computed on encrypted biology. Targeting the Zama Concrete ML / TFHE stack." | Adds: "Encrypted on your device under a key only you hold; analyzed on a blind server." |
| Footer bottom line | `HIPAA-READY PATH · GDPR-COMPATIBLE · ZAMA CONCRETE ML` | `ARCHITECTURE DESIGNED FOR HIPAA/GDPR ALIGNMENT · TARGETING ZAMA CONCRETE ML · PRE-MVP` |

---

## 5. Litepaper updates

| Location | Change |
|---|---|
| Q02 (the 300s / 96% benchmark) | Was "Source verification in progress." Now **resolved** — directly cites Zama bounty #95 with submitters `soptq` and `alephzerox`, clarifies it's ancestry classification on the 1000 Genomes test set (not whole-genome variant calling). |
| Q04 (the 30M studies claim) | Updated to "PubMed ~37M indexed records + Cochrane Library ~4,500 systematic reviews," cites the NLM open-data portal. |
| Q05 (zkSNARKs) | Sharpened to "ornamental at MVP; FHE already gives correctness guarantee; zkSNARKs only matter if we need third-party verifiable proof-of-compute (V2+)." |
| Citation pipeline paragraph | "30M+ records" → "37M+ records." |
| Reference [3] | Was "Source pending verification (Q02)." Now a direct hyperlink to `github.com/zama-ai/bounty-and-grant-program/issues/95`. |

---

## 6. Audit checklist (all passing as of bundle date)

- [x] No console errors on landing or litepaper
- [x] No broken anchor links (`a[href^="#"]` → all targets exist)
- [x] All images load (logo in nav + footer)
- [x] Hero stage: zero overlap between profile/cipher cards and phone (programmatically verified)
- [x] Both David avatars have an SVG child, zero text content
- [x] Facts ribbon reads `6 · TFHE · 0 · 37M+`
- [x] Nav contains "Walkthrough" (not "Live demo")
- [x] Footer ends with "ARCHITECTURE DESIGNED FOR HIPAA/GDPR ALIGNMENT"
- [x] Pipeline tabs (4 stages) functional; mini-phone state updates
- [x] All 5 palettes apply (`--bg` and `--ink` CSS variables change per palette)
- [x] Theme toggle flips dark/light cleanly
- [x] Tweaks panel opens/closes; palette choice persists across reload
- [x] Ciphertext mutation loop running (rows update every ~320 ms)
- [x] Profile source-bar fill animation triggers on intersection observer

---

## 7. Items NOT included (await Paul's approval to add)

From `RESEARCH_RESPONSE.md` sections H1–H6 — the research surfaced more than fit in this round without bloating the page. Hold these for the next round:

1. **Pillar 3 · Live research processing** — a dedicated section showing the daily PubMed / OpenAlex / GWAS Catalog / ClinVar / OpenFDA watch. Research response calls this the most under-told differentiator. Would slot between "Data fabric" and "Pipeline."
2. **Terra API** mention — the credible answer to "how will you integrate 20 sources by Q3 2026?"
3. **PROMIS** as the named survey instrument — only consumer health product using NIH-validated subjective inputs.
4. **Updated competitor table** with Superpower ($30M Series A, April 2025), Function-v-Superpower lawsuit, Lifebit. The current Differentiation section is generic.
5. **Apple iOS 18 Swift HE** as a hero-adjacent stat (currently only mentioned mid-thesis).
