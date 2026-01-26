# VitaCrypt Landing Page — Definitive Build Prompt

You are an expert front-end developer building a premium SaaS landing page. Create a complete, single-file HTML landing page for **VitaCrypt** — a privacy-first personalized health app that integrates environmental (PM2.5, pollen, UV), genetic (MTHFR, APOE), microbiome, wearable (HRV, sleep), and lifestyle data to deliver evidence-based recommendations, using advanced encryption (FHE by Zama, zkSNARKs, Nillion blind computing).

## Tech Stack (must use exactly)
- HTML5 with semantic markup
- Tailwind CSS v3.4+ (via CDN: https://cdn.tailwindcss.com)
- Alpine.js v3 (via CDN) for mobile menu, form interactions, and animations
- AOS (Animate On Scroll) library v2.3+ for scroll-triggered animations
- Gsap (via CDN) for advanced hero animations and parallax effects
- Google Fonts: Inter (body), Outfit (headings)

## Brand Identity

**Primary Tagline:** "Your Health, Fully Encrypted. Truly Personalized."

**Color Palette:**
- Primary: Teal/Emerald (#0D9488 - hover: #059669)
- Secondary: Deep Purple/Blue (#6366F1 - hover: #4F46E5)
- Accent: Coral (#FF6B6B - hover: #EE5A52)
- Neutral: Dark (#0F172A), Light (#F8FAFC), Gray (#64748B)
- Gradient primary: from #0D9488 to #10B981 (emerald flow)

**Typography:**
- Headings: Outfit, bold (700), sizes H1:56px, H2:42px, H3:28px
- Body: Inter, regular (400)/medium (500), 16px baseline
- Tone: Premium, scientific, trustworthy, calm — like Linear + Stripe + Mercury

**Visual Effects Framework:**
- Smooth micro-interactions (150ms ease-out)
- Subtle parallax on hero background
- Staggered fade-up animations on scroll (AOS)
- Hover state elevation with shadow transitions
- Glassmorphic cards with backdrop-blur on feature sections
- Animated gradient backgrounds (gentle pulse/shift)
- Floating/breathing animations on icons
- Smooth page transitions and scroll behavior

## Technical Requirements
- Fully responsive (mobile-first design)
- Lighthouse targets: Performance 95+, Accessibility 100, Best Practices 100, SEO 100
- Semantic HTML, proper heading hierarchy (H1 → H6)
- All images lazy-loaded with loading="lazy" and descriptive alt text
- Meta tags for SEO (title, description, canonical, OG tags for social)
- WCAG 2.1 AA compliant (contrast 4.5:1, focus states visible, ARIA labels where needed)
- Smooth scroll-behavior enabled site-wide
- Touch-friendly (48px min tap targets)

---

## Section-by-Section Content & Visual Specs

### 1. Hero Section
**Visual Style:**
- Full-screen (min-height: 100vh) dark background (#0F172A)
- Animated gradient background: subtle shift between #0F172A → #1A2F46 (gentle 4s loop)
- Parallax background element (floating abstract nodes/dots moving at 30% scroll rate)
- Semi-transparent overlay grid pattern (10px grid, 0.05 opacity)
- Center-aligned content with staggered fade-in animation

**Content:**
- **Headline:** "Your Health, Fully Encrypted. Truly Personalized."
  - Font: Outfit Bold 56px (64px on desktop)
  - Color: White with subtle glow effect (text-shadow: 0 0 30px rgba(13,148,136,0.3))
- **Subheadline:** "VitaCrypt combines your genetics, microbiome, environment, wearables, and lifestyle — all processed with Fully Homomorphic Encryption and zkSNARKs — to deliver private, evidence-based health recommendations."
  - Font: Inter Regular 20px (18px on mobile)
  - Color: #CBD5E1 (light gray)
  - Line-height: 1.6
- **CTA Buttons:**
  - Primary: "Join Waitlist" (Coral background, white text, 16px medium, hover: scale 1.05 + shadow)
  - Secondary: "Download Litepaper" (Transparent border teal, white text, hover: bg-teal/10)
  - Button size: 16px padding, 32px horizontal, 8px rounded
  - Spacing between: 16px
- **Hero Visuals:**
  - Optional right-side abstract data visualization (3-4 floating circles with connecting lines, animated at 2s interval, opacity pulse 0.3 → 0.8)
  - Mobile: Center-stacked, visualization below CTA

---

### 2. Problem Statement
**Visual Style:**
- Light background (#F8FAFC)
- Section padding: 80px (40px mobile)
- Heading centered with underline accent (4px teal, width 60px, centered below)

**Content:**
- **Headline:** "The Problem: Your Health Data is Scattered, Exposed, and Underutilized"
  - Font: Outfit Bold 42px
  - Color: #0F172A
- **Subheadline:** "Health apps fragment your data across silos. Privacy disappears. Recommendations stay generic. Environmental threats go unnoticed."
  - Font: Inter Medium 18px, #475569

**Pain Point Cards (4 columns on desktop, 2 on tablet, 1 on mobile):**
Each card: glassmorphic (bg-white/80 backdrop-blur, border-white/20), padding 24px, rounded 12px, shadow-md hover:shadow-lg transition

1. **"Fragmented Data Across Apps"**
   - Icon: 🔀 or scattered squares animation
   - Text: "Your genetic data lives in one app, wearable data in another, medical records in a third. No unified view of your health."
   - Visual: Icons fade-in on scroll

2. **"Privacy Risks with Every Upload"**
   - Icon: 🔓 or lock-breaking animation
   - Text: "Traditional apps store your unencrypted health data on vulnerable servers. Your genome, your microbiome — exposed."
   - Visual: Red warning pulse

3. **"Generic Advice That Misses the Mark"**
   - Icon: 🎯 off-target or confused person animation
   - Text: "Recommendations ignore your unique genetics, environment (PM2.5 in LA?), stress levels, and microbiome diversity."
   - Visual: Blurred target graphic

4. **"Environmental Threats Ignored"**
   - Icon: 🌍 or pollution waves animation
   - Text: "No alerts about tomorrow's air quality spike, pollen counts, or UV exposure — threats you could prepare for."
   - Visual: Animated wave or particle effect

---

### 3. How It Works
**Visual Style:**
- Dark background (#0F172A)
- Section padding: 80px
- Heading centered, white

**Content:**
- **Headline:** "Secure, Intelligent, Personalized"
  - Font: Outfit Bold 42px, white
- **Subheadline:** "Four steps from raw data to actionable insights, with privacy guaranteed at every layer."
  - Font: Inter Regular 18px, #CBD5E1

**4-Step Flow (Horizontal on desktop, vertical on mobile):**

**Step 1: Connect**
- **Visual:** Icon (plug symbol, animated rotation on hover)
- **Headline:** "Connect Your Sources"
- **Text:** "Securely link wearables (Apple Watch, Oura), genetic reports (23andMe, AncestryDNA), microbiome tests (Viome), medical records, and location data."
- **Animation:** Fade-up on scroll, staggered entrance (100ms delay)

**Step 2: Encrypt**
- **Visual:** Icon (lock with glow, pulse animation)
- **Headline:** "Encrypt at the Edge"
- **Text:** "Data processed with Fully Homomorphic Encryption (Zama Concrete ML), zkSNARKs, and Nillion blind computing. Never decrypted, never exposed."
- **Animation:** Icon glows on scroll trigger

**Step 3: Analyze**
- **Visual:** Icon (brain/neural net, animated branching lines)
- **Headline:** "AI Analyzes in Darkness"
- **Text:** "Encrypted data cross-referenced against 1M+ peer-reviewed studies (PubMed, Cochrane, Springer). Patterns emerge without exposing raw data."
- **Animation:** Lines branch and connect on scroll

**Step 4: Improve**
- **Visual:** Icon (trending arrow or heart health pulse, animated rise)
- **Headline:** "Receive Personalized Guidance"
- **Text:** "Real-time dashboard, alerts (high PM2.5 spike tomorrow?), tailored recommendations (Omega-3, probiotics, mindfulness), and progress tracking."
- **Animation:** Arrow rises or heart pulses on view

**Connecting Flow:**
- Desktop: Horizontal line connecting all 4 steps with arrow transitions
- Mobile: Vertical dotted line on left side
- Animated particle flow along line (directional movement)

**Architecture Diagram:**
- Below the 4 steps, center a large diagram representing the VitaCrypt IDA engine with lock icon, data inputs, and encrypted outputs.
- Subtle glow/shadow, connected by animated lines

---

### 4. Three Pillars (Data Integration)
**Visual Style:**
- Light background (#F8FAFC)
- Bento-style 3-column grid on desktop, stacked on mobile
- Padding: 80px (40px mobile)

**Content:**
- **Headline:** "Integrated Data for Deeper Insights"
  - Font: Outfit Bold 42px, #0F172A, centered
- **Subheadline:** "VitaCrypt doesn't just collect data. It synthesizes it."

**Pillar 1: Genetic Blueprint**
- **Card Visual:** Gradient bg from teal to emerald (#0D9488 to #10B981)
- **Icon:** DNA helix (animated double-helix rotation, 3s cycle)
- **Headline:** "Genetic Blueprint"
- **Text:** "Decode your DNA: MTHFR methylation issues? APOE genetic risk? Poor B12 absorption? VitaCrypt maps it and recommends accordingly."
- **Key Insight:** "96% accuracy encrypted DNA analysis without decryption"
- **Animation:** Icon rotates continuously on page load, spins faster on hover

**Pillar 2: Environmental Context**
- **Card Visual:** Gradient bg from indigo to purple (#6366F1 to #8B5CF6)
- **Icon:** Air quality/weather (animated PM2.5 particle cloud, particles rise/fall)
- **Headline:** "Environmental Context"
- **Text:** "Real-time PM2.5, pollen counts, UV exposure, and weather patterns from your location. Know when air quality spikes before it hits."
- **Key Insight:** "Alerts adapted to your genetic sensitivity profile"
- **Animation:** Particles animate up/down continuously, intensity pulses with data

**Pillar 3: Lifestyle & Microbiome**
- **Card Visual:** Gradient bg from coral to orange (#FF6B6B to #FB923C)
- **Icon:** Microbe/lifestyle (animated bacterial cells or interconnected nodes)
- **Headline:** "Lifestyle & Microbiome Harmony"
- **Text:** "Sleep, stress, diet, exercise, and gut microbiome diversity tracked together. See how your choices reshape your internal ecosystem."
- **Key Insight:** "Personalized recommendations for nutrient absorption based on YOUR microbiome"
- **Animation:** Nodes connect/disconnect in gentle pulse, showing interdependence

**Card Hover Effects:**
- Lift effect (transform: translateY(-8px))
- Shadow expansion
- Icon animation speed doubles
- Border glow appears

---

### 5. Privacy & Security (KEY DIFFERENTIATOR)
**Visual Style:**
- Dark background (#0F172A) with subtle grid overlay
- Centered content, max-width 900px
- Padding: 100px (60px mobile)

**Content:**
- **Headline:** "Privacy Isn't a Feature — It's the Foundation"
  - Font: Outfit Bold 42px, white
  - Color highlight: Teal on "foundation"
- **Subheadline:** "Every bit of your health data stays encrypted. Always. Proven by leading cryptography research."

**Three Trust Pillars (3-column on desktop, stacked mobile):**

**Column 1: Fully Homomorphic Encryption (Zama)**
- **Icon:** Lock with circuit pattern (animated glow pulse, 2s cycle)
- **Headline:** "Encrypted Computation"
- **Text:** "Zama's Concrete ML enables analysis directly on encrypted data. We never see your genes. We never see your microbiome. We deliver insights anyway."
- **Stat:** "96% accuracy on encrypted DNA ancestry analysis"
- **Visual:** Animated code snippet or formula behind semi-transparent overlay

**Column 2: Zero-Knowledge Proofs**
- **Icon:** Checkmark in circle with subtle shimmer
- **Headline:** "Verified Without Exposure"
- **Text:** "zkSNARKs prove your recommendations are valid without revealing your underlying data. We verify, you trust, your privacy remains."
- **Stat:** "Computations verified on-chain"
- **Visual:** Checkmark animates confirmation on scroll

**Column 3: Nillion Blind Computing**
- **Icon:** Network nodes with encryption symbol
- **Headline:** "Decentralized Processing"
- **Text:** "Health data processing distributed across secure nodes. No single entity can access raw data. No central point of failure."
- **Stat:** "Multi-party computation with mathematical guarantees"
- **Visual:** Nodes connect/disconnect showing distributed network

**Trust Badge Section (below pillars):**
- Large centered badge (200x200px) with layered logos (Zama, Nillion, zkSNARK symbol)
- Tagline below: "Military-Grade Encryption. Medical-Grade Privacy."

**Compliance Text:**
- Small centered text: "HIPAA Compliant • GDPR Ready • SOC 2 Type II Path"
- Font: Inter 14px, #94A3B8

---

### 6. User Journey: Meet David
**Visual Style:**
- Light background (#F8FAFC)
- Left-aligned narrative on desktop, stacked on mobile
- Right side: illustrated avatar + journey visualization
- Padding: 80px

**Content:**
- **Headline:** "Meet David — 25, Los Angeles"
  - Font: Outfit Bold 42px
- **Intro Text:** "David faces unique health challenges shaped by his environment, genetics, and lifestyle. His story shows how VitaCrypt transforms scattered data into actionable insights."

**The Challenge (Left column, dark card background #1A2F46):**
- **Section Headline:** "The Challenge"
- **Bullet points:**
  - 🌍 High PM2.5 exposure in LA (frequent air quality alerts)
  - 🧬 Genetic predisposition: MTHFR mutation (poor B vitamin absorption), APOE risk (cardiovascular sensitivity)
  - 😰 Chronic stress → disrupted gut microbiome
  - ❌ Deficient in B vitamins, DHA, EPA (can't absorb even when supplemented)

**The Solution (Left column, teal card background #0D9488):**
- **Section Headline:** "VitaCrypt Reveals"
- **Bullet points:**
  - ✅ Personalized nutrient tracking highlighting deficiencies
  - 🎯 Real-time environmental alerts (PM2.5 spike tomorrow → stay indoors)
  - 🧪 Genetic analysis shows B vitamin absorption barrier
  - 📊 Microbiome data reveals stress-induced dysbiosis

**The Outcome (Left column, coral card background #FF6B6B):**
- **Section Headline:** "The Results"
- **Metrics:**
  - ⚡ Energy levels improved 40% in 6 weeks
  - 📉 Inflammation markers down 25%
  - 💤 Sleep quality improved (HRV normalized)
  - 🧠 Brain fog reduced (better nutrient absorption)
- **Quote:** *"For the first time, I understand WHY my body works the way it does. VitaCrypt didn't just give me recommendations—it gave me answers."* — David

**Right Side Visual (on desktop):**
- Avatar circle with initials "D" (animated pulse or subtle breathing effect)
- Below: Timeline/journey visualization (Challenge → Progress → Result)

---

### 7. Features Grid (Bento Layout)
**Visual Style:**
- Dark background (#0F172A)
- Irregular Bento grid with mix of 1x1, 2x1, 1x2, and 2x2 boxes
- Glassmorphic cards: bg-white/10, backdrop-blur-md, border-white/10
- Padding: 80px

**Content:**
- **Headline:** "Intelligent Features, Built for Real Health"
  - Font: Outfit Bold 42px, white

**Feature Cards:**
1. **Personalized Dashboard (2x2)**: "Your Health at a Glance" - Central hub for all insights.
2. **Real-Time Alerts (2x1)**: "Environmental & Health Alerts" - Advance warnings for PM2.5, pollen.
3. **Recommendations Engine (2x1)**: "Actionable Guidance" - Tailored supplements and lifestyle changes.
4. **Inflammation Tracking (1x1)**: "Inflammation Monitoring" - Track CRP, IL-6.
5. **Nutrient Intelligence (1x1)**: "Nutrient Gaps Decoded" - Supplements that work for YOUR genetics.
6. **Privacy Badge (1x1)**: "Privacy Verified" - Encrypted end-to-end.
7. **Action Insights (1x2)**: "Continuous Improvement" - Weekly progress tracking.

---

### 8. Science & Research
**Visual Style:**
- Light background (#F8FAFC)
- Max-width: 1000px, centered
- Padding: 80px

**Content:**
- **Headline:** "Backed by Peer-Reviewed Science"
  - Font: Outfit Bold 42px, #0F172A
- **Subheadline:** "Every recommendation is cross-referenced against the world's largest health research database."

**Research Sources (Cards):** PubMed, Cochrane Library, Springer & Elsevier, Scopus.

**Research-Backed Claims:**
1. "MTHFR Mutations Affect B Vitamin Metabolism" (PubMed)
2. "Microbiome Diversity Correlates with Mental Health" (Cochrane)
3. "PM2.5 Exposure Linked to Cardiovascular Risk" (WHO)

---

### 9. Impact & Benefits
**Visual Style:**
- Dark background (#0F172A)
- Section split: Left (text), Right (visual)
- Padding: 80px

**Content:**
- **Headline:** "Proactive Health, Longer Life"
  - Font: Outfit Bold 42px, white

**Impact Pillars:**
1. **Disease Prevention**: Identify and mitigate risks early.
2. **Personalized Longevity**: Healthier for longer based on genetics.
3. **Mental Health & Resilience**: Optimize sleep, stress, gut-brain axis.
4. **Environmental Resilience**: Prepare for pollution and allergens.
5. **Continuous Empowerment**: Your health data as a competitive advantage.

---

### 10. CTA Section
**Visual Style:**
- Gradient background: teal (#0D9488) to emerald (#10B981)
- Centered content, full width
- Padding: 120px (60px mobile)

**Content:**
- **Headline:** "Be Among the First to Own Your Health"
- **Subheadline:** "VitaCrypt MVP launching Q1 2026. Join thousands reclaiming privacy and personalization."
- **Waitlist Form:** Email input + "Join Waitlist" button (Coral).
- **Litepaper Link:** "Download Full Litepaper (PDF)"

---

### 11. FAQ (Accordion)
**Visual Style:**
- Light background (#F8FAFC)
- Max-width: 800px, centered
- Padding: 80px (40px mobile)

**FAQs:**
1. **How is my data actually encrypted?** (FHE via Zama)
2. **What data can VitaCrypt analyze?** (Genetics, microbiome, wearables)
3. **Is VitaCrypt HIPAA compliant?** (Yes, HIPAA, GDPR, SOC 2)
4. **Who can see my health data?** (Only you)
5. **How long does analysis take?** (5-15 mins)
6. **Can I download my data?** (Yes, full portability)
7. **Is this a medical device?** (No, analytics tool)
8. **Pricing?** (Founding member pricing for waitlist)

---

### 12. Footer
**Visual Style:**
- Dark background (#0F172A)
- Padding: 60px
- 4-column layout + bottom bar

**Content:**
- **Brand**: Logo + tagline.
- **Product**: Features, Pricing, Roadmap, Litepaper.
- **Company**: About, Blog, Careers, Contact.
- **Legal/Social**: Privacy, TOS, Security. Icons: X, GitHub, LinkedIn.
- **Bottom**: © 2026 VitaCrypt. "Made with encryption ❤️ and privacy obsession."

---

## Global Visual Effects & Animations

**Scroll Effects:**
- AOS library on most sections (fade-up, zoom-in, fade-in-left/right)
- Parallax on hero background (30% scroll rate)
- Staggered animations: each card/element delays by 50-150ms
- Smooth scroll behavior across entire page

**Hover Effects:**
- Buttons: Scale 1.05 + shadow-lg (150ms ease-out)
- Cards: translateY(-8px) + shadow-lg (200ms ease-out)
- Icons: Color shift to accent + animation speed increase
- Links: Underline animation (width 0 → 100%, 300ms)

**Micro-interactions:**
- **Input Fields:** Focus ring expansion (teal glow) + label floats up
- **Accordion:** Smooth height transition with opaque fade-in
- **Buttons:** Active state 'press' effect (scale 0.98)
- **Scrollbar:** Custom thin scrollbar (teal thumb, dark track)
- **Selection:** Custom text selection color (bg-teal-500/30 text-white)
