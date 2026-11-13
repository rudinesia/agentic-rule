---
name: lp-conversion
description: >
  Conversion psychology patterns and UX rules for landing pages. Use this skill when
  coding or designing any section of a landing page, hero section, CTA block, testimonial
  section, pricing section, or conversion-focused UI. Triggers on: designing a landing page
  layout, building hero sections, writing CTA copy, creating trust/social proof components,
  adding animations or micro-interactions, choosing layout structure, or any time the goal
  is to maximize conversions. Pair with lp-strategy (positioning) and lp-technical (build).
  Apply this skill's rules to EVERY section you code, not just the hero.
---

# Landing Page Conversion Patterns

Apply these rules to every section of the page during build.

## The 50ms "Blink Test"

Visitors judge the entire business in ~50ms. The first impression creates a **Halo Effect** —
a bad first impression makes good testimonials feel suspicious; a premium first impression
makes average copy feel credible.

**Blink Test Checklist (hero section must pass all):**
- [ ] Clear hierarchy: one dominant element draws the eye immediately
- [ ] Not cluttered: whitespace communicates confidence, not emptiness
- [ ] Visual style signals the price point (luxury ≠ cheap SaaS template)
- [ ] Zero stock photos in above-the-fold area
- [ ] CTA button is the most visually distinct interactive element on screen

## Cognitive Load Reduction

The brain seeks the path of least resistance (Cognitive Fluency). Every friction point
is a conversion killer.

### Hierarchy Rules
```
ONE dominant focal point per section.
H1 > H2 > body — never reverse this contrast.
If everything is bold, nothing is bold.
Max 1 accent color; all else is neutral.
```

### Navigation Rules
```
Max 7 nav items (ideally 4-5).
One nav CTA that mirrors the page's primary action.
No mega menus on landing pages — they create escape routes.
```

### Copy Density Rules
```
Hero body text: max 2 lines / 25 words.
Section intros: max 3 lines.
Bullet points: max 5 per list, each under 12 words.
Break up long text with subheads every 3-4 paragraphs.
```

## Layout Anti-Grid Patterns

Generic grid layouts signal "template." Break the box to signal "crafted."

**Use these patterns:**
- **Offset cards:** testimonial cards staggered vertically, not aligned to a single row
- **Bleeder image:** photo or graphic extends past its container edge
- **Asymmetric split:** 60/40 column instead of 50/50
- **Diagonal section break:** `clip-path: polygon(0 0, 100% 0, 100% 90%, 0 100%)` between sections
- **Large number overlay:** stat like "342%" rendered huge and transparent behind body copy
- **Sticky proof bar:** thin bar below nav showing logo wall or a live counter

**Avoid:**
- Equal-width 3-column feature grids (the "three boxes" cliché)
- Centered hero with centered paragraph text (looks like a template)
- Section dividers that are just horizontal lines

## CTA Engineering

```
ONE primary CTA per page viewport. Never two competing actions visible simultaneously.

CTA Copy Formula:
❌  "Submit" / "Click Here" / "Learn More"
✅  "[verb] + [specific outcome]" → "Get My Free Audit" / "Start Saving Today"

CTA Button:
- Contrast ratio vs background: minimum 4.5:1 (WCAG AA)
- Size: min 44×44px tap target (mobile)
- Whitespace around CTA: generous (don't crowd it)
- Micro-copy below CTA: kills final objection ("No credit card. Cancel anytime.")
```

## Social Proof Hierarchy

Not all proof is equal. Rank by persuasive power:

1. **Specific outcome + named person + photo** — "47% more leads in 6 weeks — Sarah M., CEO at Finlo"
2. **Named person + photo** — no metric but face = trust
3. **Logo wall** — recognizable brands only; generic logos add zero trust
4. **Star ratings** — only with a review count ("4.9 / 5 from 238 reviews")
5. **Generic quote, no photo** — weakest; use only if nothing else available

**Rules:**
- Never use stock photography for testimonials
- Specificity > enthusiasm: "amazing!" means nothing; "3× revenue in 90 days" means everything
- Place proof immediately after the first CTA (preempts the "but does it work?" thought)

## Micro-Interaction Recipes

Micro-interactions create "peaks of delight" that build psychological trust.
Implement these — they signal craftsmanship:

```css
/* Hover lift — cards, buttons */
.card { transition: transform 0.2s ease, box-shadow 0.2s ease; }
.card:hover { transform: translateY(-4px); box-shadow: 0 16px 40px rgba(0,0,0,0.12); }

/* CTA pulse (draw attention without being annoying) */
@keyframes subtle-pulse {
  0%, 100% { box-shadow: 0 0 0 0 rgba(var(--accent), 0.4); }
  50% { box-shadow: 0 0 0 8px rgba(var(--accent), 0); }
}
.cta-primary { animation: subtle-pulse 3s infinite; }

/* Scroll reveal */
.reveal { opacity: 0; transform: translateY(24px); transition: opacity 0.5s ease, transform 0.5s ease; }
.reveal.visible { opacity: 1; transform: none; }
/* JS: IntersectionObserver to add .visible class */
```

**Restraint rule:** Maximum 2-3 animation types per page. More = noise.

## Section-by-Section Conversion Rules

| Section | Primary Job | Conversion Lever |
|---|---|---|
| Hero | Pass blink test + state outcome | Bold H1, single CTA, social proof snippet |
| Problem | Create "that's me" recognition | Specific pain language (not generic) |
| Solution | Shift belief (not feature list) | Before/after framing, transformation language |
| Proof | Remove skepticism | Specific results, real names, real photos |
| Process | Reduce decision anxiety | 3 steps max, makes it feel easy |
| FAQ | Kill objections | Answer the *real* objection, not the polite one |
| Final CTA | Convert the fence-sitters | Restate outcome + add risk reversal |

→ After applying these patterns, use `lp-technical` for build standards and performance.