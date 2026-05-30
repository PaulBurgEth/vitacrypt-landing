# VitaCrypt — Architecture (plain-English)

_Written for the owner. No deep coding knowledge assumed._

## 1. What this is
A marketing/investor website for **VitaCrypt** — a privacy-first personal health
platform that analyzes your biology under Fully Homomorphic Encryption (FHE), so
the server never sees your raw data. The site is **static HTML** (no framework,
no database) hosted on **Vercel**, with one small serverless function for the
waitlist form. Stage: pre-MVP, private beta targeted Q4 2026.

## 2. The pages (file map)
Everything lives at the repo root. Each page is **self-contained** (its own CSS
and JavaScript inline) — there is no shared stylesheet file yet.

| File | What it is |
|---|---|
| `index.html` | The landing page — the hook. Hero, a **4-pillar overview** block, the data-fabric/research/pipeline/case sections, roadmap, "why now", CTA. |
| `platform.html` | The deep-dive page — explains the **4 pillars** in depth (how each works, evidence, honest MVP-vs-roadmap scope). Links back to the landing for the live demo + case; links into the litepaper for specs. |
| `litepaper.html` | The technical brief — cryptography, threat model, integration spec, validation plan. Dark-only by design. |
| `api/waitlist.js` | Serverless function: receives the waitlist form and forwards it to Telegram. |
| `sitemap.xml`, `robots.txt`, `llms.txt`, `vercel.json` | SEO + hosting config. |
| `img/` | Logos + the social-share image (`og-preview.jpg`, 1200×630). |
| `docs/` | Internal notes (research findings, decisions, backups). **Excluded from deploy** via `.vercelignore` — not public. |

Navigation funnel: **landing (what it is) → platform (how it works) → litepaper (proof)**.

## 3. How the waitlist form works (the one moving part)
1. Visitor clicks "Join waitlist" → a modal form opens (name, email, audience,
   message, consent checkbox + a hidden anti-bot "honeypot" field).
2. The browser sends it to `POST /api/waitlist`.
3. `api/waitlist.js` validates everything, then calls the **Telegram Bot API** to
   message the founder's chat (`@paul_burg`).
4. Secrets live in **Vercel environment variables**, never in the code:
   - `TG_BOT_TOKEN` — the bot token (BotFather, `@paulburgcom_request_bot`)
   - `TG_CHAT_ID` — where messages land (`1250810291`)
   - ⚠️ Currently set for **Production only**. Preview/Dev deployments will error
     on the form until those scopes are added.

If the function ever fails, the form shows a fallback: "email marketing@vitacrypt.xyz".

## 4. External dependencies (and why)
- **Vercel** — hosting + the serverless function. Deploy with `vercel --prod`.
- **Telegram Bot API** — receives waitlist submissions (no database needed).
- **Google Fonts** — Geist, Geist Mono, Instrument Serif (loaded non-blocking).
- **Google Analytics 4** (`G-NGT34ZL8HV`) — traffic stats, IP anonymized.
- **GitHub** (`PaulBurgEth/vitacrypt-landing`) — source. Push to `main` is
  production-intent; Vercel is deployed manually (no auto-sync).

## 5. Non-obvious decisions (so future-you doesn't undo them)
- **Theme** = dark by default; on a visitor's first load it matches their
  device's light/dark setting (`prefers-color-scheme`); the toggle overrides and
  remembers the choice. Litepaper stays dark.
- **No "Live" status anywhere** — the product is pre-MVP. Data sources are labelled
  "MVP" / "Direct API at MVP launch" / "2027", never "Live". Keep it honest.
- **Self-contained pages** (CSS/JS duplicated per page) — chosen for simplicity
  and to match the existing litepaper. The cost is 3-way upkeep for shared bits
  (nav, footer, form). Extract to `assets/shared.css` + `assets/app.js` when a
  change next touches all pages.
- **`docs/` is deploy-excluded** — it holds internal positioning notes
  (`RESEARCH_FINDINGS.md`) that must not be public.

## 6. Fragile areas (touch with care)
- **Waitlist rate-limiting is best-effort** (in-memory, per Vercel instance). It
  blunts a naive loop but won't stop a distributed flood; worst case = spammed
  Telegram (rotate the bot token to recover). Proper fix = Vercel KV.
- **Cross-page anchor links** — the landing's "Explore →" cards and the platform's
  cross-links point at specific section IDs (`index.html#fabric`,
  `litepaper.html#threat`, etc.). If you rename a section's `id`, update the
  links that point to it.
- **The big inline `<style>`/`<script>` blocks** — editing one page's nav/footer/
  form does **not** update the others; change all three to stay consistent.
- **Roadmap dates + "MVP" honesty** — keep claims defensible (no "compliant",
  no "live", scope benchmarks honestly).

## 7. Day-to-day
- Edit an `.html` file → `git add` + `git commit` + `git push origin main` →
  `vercel --prod` to deploy → check `https://www.vitacrypt.xyz`.
- Backups of `index.html`/`litepaper.html` before this round's big changes are in
  `docs/round-5/*.bak`.
