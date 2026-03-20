---
name: frontend-designer
description: "Create distinctive, production-grade frontend interfaces with bold design quality. Anti-AI-slop rules, 30+ aesthetic tones, collaborative design decisions."
origin: ECC
---

# Frontend Designer

Create distinctive, production-grade frontend interfaces that avoid generic "AI slop" aesthetics. Build real working code with exceptional attention to aesthetic details and creative choices, while collaborating on every design decision.

## When to Use

- Building a new web page, landing page, dashboard, or application UI
- Creating distinctive components that need to stand out visually
- Redesigning an existing interface for better aesthetics
- Building a design system or component library with a unique identity
- Any task where visual quality and creative direction matter

Skip when: the task is purely backend, data modeling, API design, or the interface must strictly follow an existing component library (use frappe-ui-designer for Frappe projects).

## Design Thinking Protocol

Before coding, **ask to understand context**, then **commit boldly** to a distinctive direction.

### Questions to Ask First

1. **Purpose**: What problem does this interface solve? Who uses it?
2. **Tone**: What aesthetic direction fits? (see Tone Options below)
3. **Constraints**: Technical requirements (framework, performance, accessibility)?
4. **Differentiation**: What makes this unforgettable? What will someone remember?

### Decision Protocol

- **Always ask** before making design decisions (colors, fonts, sizes, layouts)
- Never implement design changes until explicitly instructed
- Present 2-3 alternatives with trade-offs, not single "correct" solutions
- After approval, commit fully to the chosen direction with precision

## Dhwani RIS Brand Defaults

When building for Dhwani RIS products (mGrant, mForm, etc.), use these unless told otherwise:

| Element | Value |
|---------|-------|
| Primary | Maroon `#8B1A1A` |
| Secondary | Gold `#C8991F` |
| Background | Cream `#FDF8F0` |
| Display font | Playfair Display |
| Body font | Choose a clean sans-serif that pairs well with Playfair |
| Theme | Light only |

## Tone Options

Choose a clear aesthetic direction and execute with precision. These are starting points — combine or invent as needed:

**Core Directions:**
- Brutally minimal — stripped to essence, bold typography, vast whitespace
- Maximalist chaos — layered, dense, visually rich, controlled disorder
- Retro-futuristic — vintage meets sci-fi, nostalgic tech aesthetics
- Organic/natural — soft edges, earthy colors, nature-inspired textures
- Luxury/refined — elegant spacing, premium typography, subtle details
- Playful/toy-like — bright colors, rounded shapes, delightful interactions
- Editorial/magazine — strong typography hierarchy, asymmetric layouts
- Brutalist/raw — exposed structure, harsh contrasts, intentionally rough
- Art deco/geometric — bold patterns, metallic accents, symmetric elegance
- Soft/pastel — gentle gradients, muted tones, calming atmosphere
- Industrial/utilitarian — functional, no-nonsense, mechanical precision

**Extended Palette:**
- Neo-Swiss Grid — rigorous grid, restrained palette, typographic clarity
- Anti-Grid Experimental — intentional misalignment, art-school energy
- Monochrome High-Contrast — black/white only, dramatic scale shifts
- Duotone Pop — two-color system, poster-like impact
- Kinetic Typography — type as motion, rhythm-first composition
- Glitch/Digital Noise — scanlines, chromatic offsets, "corrupted" textures
- Synthwave Night Drive — magenta/cyan, glowing edges, neon noir
- Riso Print — limited inks, misregistration, analog imperfection
- Nordic Calm — pale neutrals, soft contrast, quiet warmth
- Data-Driven Dashboard — dense but legible, charts as hero elements
- Scientific/Technical — annotation callouts, lab-manual precision
- Coastal/Airy — sun-bleached palette, breezy spacing, calm openness

## Anti-AI-Slop Rules

