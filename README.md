#Claude Skill For Premium UI
A Claude skill that permanently eliminates vibe-coded UI from all AI-generated interfaces.

When installed, this skill instructs Claude to produce **premium, production-grade UI** that feels like it was designed by a senior product designer — every time, without being asked.

---

## What It Does

Claude has a tendency to produce generic "AI aesthetic" UI:
- Random spacing with no grid system
- Default component library styling (unstyled shadcn, default MUI)
- Missing interaction states (no hover, no press, no loading, no error)
- Purple gradients on dark backgrounds for everything
- Inter/Roboto as the default font choice
- Touch targets too small for real fingers on mobile

This skill eliminates all of that by giving Claude a **complete design system** to follow:

- ✅ 8pt spacing grid enforced
- ✅ Type scale with line-height and letter-spacing
- ✅ Semantic color token system
- ✅ Consistent border-radius and shadow scales
- ✅ All component states required (default, hover, press, disabled, loading, empty, error)
- ✅ Mobile-first: safe areas, 44pt touch targets, spring press animations
- ✅ React Native / Expo deep guidance
- ✅ Premium animation timing and easing presets
- ✅ Pre-ship checklist
- ✅ Reference apps (Linear, Things 3, Craft, Stripe) with specific notes on what makes them premium

---

## File Structure

```
premium-app-ui/
├── SKILL.md                    ← Main skill file (always loaded)
└── references/
    ├── mobile.md               ← React Native / Expo deep patterns
    ├── components.md           ← Ready-to-adapt component recipes
    └── inspiration.md          ← Premium app benchmarks + anti-patterns
```

---

## Installation

### Claude.ai (Skills feature)
1. Download or clone this repository
2. In Claude.ai, open Settings → Skills
3. Upload the `premium-app-ui` folder or the `.skill` file if packaged
4. The skill will trigger automatically whenever Claude builds any UI

### Claude Code / Cowork
Place the skill folder in your skills directory. Claude will discover it automatically.

---

## What Changes After Install

**Before:** Ask Claude to "build a music app screen" → generic dark card with purple gradient, Inter font, no press states, hardcoded padding that breaks on iPhone SE.

**After:** Claude produces a proper screen with:
- 8pt grid spacing
- Custom spring press animations on cards and buttons
- Safe area handling
- Skeleton loading states
- Empty states with copy and a CTA
- Platform-specific haptic feedback calls
- Type scale with correct line-height

---

## Design Philosophy

This skill is based on studying what separates forgettable apps from apps people love:

> **Linear** — Every element earns its place. Nothing decorative.  
> **Things 3** — Feels handcrafted. Every animation was designed, not defaulted.  
> **Raycast** — Speed is a feature. The UI communicates that everything is instant.

The benchmark question is always: *Would a designer at Linear be embarrassed by this?*

---

## Contributing

This skill is a living document. PRs welcome for:
- New component recipes in `references/components.md`
- New premium app benchmarks in `references/inspiration.md`
- Corrections or improvements to design token scales
- Platform-specific patterns (tvOS, iPad, Android tablets)

---

## License

MIT — use freely, contribute back.

---

*Built to make Claude produce interfaces worth shipping.*
