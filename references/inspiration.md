# Inspiration Reference — What Makes Apps Premium

Study these apps when you need to calibrate what "premium" feels like. For each, the specific design decisions that make them exceptional are noted.

---

## Tier 1: Benchmarks (The Best of the Best)

### Linear (linear.app)
**What makes it premium:**
- Monochromatic palette with ONE accent color — never competing colors
- Typography is extremely tight and purposeful — every pixel of spacing is intentional
- Keyboard shortcut indicators in every context menu (implies power, respects the user)
- Loading states are skeleton-first, never a spinner on content
- Command palette (Cmd+K) — the power move
- Transitions between views: fade + very slight scale, under 200ms
- Icons are custom, not from a generic library — they all share the same weight and style
- Empty states have personality but not noise
- Hover states use ultra-subtle bg tint (#ffffff08) — not heavy highlights

**Key principle:** Every element earns its place. Nothing decorative. Density that feels spacious.

---

### Things 3 (iOS)
**What makes it premium:**
- Fluid drag and drop with spring physics — feels like touching real objects
- Header transforms elegantly as you scroll (large title → small title)
- Task completion: the circular checkbox has a perfect spring animation + satisfying fill
- Whitespace is generous but NEVER feels empty — every gap serves the hierarchy
- Typography: uses SF Pro Rounded for a soft but structured feel
- Color use is extremely restrained — projects have colors but the UI chrome stays clean
- Empty inbox state: genuinely pleasant, not apologetic

**Key principle:** Feels handcrafted. Every animation was designed, not defaulted.

---

### Craft (craft.do)
**What makes it premium:**
- Block-level design with perfect vertical rhythm (every block is on a grid)
- Document pages have a satisfying tactile feel — shadow + slight texture
- The sidebar is a perfect dark panel with clean hierarchy
- Drag handles appear contextually — UI doesn't feel cluttered
- Share sheets and popovers feel native but elevated
- Image blocks resize with smooth spring feedback

**Key principle:** Content feels precious. The chrome recedes completely.

---

### Stripe Dashboard (iOS)
**What makes it premium:**
- Numbers are displayed with extreme precision — fonts chosen specifically for tabular data
- Charts are clean with no visual noise — gridlines are barely visible
- Status badges use very low-saturation tints (not bright pill badges)
- Every tap gives immediate response — the UI never feels frozen
- Navigation: drawer from left feels weighty and authoritative
- Revenue graph sparkline: tiny but perfectly readable

**Key principle:** Trust is communicated through precision. Not a pixel is wasted.

---

### Raycast (raycast.com)
**What makes it premium:**
- Blur + dark glass — the signature look that feels like native macOS done right
- Result rows: avatar/icon left, title center, shortcut hint right — perfect 3-column info hierarchy
- Search input: no border, no shadow — the whole window IS the search bar
- Selection state: purple tint, not a heavy box
- Loading: skeleton rows, matches the item layout exactly
- Keyboard-first design principle — hover is secondary to keyboard navigation

**Key principle:** Speed is a feature. The UI communicates that everything is instant.

---

## Tier 2: Strong Reference Points

### Vercel Dashboard
- Clean data tables with perfect hover states
- Monochrome color system with accent only for CTAs and status
- Activity timeline: dots on a line, each with a subtle shadow
- Mobile responsive without feeling like a degraded desktop version

### Apollo (GraphQL client)
- Syntax highlighting done beautifully — custom color tokens
- Response pane: tree view with collapse animations
- Dark-first design

### Superwall
- Bold typography in onboarding (paywall screens)
- Price presentation: the annual price highlighted, monthly price struck through
- Gradient meshes used correctly — behind hero content, not everywhere

### Fey (portfolio tracker)
- Chart tooltips: floated above chart, rounded, perfect shadow
- Asset row: logo left, name + ticker center, price right + daily change in colored text
- Historical chart fill: gradient below line, not solid color

---

## Anti-Benchmarks (What NOT to Do)

### Generic SaaS Dashboard Template
- Sidebar: gray-100 background, icons from Heroicons with the wrong weight
- Cards: white bg with border-gray-200, no elevation
- Table: stock shadcn table with default font sizes
- Charts: default Chart.js with no customization
- Buttons: the default blue with rounded-md radius
**Problem:** Could be any app. Has no identity.

### "AI-generated" Aesthetic
- Always purple-gradient-on-dark or purple-gradient-on-white
- Space Grotesk or Inter for everything
- Glowing cards with radial gradient glow
- Random floating particles or grid background
- 80px hero text with gradient text fill
**Problem:** This is the "I asked Claude to make a landing page" look. Instantly recognizable. Avoid completely.

### Vibe-Coded Mobile App
- Different border radius on every screen
- Mix of 14px and 18px body text for no reason
- Buttons that don't show any press state
- Hardcoded padding that's clipped on iPhone SE
- Status bar overlapping content on notch devices
**Problem:** Feels unfinished. Users won't trust it.

---

## Asking the Right Questions

When evaluating your own UI, ask:

1. **Would a designer at Linear be embarrassed by this?** — Calibrates for detail level
2. **Is every animation intentional, or did I just add bounce to everything?** — Calibrates for restraint
3. **If I screenshot this screen with the branding removed, does it still have a clear identity?** — Tests for distinctive design
4. **Would this feel native on iOS? On Android?** — Tests for platform respect
5. **What's the one thing a user will remember about this UI?** — Tests for memorability
