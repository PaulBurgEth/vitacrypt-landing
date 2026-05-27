# VitaCrypt — Round 2 changes summary

**Date:** 2026-05-27
**Scope:** Research-driven content additions on top of Round 1's d-combined redesign.
**Source plan:** `~/.claude/plans/1-misty-wren.md`
**Commits:** `187f75a` (2A) · `ce83ef8` (2B) · `ca3a4af` (2C) · `62681f5` (2D) · this commit (2E).

---

## What changed

### Landing (`index.html`)

**Hero — Batch 2A**
- New punch line above the detailed lead paragraph: *"Every wearable reads one signal. We read **all of them** — plus your DNA, your environment, and the latest science — without ever seeing your data."* (research §J2)
- Underlying detail paragraph kept.

**Section #fabric — Batch 2A**
- Lede names **Terra API** (wearable integration mechanism) and **PROMIS** (NIH-validated survey instrument).
- Sixth source card title `Lifestyle & survey` → `Lifestyle & PROMIS`; one source chip renamed `Mood survey` → `PROMIS short-form`.
- Facts ribbon label updated to `… environment · PROMIS survey`.
- Pipeline COLLECT stage JS summary mirrors the same updates.

**NEW Section #research — Batch 2C**
- Inserted between #fabric (02) and #pipeline. Eyebrow `03 · Live science`; downstream eyebrows renumbered 03→04 … 08→09.
- Header: *Live research watch — the part static apps don't do.*
- 8-corpus grid: PubMed · OpenAlex · GWAS Catalog · ClinVar · USPSTF · WHO · NICE · Cochrane. Each with one-line descriptor. Responsive grid (4 → 2 → 1 col).
- Architecture callout: vendor-free LLM disclosure. Says "*the model only reads public papers; your encrypted profile never reaches the model; profile-vs-paper matching happens under FHE — cryptographically, not by sending your data anywhere.*" Out-link to `litepaper.html#engine` for full detail.

**Section #diff — Batch 2A + 2B**
- Lede prepended with: *"Keep your Oura. Keep your Apple Watch. Keep your Whoop. We connect to all of them — and add what they can't see."* (research §J3)
- NEW `.diff-banner` panel between lede and 3-card grid: **"What only VitaCrypt adds — beyond your wearable"** with 4 bullets (DNA & genetics · Microbiome · Labs & hormones · Live environment) and a privacy-moat footnote citing *npj Digital Medicine* (2025).
- Stale "Five layers" → "Six layers" in two card bodies (sibling-instance fix).

**Footer — Batch 2A + 2E**
- Compliance line appended with `· HIPRA-AWARE` (Sen. Cassidy bill, Nov 4 2025).
- Footer Product list gained `Live research` link to `#research`.
- Meta description / OG description / Twitter description updated to include "live science (PubMed, GWAS, ClinVar)" — reflects the new Pillar 3 content.

### Litepaper (`litepaper.html`) — Batch 2D

- **NEW §05 — Head-to-head vs popular wearables.** Full 13-row comparison table (Oura Ring 4, Whoop 5.0 MG, Apple Watch S10/Ultra 2, Galaxy Ring, Eight Sleep Pod 4 vs VitaCrypt) + five-point analysis (scope, on-device-data, monetisation, HIPAA status, HIPRA tailwind). Closes with "we don't replace, we connect" positioning.
- **§06 engine — Research-agent (model layer) NEW h3.** Vendor-agnostic LLM architecture disclosure. Preferred posture: self-hosted open-weight (Llama 3.x / Mistral / Qwen class). Cites arXiv 2408.13833 (fine-tuned biomedical LLMs don't outperform frontier+RAG). Explicitly states "no production-grade FHE-compiled transformer exists today; Nillion (MPC) and Apple Swift HE (BFV primitives) are not single-user LLM substitutes."
- **§06 engine — Citation pipeline expanded** into full 14-source MVP menu (PubMed, OpenAlex, Semantic Scholar, Cochrane, GWAS Catalog, ClinVar, ClinGen, Open Targets, OpenFDA, USPSTF, WHO, NICE, CDC, NIH ODS) with license/cost + MVP status badges. Also fixes a pre-existing typo (`both. uery at indexing time`).
- **Section renumbering** to accommodate §05 insertion:
  - `engine` §05 → §06
  - `valid` §06 → §07
  - `open` §07 → §08
  - `refs` §08 → §09
- TOC updated; 4 body cross-refs to `§07` (all referring to Open Technical Questions) updated to `§08`.

### Infrastructure (`sitemap.xml`, `docs/`)

- `sitemap.xml` lastmod confirmed at `2026-05-27` for both URLs.
- Bundle docs reorganized into `docs/`:
  - `docs/round-1/CHANGES_SUMMARY.md` (Round 1 redesign)
  - `docs/round-1/DEPLOY_NOTES.md` (Round 1 deploy steps)
  - `docs/research/RESEARCH_RESPONSE.md` (source-of-truth research brief)
  - `docs/round-2/CHANGES_SUMMARY.md` (this file)

---

## What was deliberately CUT (research items not implemented)

| Item | Why cut |
|---|---|
| A5 · Apple iOS 18 Swift HE in hero | Already in thesis P2; stack mismatch (Apple BFV vs Zama TFHE); Apple-as-competitor framing risk |
| C3 as originally written · Naming OpenAI/Claude/Gemini on landing | Breaks privacy-first brand. Replaced by Batch 2C's vendor-free landing disclosure + Batch 2D's litepaper detail with self-hosted open-weight preference |
| D1 full 13-row table on landing | Too dense. Moved to litepaper §05 |
| B2 · 4-pillar full restructure | Audit confirmed Pillars 1/2/4 already covered in existing sections. Only Pillar 3 was missing — closed by Batch 2C. No restructure needed |
| C4 · Adaptive recommendation UX | Too vague to ship without product detail. V2 |
| 2A.6 · B4 "ALIGNMENT BY ARCHITECTURE — NO AUDITS CLAIMED" compliance tighten | Optional in plan; not applied. Footer reads `ARCHITECTURE DESIGNED FOR HIPAA/GDPR ALIGNMENT · HIPRA-AWARE · TARGETING ZAMA CONCRETE ML · PRE-MVP` — already strong without the extra phrasing |
| Top-nav addition for "Live research" | Kept nav minimal (5 items) per user constraint. Footer Product list got the link |

---

## Verification performed (all batches)

- `preview_eval` confirmed every inserted/changed element renders the exact expected text.
- `preview_console_logs level=warn` returned no errors after each batch.
- Sibling-instance grep for `survey`, `Live demo`, `Five layers`, `5 layers`, `5 data`, `30M`, and the J2 hero line — no unintended duplicates.
- DOM verification confirms new `#research` section sits between #fabric and #pipeline, all 8 corpora rendered, architecture out-link to `litepaper.html#engine` resolves.
- DOM verification confirms litepaper has 10 sections in correct ID order, TOC has all 9 numbered links in order, no OpenAI/Anthropic naming, arXiv + npj + HIPRA citations present.
- Mobile responsive layout verified at ~682px viewport (banner grid collapses, hero punch font clamps to 20px).
- All 5 palette swatches + dark/light theme inherit cleanly into the new section (CSS uses variables only).

---

## Deploy posture

All Round 2 commits land on `main` after the Round 1 checkpoint `prev/20260527-1605`. **Not pushed. Not deployed.** Await explicit user `push` or `vercel` command.

Rollback: each batch is a separate commit; any batch reverts cleanly with `git revert <sha>`.