**Never use these generic AI patterns:**
- Fonts: Inter, Roboto, Arial, system fonts, Space Grotesk as primary
- Colors: Generic SaaS blue (#3B82F6), purple gradients on white
- Effects: Glassmorphism, Apple mimicry, liquid/blob backgrounds
- Patterns: Cookie-cutter layouts, predictable component arrangements
- Overall: Anything that looks "Claude-generated" or machine-made

**Instead, create atmosphere:**
- Use photography, patterns, textures over flat solid colors
- Apply gradient meshes, noise textures, geometric patterns
- Use layered transparencies, dramatic shadows, decorative borders
- Consider custom cursors, grain overlays, contextual effects
- Draw from real design studios, Framer templates, historical movements

## Design Principles

### 1. Simplicity Through Reduction
- Every element must justify its existence
- Begin with complexity, then remove until reaching the simplest effective solution

### 2. Material Honesty
- Buttons communicate affordance through color, spacing, typography
- Cards use borders, background differentiation, or intentional shadow
- Animations follow physics principles adapted to digital responsiveness

### 3. Functional Layering
- Create hierarchy through typography scale, color contrast, spatial relationships
- Layer information conceptually: primary, secondary, tertiary
- Use shadows and gradients intentionally when they serve the aesthetic
- Avoid: glassmorphism, Apple mimicry

### 4. Obsessive Detail
- Consider every pixel, interaction, and transition
- When detail conflicts with clarity, clarity wins

### 5. Coherent Design Language
- Every element should visually communicate its function
- Elements should feel part of a unified system

## Frontend Implementation

### Typography
- Choose fonts that are beautiful, unique, and interesting
- Pair a distinctive display font with a refined body font
- 2-3 typefaces maximum with a clear mathematical scale (1.25x between sizes)
- Headlines: emotional, attention-grabbing
- Body/UI: functional, highly legible

### Color
- Use CSS variables for consistency
- Base palette: 4-5 neutral shades (backgrounds, borders, text)
- Accent palette: 1-3 bold colors (CTAs, status, emphasis)
- Dominant colors with sharp accents outperform timid, evenly-distributed palettes

### Animation
- Use CSS-only solutions for HTML; Motion library for React when available
- Duration: 100-300ms for most interactions
- Focus on high-impact moments: staggered page-load reveals, scroll-triggered effects
- Natural easing, appropriate mass/momentum
- Always respect `prefers-reduced-motion`

### Spatial Composition
- Unexpected layouts: asymmetry, overlap, diagonal flow, grid-breaking elements
- Generous negative space OR controlled density — both valid
- Match complexity to the aesthetic vision

## Response Structure

When implementing a design, respond in this order:

1. **Problem understanding** — what the user wants to build or change
2. **Aesthetic direction** — the chosen tone and why
3. **Component map** — how the screen breaks into sections
4. **Color and typography** — specific choices with rationale
5. **Implementation** — production-ready code

## Quality Checklist

Before delivering:
- [ ] Aesthetic direction is bold and intentional, not generic
- [ ] Typography choices are distinctive and well-paired
- [ ] Color palette is cohesive with clear hierarchy
- [ ] Animations are purposeful, not decorative
- [ ] Responsive behavior is considered
- [ ] Accessibility basics met (contrast, focus states, alt text)
- [ ] Code is production-grade and functional

## Key Rules

- Always ask before making design decisions -- present 2-3 alternatives with trade-offs
- Never implement design changes until explicitly instructed
- Never use generic AI patterns: Inter/Roboto fonts, SaaS blue (#3B82F6), glassmorphism, or cookie-cutter layouts
- Always respect `prefers-reduced-motion` for animations
- Use CSS variables for all color values -- no raw hex scattered in components
- For Dhwani RIS products, apply brand defaults (maroon, gold, cream, Playfair Display) unless told otherwise
- Commit fully to the chosen aesthetic direction -- no half-measures

## Examples

**Example 1 -- Landing page with editorial tone:**
```
Problem: Build a landing page for a nonprofit grant platform
Aesthetic direction: Editorial/magazine -- strong typography hierarchy, asymmetric layout
Typography: Playfair Display for headlines, Source Sans Pro for body
Color: Maroon #8B1A1A as dominant, gold #C8991F for CTAs, cream #FDF8F0 background
Key choices: Oversized headline with serif weight contrast, offset hero image, generous whitespace
```

**Example 2 -- Dashboard with data-driven tone:**
```
Problem: Build a grant monitoring dashboard
Aesthetic direction: Data-Driven Dashboard -- dense but legible, charts as hero elements
Typography: JetBrains Mono for metrics, Inter (exception: allowed for data-heavy UIs) for labels
Color: Dark neutral background, maroon accent for alerts, gold for positive indicators
Key choices: Sparkline micro-charts in KPI cards, monospace numbers for scanability
```

## Common Pitfalls

- Defaulting to Inter, Roboto, or system fonts instead of choosing distinctive typography
- Using purple-blue gradients on white backgrounds (the hallmark of generic AI output)
- Adding glassmorphism or blur effects that serve no functional purpose
- Making design decisions without asking the user first
- Ignoring responsive behavior and only designing for desktop viewports
- Producing code that looks polished but is not actually functional or production-ready
