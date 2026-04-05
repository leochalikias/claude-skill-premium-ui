---
name: premium-app-ui
description: >
  Enforce premium, production-grade UI quality for any interface — web, mobile (React Native/Expo), or artifact.
  Use this skill EVERY TIME you are building any UI, component, app, screen, dashboard, or interface — regardless of whether
  the user explicitly asks for "premium" or "high quality." This skill exists to permanently eliminate vibe-coded UI from
  all outputs. Triggers include: any React, React Native, Expo, HTML/CSS/JS, or UI-related request; phrases like "build me
  an app," "make a screen," "create a dashboard," "design a component," "mobile UI," "landing page," or any frontend work.
  If the user is building ANYTHING visual, this skill applies. Do NOT skip this skill and default to generic component library
  defaults — that is vibe-coding and it is unacceptable.
---

# Premium App UI Skill

This skill eliminates vibe-coded UI permanently. Every interface you produce must feel like it was designed by a senior product designer at a top-tier tech company (Linear, Vercel, Apple, Stripe, Craft, Things 3, Raycast). There are no exceptions.

---

## What is Vibe-Coded UI? (Never Do This)

Vibe-coding is slapping together UI without a system. It looks like:

- **Random spacing** — padding: 12px here, 17px there, margin: 23px somewhere else. No grid.
- **No type scale** — font sizes chosen by feel: 13px, 16px, 19px, 22px with no rhythm.
- **Inconsistent radii** — some cards rounded-md, some rounded-xl, some rounded-none, no logic.
- **Flat lifeless buttons** — a rectangle with a color and no states (hover, pressed, disabled, loading).
- **Missing states** — UI that only handles the happy path. No empty state, no error state, no loading skeleton.
- **Default library aesthetics** — MUI out-of-the-box, default shadcn with no customization, unstyled Tailwind.
- **Color chaos** — random hex values, no palette, no semantic tokens, no contrast checking.
- **Touch failures** — tap targets under 44pt on mobile, no haptic-feeling press animations.
- **Typography neglect** — system fonts, no letter-spacing consideration, no line-height system.
- **Zero micro-interactions** — everything is static. Nothing responds. Nothing breathes.

---

## The Design Token System (Always Apply)

Before writing a single line of UI code, establish tokens. These are non-negotiable.

### Spacing Scale (8pt Grid)
```
2   → 2px   (hairline)
4   → 4px   (xs — icon gaps, tight inline spacing)
8   → 8px   (sm — component internal padding)
12  → 12px  (md — default inner padding)
16  → 16px  (lg — card padding, section gaps)
24  → 24px  (xl — layout breathing room)
32  → 32px  (2xl — section separation)
48  → 48px  (3xl — hero spacing, large gaps)
64  → 64px  (4xl — page-level spacing)
```
Never use values outside this scale. When in doubt, use the next step up.

### Type Scale
```
xs:   11px / line-height 1.4 / tracking +0.02em  → labels, captions
sm:   13px / line-height 1.5 / tracking +0.01em  → secondary text, metadata
base: 15px / line-height 1.6 / tracking 0        → body text
md:   17px / line-height 1.5 / tracking -0.01em  → emphasized body
lg:   20px / line-height 1.4 / tracking -0.02em  → subheadings
xl:   24px / line-height 1.3 / tracking -0.03em  → headings
2xl:  30px / line-height 1.2 / tracking -0.04em  → display
3xl:  38px / line-height 1.1 / tracking -0.05em  → hero
```
**Font pairing rule:** Use a maximum of 2 fonts. One for UI (geometric sans or humanist sans), one for display (can be same). Never use Inter or Roboto as a default — they are the Arial of 2024. Reach for: Geist, Söhne, DM Sans, Plus Jakarta, Instrument Sans, Mona Sans, or system-ui as a last resort (but then compensate with strong spacing and color).

### Color System (Always Semantic)
```css
/* Define these, then ONLY use semantic names in components */
--color-bg:           #0a0a0a   /* page background */
--color-surface:      #111111   /* card / panel surface */
--color-surface-2:    #1a1a1a   /* elevated surface */
--color-border:       #262626   /* subtle borders */
--color-border-focus: #404040   /* focused borders */
--color-text:         #f5f5f5   /* primary text */
--color-text-2:       #a3a3a3   /* secondary text */
--color-text-3:       #525252   /* disabled / placeholder */
--color-accent:       #7c3aed   /* brand accent */
--color-accent-hover: #6d28d9   /* accent hover */
--color-success:      #22c55e
--color-warning:      #f59e0b
--color-error:        #ef4444
```
Adapt the palette to the project's tone. Dark-first is usually premium. Light themes must be equally intentional — off-white (`#fafaf9`) always beats pure white.

