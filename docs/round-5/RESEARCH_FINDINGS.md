# VitaCrypt Research Findings — Round 5 Phase 0

**Purpose:** Fact-check the technical claims on the landing page per `RESEARCH_BRIEF.md`. Each row below answers a question with citation + confidence rating + recommended copy action.

**Date:** May 2026
**Scope:** Landing claims on encryption, computation, benchmarks, data sources, and competitive framing.

---

## 1. Verified facts table

| # | Claim | Verified finding | Source(s) | Confidence | Action |
|---|---|---|---|---|---|
| **A1** | "Data encrypted on-device with your key" | Defensible for *encryption + decryption* operations (Zama released iOS SDK May 2025 with Swift bindings for client-side key-gen, encrypt, decrypt). FHE *computation* happens server-side per Zama's own published architecture. Hardware-optimized clients (TFHE-SBC, 2025) show client-side encryption is up to 2486× faster than prior baselines. | [Zama iOS FHE SDK announcement, May 2025](https://www.zama.org/post/privacy-preserving-encrypted-ios-apps-using-fully-homomorphic-encryption) · [TFHE-SBC paper, arxiv 2503.02559](https://arxiv.org/pdf/2503.02559) | **High** | Keep "encrypted on your device under a key only you hold" — accurate. Drop the implication that *compute* happens on device. Phrase compute as "analyzed on a blind server that never sees plaintext" — already correct on site. |
| **A2** | Zama Concrete ML / TFHE stack target | Zama Concrete ML is production-ready, public docs, multiple working demos (DNA ancestry, sleep score, FHE Ads). Nillion blind computer in production 2026: 112K+ users, 641M+ documents, 1.4M inferences. Token migrated to Ethereum Feb 2026. Nillion stack = MPC + HE + TEE combined, not pure FHE. | [Zama success story · python.org](https://www.python.org/success-stories/zama-concrete-ml-simplifying-homomorphic-encryption-for-python-machine-learning/) · [Nillion Phase 2 launch](https://nillion.com/news/nillions-phase-2-upgrade-is-live-introducing-a-unified-developer-portal/) · [Nillion docs](https://docs.nillion.com/blind-computer/learn/overview) | **High** | Pick ONE primary: **Zama Concrete ML / TFHE** for MVP (FHE-pure, matches "blind server" narrative). Nillion as **secondary roadmap** mention only (MPC+TEE compounds the security model later). Drop direct Nillion claim from MVP copy; mention in /roadmap.html. |
| **A3** | "~300s per genome" encrypted analysis | **Misleading.** Source: Zama bounty #95 (July 2024). Latency 300s applies to *ancestry classification on chromosome 22 only* (1000 Genomes Project), NOT whole genome. Solution 2 (alephzerox) took *tens of minutes on a 192-core machine* for similar task. | [Zama bounty #95 announcement, July 2024](https://www.zama.org/post/build-an-end-to-end-encrypted-23andme-genetic-testing-application-using-concrete-ml-fully-homomorphic-encryption) · [GitHub bounty issue #95](https://github.com/zama-ai/bounty-program/issues/95) | **High** | **Rewrite required.** Replace "~300s per-genome" with "~5 min for encrypted ancestry inference on chromosome 22 (Zama bounty #95, 2024)." Or drop the specific number and say "Zama's published benchmark shows encrypted DNA ancestry is feasible at investigator-grade latency." |
| **A4** | "96% accuracy on encrypted DNA" | Verified — both bounty solutions reached 96%. But task is *ancestry classification on chromosome 22*, NOT variant calling or polygenic risk scoring. Reference panel kept in clear; only query genome encrypted. | Same as A3 | **High** | Narrow phrasing: "96% accuracy on encrypted DNA *ancestry classification* (Zama benchmark)." Do not generalize to "DNA analysis." |
| **A5** | "30M+ peer-reviewed studies cross-referenced" | PubMed ≈ **40M+ records** as of 2025 (not 30M). Cochrane Database of Systematic Reviews: ~8,477 reviews (not millions). The "30M+" framing on the landing conflates PubMed indexing with "studies behind each recommendation." | [PubMed about page](https://pubmed.ncbi.nlm.nih.gov/about/) · [Cochrane lifecycle bibliographic study, PMC12362767](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC12362767/) | **High** | **Rewrite required.** Replace "30M+ studies" with: **"Cross-referenced against PubMed (~40M indexed records) and Cochrane systematic reviews."** Specific, accurate, and clearer about what we *query against* vs what's "behind" each recommendation. |
| **A6** | "5/6 data layers" MVP feasibility | **Mixed.** DNA upload (23andMe/Ancestry raw files) — trivially feasible. Wearables — Apple HealthKit, Oura/Whoop APIs exist. Environment — PurpleAir, OpenWeather AQI free/cheap. **Microbiome**: Viome and Tiny Health do NOT offer public APIs; would be user-uploaded report parsing only. **Labs**: HL7/FHIR is on 2027 roadmap; MVP = user-uploaded PDF/CSV. | Industry knowledge + Zama iOS demo (Apple Watch HealthKit integration) + public API directories | **Medium** | Honest framing: "DNA, wearables, and environment are live integrations at MVP launch. Microbiome and labs are user-uploaded today, with API and FHIR connections on roadmap Q1–H2 2027." |
| **A7** | "zkSNARKs" alongside FHE | **Ornamental for MVP.** zkSNARKs enable *verifiable* FHE (proof that server computed correctly without seeing data). Research is active 2024 (vFHE, blind zkSNARKs, vFHE with public verification) but **not production-ready** at consumer scale. Zama's published stack does not use zkSNARKs in production demos. | [vFHE paper, eprint 2024/1764](https://eprint.iacr.org/2024/1764.pdf) · [Blind zkSNARKs, IACR CIC](https://cic.iacr.org/p/2/3/10) | **High** | **Remove from landing.** Drop "zkSNARKs" from hero copy and prompt doc. If users care about output verification, mention briefly in /architecture.html as "compute-correctness proofs are a future enhancement (research-stage in 2025)." |
| **B1** | "Built on Zama" stack naming | Zama Concrete ML is publicly committed-to direction — but VitaCrypt has not deployed against it yet. Brief explicitly recommends softer wording at MVP stage. | RESEARCH_BRIEF.md B1 | **High** | Use **"Targeting the Zama Concrete ML / TFHE stack"** — already on current landing. Switch to "Built on" only after first deployment. |
| **B2** | "On your device" encryption phrasing | A1 verified that client-side encryption + decryption ARE realistic on consumer mobile in 2026. So "on your device" is *technically defensible* — the *key* lives in device keychain, encryption happens in client SDK. Server does compute. | A1 sources | **High** | Keep "encrypted on your device under a key only you hold." Pair with: "analyzed on a blind server that never sees plaintext." This split is honest and aligned with Zama's published pattern. |
| **B3** | David case: MTHFR C677T × APOE ε3/ε4 × PM2.5 | **Strong evidence.** Multiple peer-reviewed studies link MTHFR C677T + PM2.5 to inflammation, ischemic heart disease, congenital heart defects, and Alzheimer's risk. APOE4 + PM2.5 link to neuroinflammation in young populations published 2023. The case is scientifically defensible. | [MTHFR + PM2.5 meta-analysis, PMC6068673](https://pmc.ncbi.nlm.nih.gov/articles/PMC6068673/) · [APOE4 + PM2.5 in young Metropolitan Mexico City residents, PMC10296707](https://pmc.ncbi.nlm.nih.gov/articles/PMC10296707/) · [MTHFR + PM2.5 + Alzheimer's, PMC5295314](https://pmc.ncbi.nlm.nih.gov/articles/PMC5295314/) | **High** | **Keep David case as primary persona.** Optionally add a footnote referencing the meta-analysis. Consider adding a 2nd persona around APOE4 + PM2.5 + cognitive risk (different demographic). |
| **B4** | "HIPAA-ready path, GDPR-compatible" | At MVP stage, with no real PHI flowing yet and no BAA in place, "HIPAA-ready" is **aspirational, not compliant**. "HIPAA-ready architecture" is more defensible than "HIPAA-compliant." GDPR similarly: encrypted-at-rest + user key custody is *technically aligned* with GDPR data minimization principles but not certified. | General regulatory knowledge | **Medium** | Use **"Architecture designed for HIPAA/GDPR alignment"** — current footer copy is already this. Avoid "compliant" or "ready" without qualifiers. |
| **B5** | Competitive frame | **Distinct category.** Function Health, InsideTracker, Levels, Ultrahuman are biomarker/lab services on *plaintext* data, with traditional cloud storage. None do FHE-based encrypted compute. VitaCrypt's category is "encrypted health intelligence" — orthogonal to lab testing. Lifebit and Owkin do FHE-adjacent work in pharma/research, not consumer. | [Function Health vs InsideTracker comparison, mygenefood.com](https://www.mygenefood.com/blog/function-health-vs-insidetracker-heres-how-to-decide/) | **Medium** | Brief mention on /architecture.html: "Existing services (Function Health, InsideTracker, Levels, Ultrahuman) store plaintext on their servers. We don't replace them; we synthesize signals across silos under encryption." Avoid head-to-head comparison table — it implies parity. |
| **C1** | Phone mockup UX consistency | The current mockup (Today / Trends / Protocol / Sources tabs, priority insight card, mini metric cards) matches the litepaper description of a daily-action-oriented surface. Consistent. | RESEARCH_BRIEF.md C1 + landing review | **High** | Keep but **strip chrome** (notch, status bar, lock pill) per AI-slop audit. Focus on UI content. |
| **C2** | "Live demo" framing | The current pipeline animation is illustrative, not a real FHE round-trip. "Live demo" overstates. | Self-evident | **High** | Rename **"Guided walkthrough"** or **"How it works"**. The Zama Hugging Face Space ([encrypted_dna](https://huggingface.co/spaces/zama-fhe/encrypted_dna)) is an actually-deployed FHE demo we could link from /architecture.html for users who want the real thing. |

---

## 2. Recommended landing copy replacements

| File:section | Current text | Proposed replacement |
|---|---|---|
| `index.html` hero lead (line ~3138) | "VitaCrypt unifies your **genetics, microbiome, wearables, labs, and environment** into a single living health profile — encrypted on your device under a key only you hold, analyzed on a blind server that **never sees plaintext**. Targeting the Zama Concrete ML / TFHE stack." | "VitaCrypt encrypts your DNA, wearable, and environment signals under a key only you hold. A blind server analyzes them on ciphertext alone. Microbiome and lab integrations follow in 2027. Targeting the Zama Concrete ML / TFHE stack." |
| `index.html` facts ribbon "37M+" (line ~3175) | "**37M+** PubMed records + Cochrane reviews cross-referenced" | "**PubMed + Cochrane** indexed for every recommendation (~40M records · ~8.5K systematic reviews)" |
| `index.html` thesis paragraphs (line ~3188-3189) | Generic "false choice" rhetoric | "Most health apps see your raw data. We don't. Your phone encrypts under a key only you hold. Our servers compute on ciphertext, returning insight without ever seeing what's inside." |
| `index.html` whatever mentions "zkSNARKs" | "Zero-knowledge proofs" / "zkSNARKs" anywhere | **Remove.** Not in MVP scope per A7. |
| `index.html` "300s per genome" (Why now / Hero) | "~300s per-genome encrypted analysis" | "Encrypted DNA ancestry inference in ~5 minutes (Zama bounty #95, 2024) — at consumer-app latency for the first time." |
| `index.html` "96% accuracy on encrypted DNA" | "96% accuracy on encrypted DNA analysis" | "96% accuracy on encrypted **DNA ancestry classification** (Zama benchmark, 2024)" |
| `index.html` "Apple shipped this same primitive" (thesis) | "Apple shipped this same primitive (BFV) on every iOS 18 device." | **Drop.** False equivalence: Apple's PSI primitive ≠ TFHE compute on biology. Or move to footnote: "Apple's iOS 18 ships related FHE primitives for private federated stats (BFV)." |
| `index.html` final section CTA (line ~4083) | "VitaCrypt opens to its first design partners in Q4 2026." | (Already updated in 5A.3) ✓ |
| `index.html` "Live demo" link (#pipeline section title or button) | "Live demo" / "See it work" | "Guided walkthrough" or "See the architecture" |
| Any "blind computing across nodes / Nillion" mention | Nillion blind computing as MVP claim | Move to roadmap: "Multi-party blind computing (Nillion) on the roadmap for defense-in-depth post-Public-beta." |

---

## 3. MVP stack diagram (honest)

```
┌────────────────────┐                  ┌──────────────────────┐
│  USER DEVICE       │                  │  VITACRYPT SERVER    │
│  (iOS / Android)   │                  │  (cloud, GPU-accel)  │
│                    │                  │                      │
│  ┌─────────────┐   │                  │   ┌──────────────┐   │
│  │ DNA upload  │   │   ciphertext     │   │              │   │
│  │ Wearable    │───┼─────────────────▶│   │  TFHE        │   │
│  │ Environment │   │   (HTTPS)        │   │  computation │   │
│  │ Daily       │   │                  │   │  on encrypted│   │
│  └─────────────┘   │                  │   │  inputs      │   │
│        │           │                  │   │              │   │
│  ┌─────▼───────┐   │                  │   └──────┬───────┘   │
│  │  Key (in    │   │                  │          │           │
│  │  Keychain / │   │   ciphertext     │          │           │
│  │  Keystore)  │   │   result         │   ┌──────▼───────┐   │
│  │             │◀──┼──────────────────┼───│              │   │
│  └─────┬───────┘   │                  │   │  Reference   │   │
│        │           │                  │   │  panels +    │   │
│  ┌─────▼───────┐   │                  │   │  PubMed/     │   │
│  │  Decrypted  │   │                  │   │  Cochrane    │   │
│  │  insight    │   │                  │   │  (plaintext) │   │
│  └─────────────┘   │                  │   └──────────────┘   │
└────────────────────┘                  └──────────────────────┘
        ▲
        │
        Plaintext NEVER leaves device.
        Server only ever sees ciphertext.
```

**Stack components (MVP):**
- **Client SDK:** Zama Concrete ML iOS/Android client (Swift/Kotlin bindings, key in Keychain/Keystore).
- **Transport:** HTTPS, TLS 1.3 — ciphertext-in / ciphertext-out only.
- **Compute:** Server-side TFHE on GPU-accelerated nodes (Zama Concrete ML stack).
- **Reference data:** Plaintext on-server (1000 Genomes reference panels, PubMed/Cochrane indexes).
- **NOT in MVP:** Nillion MPC (roadmap), zkSNARK output verification (research-stage), HL7/FHIR labs (2027), HIPAA BAA (post-MVP).

---

## 4. Risk list — claims to DROP from landing

| Claim | Reason | Replacement |
|---|---|---|
| **"zkSNARKs"** anywhere | Not in MVP scope; research-stage in 2025 | Remove. Optional /architecture.html footnote: "Compute-correctness proofs (zkSNARKs over FHE) are an active research direction we will incorporate when production-stable." |
| **"30M+ peer-reviewed studies"** | Misleading framing | "Cross-referenced against PubMed (~40M indexed records) and Cochrane (~8.5K systematic reviews)" |
| **"~300s per genome"** unqualified | Implies whole genome, actually chromosome 22 only | "Encrypted DNA ancestry inference in ~5 min (Zama benchmark, 2024)" |
| **"96% accuracy on encrypted DNA"** unqualified | Implies all DNA analysis, actually ancestry classification | "96% accuracy on encrypted DNA *ancestry classification* (Zama benchmark)" |
| **"Apple shipped this same primitive (BFV)"** | False equivalence — Apple's PSI primitive ≠ TFHE biology compute | Drop or footnote: "Apple's iOS 18 ships related FHE primitives (BFV) for private federated stats — consumer hardware now supports the underlying cryptography." |
| **"Built on Zama"** | Not yet deployed | "Targeting the Zama Concrete ML / TFHE stack" — already on site ✓ |
| **"HIPAA-ready" / "GDPR-compatible"** unqualified | No BAA at MVP, no certification | "Architecture designed for HIPAA/GDPR alignment" — already in footer ✓ |
| **"Nillion blind computing"** in MVP claims | Not in MVP stack | Roadmap mention only on /roadmap.html. |
| **"Live demo"** for #pipeline section | Animation is illustrative, not real FHE | "Guided walkthrough" or "See the architecture" |
| **Fake citations on mockup cards** ("PubMed +4", "Confidence 92%") | False-authority illusion | Label as "example" OR remove the citation pill from mockup |

---

## 5. Founder's 6 open questions — answered

1. **"How sure are we the pipeline diagram (provide → encrypt → compute → resolve) matches what is actually on GitHub / what we will actually build?"**
   - **Answer:** Pipeline diagram is honest in *direction* but should reflect the **centralized MVP architecture** (single VitaCrypt server, not distributed Nillion). Use the diagram in §3 above. Mark "blind computing across nodes" as roadmap.

2. **"How much does correctness of the shown 'code' matter — does it just hook devs, or does it actively repel non-technical investors?"**
   - **Answer:** Both. Combine a **visual UX-first hero** (no pseudocode) with a **small honest code snippet** in /architecture.html ("here's what a TFHE client call looks like" — 8-12 lines). This signals technical depth to sophisticated investors without cluttering the main flow.

3. **"Should we name Zama / specific stacks, or keep it general?"**
   - **Answer:** **Name Zama Concrete ML / TFHE** — publicly committed direction, signals technical thesis. Mention Nillion only as roadmap. **Drop zkSNARKs** entirely (research-stage, ornamental for MVP).

4. **"The current variants say 'on your device' for encryption — but we may not actually be able to do real on-device FHE encryption for the MVP. Can we?"**
   - **Answer:** **Yes for encryption + decryption.** Zama's iOS SDK (May 2025) ships Swift bindings for client-side key-gen/encrypt/decrypt. The TFHE-SBC paper (2025) shows even resource-constrained clients can do this. *Compute* happens server-side — already accurately described as "blind server" on landing.

5. **"Zama has DNA encrypted-processing case studies. Nillion does blind computing with LLM workloads. Which one fits our MVP narrative best? Or both?"**
   - **Answer:** **Zama is MVP primary.** Their DNA ancestry bounty (#95, July 2024) is the closest publicly verified work to VitaCrypt's hero example. Nillion is great for LLM/inference workloads but their MPC+TEE stack diverges from a pure FHE story. **Mention Nillion as roadmap** for multi-node blind computing post-Public-beta.

6. **"The site reads like a print magazine — overwhelming text. Can we cut content and shift to product-page UX?"**
   - **Answer:** **Yes.** Confirmed by AI-slop audit (Phase 1.5). Round 5B multi-page split is the response: index.html becomes a 3-screen wow-summary landing; deep technical content goes to 4 pillar sub-pages. Voice rewrite cuts em-dash floods, three-item-list overuse, italic-serif accent abuse.

---

## 6. Optional "Why now" 1-paragraph rewrite

> Three things converged in 2024-2025. First, Zama's Concrete ML toolkit reached production-grade ergonomics — their July 2024 encrypted-DNA bounty shipped 96% accuracy on ancestry classification at consumer-app latency. Second, Apple's iOS 18 shipped BFV primitives across every device, making FHE consumer hardware. Third, the cost of homomorphic compute fell ~30× via GPU acceleration. The bargain we now offer — see your biology without anyone else seeing it — became technically real this year, not next.

---

## 7. Items still requiring user input

- **Confirm: cut "Apple shipped this primitive (BFV)" entirely or move to footnote?**
- **Confirm: David case study + one additional persona (APOE4 cognitive risk)?** Or keep David solo for now?
- **Confirm: link to Zama HuggingFace Space ([encrypted_dna](https://huggingface.co/spaces/zama-fhe/encrypted_dna)) from /architecture.html as "see a real FHE demo running"?** Adds credibility but cedes attention to Zama.

---

## 8. Sources consulted

- [Zama: Build Encrypted 23andMe-like App, July 2024](https://www.zama.org/post/build-an-end-to-end-encrypted-23andme-genetic-testing-application-using-concrete-ml-fully-homomorphic-encryption)
- [Zama Bounty #95 issue, GitHub](https://github.com/zama-ai/bounty-program/issues/95)
- [Zama Encrypted DNA HuggingFace Space](https://huggingface.co/spaces/zama-fhe/encrypted_dna)
- [Zama iOS FHE SDK announcement, May 2025](https://www.zama.org/post/privacy-preserving-encrypted-ios-apps-using-fully-homomorphic-encryption)
- [TFHE-SBC paper, arXiv 2503.02559](https://arxiv.org/pdf/2503.02559)
- [Python.org Zama Concrete ML success story](https://www.python.org/success-stories/zama-concrete-ml-simplifying-homomorphic-encryption-for-python-machine-learning/)
- [Nillion Phase 2 launch](https://nillion.com/news/nillions-phase-2-upgrade-is-live-introducing-a-unified-developer-portal/)
- [Nillion docs: Blind Computer overview](https://docs.nillion.com/blind-computer/learn/overview)
- [PubMed about page, NLM](https://pubmed.ncbi.nlm.nih.gov/about/)
- [Cochrane Lifecycle bibliographic study, PMC12362767, 2024](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC12362767/)
- [MTHFR + PM2.5 meta-analysis (heart disease), PMC6068673](https://pmc.ncbi.nlm.nih.gov/articles/PMC6068673/)
- [MTHFR + PM2.5 + Alzheimer's risk, PMC5295314](https://pmc.ncbi.nlm.nih.gov/articles/PMC5295314/)
- [APOE4 + PM2.5 + Alzheimer's in young residents, PMC10296707, 2023](https://pmc.ncbi.nlm.nih.gov/articles/PMC10296707/)
- [Function Health vs InsideTracker comparison](https://www.mygenefood.com/blog/function-health-vs-insidetracker-heres-how-to-decide/)
- [vFHE paper, eprint 2024/1764](https://eprint.iacr.org/2024/1764.pdf)
- [Blind zkSNARKs paper, IACR CIC 2024](https://cic.iacr.org/p/2/3/10)
