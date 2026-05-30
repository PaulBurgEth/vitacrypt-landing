# Decisions

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