### Border Radius System
```
none:  0px     → tables, full-bleed elements
sm:    4px     → badges, tags, inline chips
md:    8px     → inputs, small buttons
lg:    12px    → cards, modals, panels
xl:    16px    → sheets, large cards
full:  9999px  → pills, avatars, FABs
```
Pick ONE radius scale and use it consistently. Never mix xl cards with sm modals.

### Shadow System
```
sm:  0 1px 2px rgba(0,0,0,0.3)                          → subtle lift
md:  0 4px 12px rgba(0,0,0,0.4)                          → card elevation
lg:  0 8px 32px rgba(0,0,0,0.5), 0 2px 8px rgba(0,0,0,0.3)  → modal / popover
xl:  0 24px 64px rgba(0,0,0,0.6)                         → dramatic hero
```
On light themes, reduce opacity by 60%. Never use colored shadows unless it's a brand accent glow.

---

## Component Quality Standards

Every component must handle these states or it is not done:

| State | Required |
|-------|----------|
| Default | ✅ |
| Hover (web) / Press (mobile) | ✅ |
| Active / Selected | ✅ |
| Disabled | ✅ |
| Loading | ✅ — skeleton or spinner, never just frozen |
| Empty | ✅ — illustrated or well-copywritten |
| Error | ✅ — with recovery action when possible |

**Button standards:**
- Minimum height: 36px (compact) / 44px (default) / 52px (large)
- Minimum tap target on mobile: 44×44pt (can have larger invisible hit area)
- Always have pressed state: `scale(0.97)` + slight darken
- Never show just a spinner on loading — disable + show spinner in button
- Icon buttons need aria-label, always

**Input standards:**
- Always show focus ring (2px, accent color, offset 2px)
- Error state = red border + error text below (not just border color change)
- Character counts when limit applies
- Never use placeholder as label — use floating label or label above

**Card standards:**
- Consistent inner padding (16px default, 24px spacious)
- Clear hierarchy: title → subtitle → content → actions
- Actions at bottom or top-right, never scattered
- Hover state should be subtle (1-2px translate or slight bg change), not dramatic

---

## Mobile UI Rules (React Native / Expo)

Read `references/mobile.md` for deep mobile guidance. Key non-negotiables:

### Safe Areas — Always
```javascript
import { SafeAreaView } from 'react-native-safe-area-context';
// or useSafeAreaInsets() hook for custom layouts
// Never hardcode top padding — devices have different notch/island sizes
```

### Touch Target Minimums
```javascript
// Every pressable element must be at least 44x44 points
// Use hitSlop if the visual is smaller:
<Pressable hitSlop={{ top: 8, bottom: 8, left: 8, right: 8 }}>
```

### Press Animations (Make It Feel Alive)
```javascript
// Use Animated or Reanimated for press feedback
// Never use opacity:0.5 as the only press state — it feels dead
// Spring-based press: scale 0.96 with spring(damping: 15, stiffness: 300)
// Always animate: opacity 1→0.8 AND scale 1→0.96 together
```

### Scroll Feel
```javascript
// Always set scrollEventThrottle={16} on ScrollView
// Use bounces={true} on iOS (default), don't disable unless necessary
// Large lists: FlatList with getItemLayout for performance
// Pull-to-refresh when content can be refreshed
```

### Typography on Mobile
```javascript
// iOS: SF Pro system font feels native — only override if brand requires
// Android: Roboto is fine — again, only override if brand requires
// But if you DO use custom fonts, make sure they're loaded before render (font splash)
// Always use allowFontScaling={false} on UI chrome, true on content
```

### Navigation Feel
- Use gesture-based navigation (react-navigation with gesture handler)
- Transitions must be smooth — default slide is fine, but match the app's energy
- Tab bars: always show active indicator with a smooth animation
- Back buttons: always visible, never hidden unless explicitly full-screen

---

