# Phase 5: Design & Polish

## Objective

The frontend is functional — every button works, every link resolves, every form submits, navigation is complete. NOW make it beautiful. This phase transforms a working but generic frontend into a visually polished, domain-appropriate, professionally designed application.

**This phase uses WebSearch (freshness protocol) to research current design trends, competitive visual benchmarks, and domain-appropriate aesthetics BEFORE making design decisions.**

## Prerequisites

Phase 4b (Functional Verification) MUST be complete. Every interactive element works. Do NOT polish a broken frontend.

## 5.1 Domain Research (WebSearch Required)

Before touching any design tokens, research:

1. **Domain visual benchmarks** — WebSearch for the top 3-5 products in the same domain as the BRD. What do they look like? What visual language do they use?
   - Fintech/banking → clean, trustworthy, blue/green, lots of whitespace, data-dense dashboards
   - E-commerce → vibrant, product-focused, high-contrast CTAs, image-heavy
   - Developer tools → dark mode default, monospace accents, technical, information-dense
   - Healthcare → soft, accessible, calming blues/greens, large touch targets
   - Consumer social → playful, colorful, rounded, motion-rich, personality-driven
   - Enterprise B2B → professional, neutral, data-focused, dense but organized
   - Creative/design → bold, expressive, unique typography, visual storytelling

2. **Current design trends** — WebSearch for "web design trends {current year}" and "{domain} dashboard design {current year}". Note patterns that are current vs dated.

3. **Competitive analysis** — What do direct competitors look like? Where can we match? Where can we differentiate?

Document findings in `Claude-Production-Grade-Suite/frontend-engineer/docs/design-research.md`.

## 5.2 Color Theory

Upgrade the default blue palette to a researched, domain-appropriate color system:

### Primary Color Selection

Choose based on:
- **Product personality** from BRD (trustworthy? innovative? playful? professional?)
- **Domain conventions** (fintech → blue/green, health → blue/teal, creative → purple/orange)
- **Color psychology:**
  - Blue → trust, stability, professionalism
  - Green → growth, health, money, nature
  - Purple → creativity, luxury, innovation
  - Orange → energy, warmth, friendliness
  - Red → urgency, passion, action (use sparingly)
  - Teal → modern, fresh, tech-forward
  - Indigo → depth, wisdom, premium feel

### Color System

Generate a complete color system:

```
Primary    → 50-950 scale (main brand color)
Secondary  → 50-950 scale (complementary or analogous)
Accent     → single vivid shade (for CTAs, highlights, attention)
Neutral    → 50-950 scale (grays — warm or cool depending on primary)
Semantic   → success (green), warning (amber), danger (red), info (blue)
```

Rules:
- Primary and secondary must have sufficient contrast when used together
- Accent color must pop against both light and dark backgrounds
- Neutral grays should have a slight warm or cool tint matching the primary (pure gray looks dead)
- Every text/background combination meets WCAG 2.1 AA (4.5:1 normal, 3:1 large)
- Dark mode is NOT just inverted — it needs its own considered palette where primary colors shift lighter

### Application

- **Primary** → main actions, active states, key UI elements, brand presence
- **Secondary** → supporting elements, secondary actions, tags, categories
- **Accent** → CTAs that need to pop, new feature badges, important notifications
- **Neutral** → text, backgrounds, borders, dividers, subtle UI

## 5.3 Typography

Upgrade from system fonts to a considered type system:

1. **WebSearch** for current popular font pairings for the domain
2. Choose a heading font and body font (can be the same family at different weights):
   - **SaaS/Dashboard** → Inter, Geist, or similar clean sans-serif
   - **Marketing/Consumer** → Wider variety — consider personality fit
   - **Developer tools** → JetBrains Mono or similar monospace for code, clean sans for UI
   - **Enterprise** → Conservative — Inter, Roboto, or system fonts

3. **Type hierarchy:**
   - Clear visual distinction between h1-h6 (not just size — weight and spacing matter)
   - Body text optimized for readability (16px minimum, 1.5 line height, 60-75 char line length)
   - Consistent use of font weight to create hierarchy (not just size)
   - Tabular numbers for data-heavy UIs (tables, dashboards, metrics)

## 5.4 Visual Hierarchy & Layout Polish

Apply design principles to make information scannable and actions obvious:

1. **Whitespace** — Increase spacing between sections. Group related items tighter. Use generous padding in cards and containers. Whitespace is not wasted space — it guides the eye.

