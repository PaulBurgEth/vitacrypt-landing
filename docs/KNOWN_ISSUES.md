# Known Issues / Open Follow-ups

_Last updated: 2026-06-07 (site repositioning + production deploy)._

## Open — needs a human / external action
- **OG social-card cache.** `img/og-preview.jpg` was regenerated and is live (origin + CDN),
  but X / LinkedIn / Telegram / Facebook cache OG images on their side. Existing share previews
  keep showing the old "Powered by FHE" card until each is re-scraped via its debugger
  (LinkedIn Post Inspector, FB Sharing Debugger, X Card Validator, Telegram @WebpageBot).
  New shares pick up the new card automatically.
- **`vitacrypt-placeholder` is a separate Vercel project.** `placeholder/index.html` had its
  Telegram fixed in-repo (`t.me/paul_burg`), but the project was **not** redeployed (unknown live
  domain). It also posts the form to `formsubmit.co/waitlist@vitacrypt.xyz` (a contact outside the
  verified set). If the placeholder is still in use, deploy it separately and decide on that email.
- **Git not pushed to remote/main.** This round is committed on branch `investor-page`
  (`3c63411`, `a7a09b6`, + the docs commit) but not pushed. Production is live via `vercel --prod`
  (CLI deploy), so deploy state is decoupled from the git remote until someone pushes/merges.

## Accepted / out of scope (intentional)
- **litepaper.html body em-dashes (~50).** Not converted — it's a dense technical DD document;
  only its `<title>`/meta and contacts were touched. Convert later if site-wide punctuation
  uniformity is wanted.
- **Internal prompt docs** (`ENHANCED_LANDING_PROMPT.md`, `LANDING_PAGE_PROMPT.md`,
  `docs/research/`) still use FHE-first language. Historical specs, not user-facing — left as-is.
- **"blind computing (Nillion)"** references in litepaper/platform are a legitimate roadmap
  mention (defense-in-depth), not an overclaim — kept on purpose.

## Pre-existing / structural (carried over)
- **Self-contained pages duplicate nav/footer/waitlist form** across index/investor/platform
  (litepaper has a minimal footer). They drift; a change to a shared block must be applied to all.
  Candidate fix: extract `assets/shared.css` + `assets/app.js` when a change next touches all pages.
- **Waitlist rate-limiting is best-effort** (in-memory per Vercel instance). Proper fix = Vercel KV.
- **Waitlist env vars are Production-scoped only** — Preview/Dev deploys error on the form until
  `TG_BOT_TOKEN` / `TG_CHAT_ID` are added to those scopes.