## Web App Rules

### Layout
- Never use fixed pixel widths for main content containers — use `max-w-*` with auto margins
- Sidebar layouts: sidebar should be visually distinct but not jarring
- Responsive breakpoints: mobile-first, then sm (640), md (768), lg (1024), xl (1280)
- Grid systems: 12-col grid or CSS Grid, not random flexbox nesting

### Scrollbars (Web)
```css
/* Custom scrollbar — default ugly scrollbars break premium feel */
::-webkit-scrollbar { width: 6px; }
::-webkit-scrollbar-track { background: transparent; }
::-webkit-scrollbar-thumb { background: var(--color-border); border-radius: 999px; }
::-webkit-scrollbar-thumb:hover { background: var(--color-border-focus); }
```

### Focus Management
- All interactive elements must have visible focus rings
- Never `outline: none` without a custom replacement
- Keyboard navigation must work logically (tab order)

### Performance Feel
- Skeleton loaders > spinners for content areas
- Optimistic UI for mutations (update UI before server confirms)
- Instant feedback on every interaction (<100ms perceived response)

---

## Animation & Interaction Standards

### Timing Functions
```css
/* Spring feel (most satisfying for UI) */
--ease-spring: cubic-bezier(0.175, 0.885, 0.32, 1.275);
/* Smooth decelerate (for things entering) */
--ease-out: cubic-bezier(0.16, 1, 0.3, 1);
/* Smooth accelerate (for things leaving) */
--ease-in: cubic-bezier(0.7, 0, 0.84, 0);
/* Sharp snap (for mechanical interactions) */
--ease-snap: cubic-bezier(0.4, 0, 0.2, 1);
```

### Duration Guidelines
```
Micro (icon toggle, color change):     100-150ms
Standard (button press, hover):        150-200ms  
Component (modal open, drawer):        250-350ms
Page (route transition):               300-450ms
Dramatic (hero animation, onboarding): 500-800ms
```

### What Deserves Animation
- ✅ Press / hover states (always)
- ✅ Component mount/unmount (enter from below 8px, fade in)
- ✅ State changes (loading → content)
- ✅ Navigation transitions
- ✅ Notification / toast appearance
- ✅ List item interactions
- ❌ Scrolling (never animate the scroll itself, only elements triggered by scroll)
- ❌ Heavy animations on every render cycle

---

## Pre-Ship Checklist

Before considering any UI done, verify:

**Visual System**
- [ ] All spacing values are on the 8pt grid
- [ ] No arbitrary pixel values outside the token scale
- [ ] Type scale is consistent, no orphaned font sizes
- [ ] Color palette uses semantic tokens, no raw hex in components
- [ ] Border radii are consistent throughout

**Component Completeness**  
- [ ] All interactive elements have hover AND press/active states
- [ ] Loading states exist for all async operations
- [ ] Empty states are designed (not just "no items found" in grey)
- [ ] Error states are handled with recovery actions
- [ ] Disabled states look disabled (not just opacity)

**Mobile Specific**
- [ ] Safe area insets applied on all screens
- [ ] All tap targets ≥ 44×44pt
- [ ] Press animations feel springy and satisfying
- [ ] Tested at small (SE) and large (Pro Max) screen sizes
- [ ] Keyboard avoidance works on forms

**Web Specific**
- [ ] Focus rings visible on all interactive elements
- [ ] Custom scrollbar styles applied
- [ ] Responsive at mobile, tablet, desktop breakpoints
- [ ] No layout shifts on load

**Polish**
- [ ] Transitions feel smooth, not janky
- [ ] Text is legible at all sizes (contrast ratio ≥ 4.5:1 for normal text)
- [ ] Nothing looks "default" — every element has intent behind it

---

## Reference Files

- `references/mobile.md` — Deep React Native / Expo patterns, navigation, gestures, platform specifics
- `references/components.md` — Detailed component recipes (bottom sheet, tab bar, list, modal, toast)
- `references/inspiration.md` — Apps to reference for quality benchmarks with specific details on what makes them premium

---

## The Golden Rule

**If it looks like something built in an afternoon by someone who just learned React, it is not done.**

Ask yourself: would a user open this app and feel satisfaction? Would they notice the care? Does every tap feel responsive? Does every screen feel intentional?

If the answer is not a confident yes — keep going.