2. **Visual weight** — Primary CTAs should be the heaviest element on screen (filled, bold color). Secondary actions lighter (outlined or ghost). Destructive actions in red but not visually dominant unless intentional.

3. **Information density** — Match the domain:
   - Dashboard → higher density, compact tables, small type for data
   - Marketing → lower density, generous whitespace, large type
   - Settings → medium density, clear sections, adequate spacing

4. **Card and container styling** — Consistent border radius, subtle shadows for elevation, clear visual grouping. Cards should look like cards (distinct from background), not just boxes.

5. **Icon consistency** — Use a single icon library throughout (Lucide, Heroicons, or Phosphor). Consistent size and stroke width. Icons should enhance, not replace text for important actions.

## 5.5 Micro-Interactions & Feedback

Add the polish that makes a UI feel alive:

1. **Hover states** — Every clickable element has a visible hover state (background shift, subtle shadow, or opacity change). Not just cursor:pointer.

2. **Active/pressed states** — Buttons should feel pressed (scale down slightly, darken).

3. **Focus states** — Visible focus ring (not browser default — styled to match design). Critical for accessibility.

4. **Transitions** — Smooth transitions on color, background, shadow, transform. 150ms for micro-interactions, 300ms for layout changes. Use ease-out for entering, ease-in for exiting.

5. **Loading feedback** — Buttons show loading spinner when clicked and waiting for API. Forms disable during submission. Skeleton screens match the actual content layout.

6. **Success/error feedback** — Toast notifications for async results. Inline validation for forms (green checkmark for valid, red text for errors). Optimistic updates with rollback for fast perceived performance.

7. **Page transitions** — Subtle fade or slide between routes (respect prefers-reduced-motion).

## 5.6 Dark Mode Polish

Dark mode is NOT light mode with colors inverted. It needs specific attention:

- Background layers: use 2-3 shades of dark (pure black is harsh — use dark gray like #0a0a0a or #111)
- Reduce white text brightness (use #e5e5e5 not #ffffff for body text)
- Primary colors shift lighter/more saturated in dark mode for same visual impact
- Shadows don't work in dark mode — use subtle border or lighter background instead
- Test all states in both modes — hover, active, focus, disabled, error

## 5.7 Responsive Polish

Verify the design works beautifully at all breakpoints:

- Mobile (320px-640px): touch-friendly, stacked layout, hamburger nav, full-width cards
- Tablet (768px): 2-column where appropriate, sidebar as overlay
- Desktop (1024px+): full layout, sidebar visible, multi-column grids
- Wide (1536px+): max-width container, content doesn't stretch infinitely

## Implementation

Update these files with researched design decisions:

1. `frontend/app/styles/tokens/colors.ts` — Replace defaults with researched palette
2. `frontend/app/styles/tokens/typography.ts` — Replace system fonts with chosen typeface
3. `frontend/app/styles/theme/light-theme.ts` — Polished light theme
4. `frontend/app/styles/theme/dark-theme.ts` — Polished dark theme (not just inverted)
5. `frontend/tailwind.config.ts` — Updated with final design tokens
6. All components — Update to use refined tokens (hover states, transitions, focus styles)
7. All pages — Apply spacing, visual hierarchy, and layout polish

## Output

- `Claude-Production-Grade-Suite/frontend-engineer/docs/design-research.md` — Domain research, competitive analysis, trend findings
- `Claude-Production-Grade-Suite/frontend-engineer/docs/design-decisions.md` — Color choices with rationale, typography selection, visual hierarchy rules
- Updated token files, theme files, and component styles

## Validation Loop

Before moving to Phase 6 (Testing):
- [ ] Design research documented with WebSearch findings
- [ ] Color palette is domain-appropriate with documented rationale
- [ ] Typography is intentional (not default system fonts unless that's the right choice)
- [ ] Every hover, active, focus, and disabled state is styled
- [ ] Dark mode is polished (not just inverted)
- [ ] Visual hierarchy is clear — primary CTA is the most prominent element on every page
- [ ] Spacing and whitespace are generous and consistent
- [ ] Responsive design looks good (not just functional) at all breakpoints
- [ ] Micro-interactions provide feedback for every user action

**Present design system with before/after comparison to user via AskUserQuestion.**

## Quality Bar

"Default blue with system fonts" is NOT production-grade. "Researched indigo palette matching fintech trust conventions, Inter for UI / JetBrains Mono for data, 8px spacing rhythm, animated state transitions, polished dark mode with proper surface hierarchy" — that is production-grade.
