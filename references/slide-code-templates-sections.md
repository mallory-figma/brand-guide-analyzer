# Slide Code Templates — By Section

This file expands on `slide-code-templates.md` with section-specific guidance
for what each slide should teach and how to compose it.

The default building blocks are in `slide-code-templates.md`. Use this file
when you need more detail about what to put on each slide, including the
preferred applied-example variants over abstract spec tables.

For the actual implementation code of each building block, see
`references/layout-spec.md` "Building Block Functions — Full Code".

## Composition Rules (Apply to Every Slide)

- Left column: vertical auto layout, 360px wide, starting at x=64, y=40
- Right column: starts at x=480, width 896px (1440 - 480 - 64)
- Title: largest text in the left column, brand display font if available
- Body copy: 15px, line-height 24px, neutral at ~60% opacity
- Bullets: 14px, line-height 21px, with bullet character at ~30% opacity
- Footer: 13px brand label left, section label right, 1px divider above

## Color Section

Default slides:

| # | Title | Right Column |
|---|---|---|
| 01 | Color Overview | `buildHeroColorBlocks` — tall blocks of the 4-6 most identifying colors |
| 02 | Color Schemes | `buildColorBlockGrid` per scheme — one row per scheme, 4-5 swatches each |
| 03 | Background Colors | `buildColorBlockGrid` 2x3 — full-bleed BG candidates with names + roles |
| 04 | Palette Specs | `buildSwatchGrid` — every color in the system, organized by family |
| 05 | Color Pairings | `buildPairingCards` — Aa specimens with WCAG badges |
| 06 | Neutrals | `buildColorBlockGrid` tall columns — neutral scale with usage notes |
| 07 | What to Avoid | `buildDontCards` — 4 specific don'ts with brand examples |
| 08 | Color In Use | `buildMiniCompositions` — small applied examples showing the palette in real use |

### Applied-example variant for Slide 04 (Palette Specs)

Instead of the swatch grid, build a slide that shows the palette applied:
- Three real-feeling mini compositions side by side
- Each uses a different scheme (Primary, Campaign, Editorial)
- Real headline copy, accent strip, support color block
- Annotated with the colors used

Use this when the brand is visual-first and a swatch grid feels clinical.

### Left column copy guidance

The Color Overview slide should explain in 2-3 paragraphs:
- What colors mean in the brand (energy, calm, premium, etc.)
- The structure of the system (how families relate, scale convention)
- Type-on-color rules

The Pairings slide should list specific approved combinations with contrast
guidance. WCAG AA is a sensible inferred standard.

The What to Avoid slide should call out specific brand-relevant don'ts —
not generic advice. "Don't use mid-blue 100 as a background" is stronger
than "Don't use low-contrast colors."

## Typography Section

Default slides:

| # | Title | Right Column |
|---|---|---|
| 01 | Typography Overview | `buildHeadlineExamples` — 1-2 hero headlines showing the brand voice |
| 02 | Primary Typeface | `buildSpecimenBlocks` — 6 large specimens at display sizes |
| 03 | Titles & Headlines | `buildHeadlineExamples` — 2-3 headlines at different scales with applied copy |
| 04 | Body & UI | `buildBodyExamples` — subhead + body pairings in cards |
| 05 | Sizing & Spacing | `buildTypeScaleTable` OR applied specimens (preferred) |
| 06 | Secondary Typefaces | `buildSpecimenBlocks` — secondary face specimens |
| 07 | Guidance (Do) | `buildDoCards` — applied correct examples |
| 08 | Guidance (Don't) | `buildDontCards` — applied incorrect examples |

### Applied-example variant for Slide 05 (Sizing & Spacing) — PREFERRED

Instead of a spec table, build applied specimens:
- Vertical stack of representative type at each scale level
- Real copy for each (use the brand's actual content if available)
- Small label beside each: "Display Large / 80px / Inter Bold"
- Apply the matching text style to each specimen

This is the preferred approach. Only use the table format if the user
specifically asks for a spec table.

### Left column copy guidance

The Typography Overview should explain:
- The brand's typeface choice and what it conveys
- The typography hierarchy (how display, headline, body relate)
- Any specific rules (sentence case, letter spacing, mixed faces)

For Do/Don't slides: the lessons should be specific. "Don't mix Display
Large with Body Small in the same headline" is stronger than "Maintain
hierarchy."

## Logo Section

Default slides:

| # | Title | Right Column |
|---|---|---|
| 01 | Logo Overview | `buildComponentGrid` — all logo variants from the file |
| 02 | Wordmark | `buildOnBackgrounds` — wordmark on 2-3 surfaces with annotations |
| 03 | Icon / Mark | `buildComponentGrid` — icon variants |
| 04 | Clear Space | Measured diagram (preferred) OR text rules + diagram |
| 05 | Color Variations | `buildOnBackgrounds` — logo across approved BG palette |
| 06 | Placement | Real placement examples (cloned from actual marketing assets) |
| 07 | What to Avoid | `buildDontCards` — applied misuse examples |

### Applied-example variant for Slide 04 (Clear Space) — PREFERRED

Build a measured diagram:
- Logo instance centered
- Dashed safe-zone rectangle around it
- Dimension lines on each side with arrows/ticks
- Unit label (e.g. "1×" or "X = cap height")
- A reference square showing what "X" equals

See `refinement-patterns.md` Pattern 2 for full implementation guidance.

### Logo asset usage

Always use `createInstance()` for logo components — never recreate the logo
manually. If the file has only static logos (rectangles, vectors), use
`.clone()` instead.

For color variations, instances of the same component all show the same
fill. To show color variants, you'll need either:
- Component variants in the file (preferred — use the right variant)
- Manual fill overrides on each instance (acceptable for documentation)

If the brand has only one approved logo color, don't fabricate variations.
Show the single approved color and explain.

## Illustration Section

Default slides:

| # | Title | Right Column |
|---|---|---|
| 01 | Illustration Overview | `buildComponentGrid` — hero selection of illustrations |
| 02 | Full Library | `buildComponentGrid` — all assets at smaller scale, more cols |
| 03 | Illustration on Color | `buildOnBackgrounds` — one hero illustration on multiple BGs |
| 04 | Composition Rules | `buildDoCards` + `buildDontCards` side by side |
| 05 | What to Avoid | `buildDontCards` |

### Condensation opportunity

Slides 04 and 05 often overlap. Consider building a single combined slide
that shows:
- 3 DO examples (correct composition)
- 3 DON'T examples (incorrect composition)
- One unified rule set in the left column

If the user requests condensation, use this pattern.

### Asset usage

Use `createInstance()` for illustration components. If the illustrations are
in a component set with variants, you can place specific variants by using
the `variantProperties` API after creating an instance.

## Photography Section

Default slides:

| # | Title | Right Column |
|---|---|---|
| 01 | Photography Overview | Hero photo from file (cloned) + supporting smaller examples |
| 02 | Shot Types | Real photo examples from file, labeled by type |
| 03 | Treatment | Do/Don't with real photos showing correct/incorrect treatment |
| 04 | What to Avoid | `buildDontCards` with real photo misuses |

### Applied-example preference

Photography sections should use real photos from the file wherever possible.
`buildImagePlaceholders` should only be used when the file has no
photography assets and the section needs to be sketched out for the user.

### Condensation opportunity

Slides 03 and 04 often overlap. Build a single combined "Treatment" slide
with do/don't pairs if condensation is requested.

## Iconography Section (if present)

Default slides:

| # | Title | Right Column |
|---|---|---|
| 01 | Icon Overview | `buildComponentGrid` |
| 02 | Icon Library | `buildComponentGrid` at smaller scale |
| 03 | Icon Usage | Applied examples in UI contexts |
| 04 | What to Avoid | `buildDontCards` |

## Voice / Messaging / Beliefs Sections

These are text-heavy. Default slides:

| # | Title | Right Column |
|---|---|---|
| 01 | Overview | `buildHeadlineExamples` — key brand phrases shown as headlines |
| 02 | Principles / Tone | `buildBodyExamples` — tone examples in subhead + body cards |
| 03 | Guidance | `buildDoCards` + `buildDontCards` — real copy examples |

### Left column copy

For voice/messaging, the left column carries most of the weight. The
right column should illustrate the principle with applied copy examples,
not abstract descriptions.

Use the brand's actual taglines, headlines, and body copy as right-column
examples wherever possible.

## Default Slide Counts by Section

| Section | Default count | Minimum | Maximum |
|---|---|---|---|
| Color | 8 | 4 | 10 |
| Typography | 8 | 4 | 10 |
| Logo | 7 | 4 | 8 |
| Illustration | 5 | 3 | 7 |
| Photography | 4 | 2 | 6 |
| Iconography | 4 | 2 | 6 |
| Voice/Messaging | 3 | 2 | 5 |
| Spacing/Layout | 3 | 2 | 5 |
| Motion | 3 | 2 | 5 |

If the source is sparse, default to the minimum. If the source is rich and
the user wants thoroughness, default to the maximum.

## When to Use a Bespoke Slide Instead of a Template

Use a bespoke (custom-coded) slide instead of a template when:
- The slide needs a measured diagram (clear space, grid, type scale)
- The slide condenses two or more slides' worth of content
- The user requested a specific layout that doesn't match the template
- The brand has a strong visual system that the templates can't represent
- A template would force the content into a shape that hides the message

Bespoke slides are encouraged. Templates are defaults, not requirements.

When you build a bespoke slide:
- Still use the core helpers from `layout-spec.md`
- Still use auto layout for the left column
- Still bind colors and text styles to tokens
- Still run visual QA after building

## Anti-Patterns to Avoid

- **Spec tables when applied specimens would teach better** — prefer
  applied unless the user specifically wants a reference table
- **Generic don'ts** — "Don't use too many fonts" is weaker than
  "Don't pair Display Bold with Mono in headlines"
- **Empty placeholder right columns** — if you can't fill the right side
  with something useful, the slide may not be needed
- **Identical-looking slides across sections** — each section should
  feel like it teaches something distinct
- **Slides that restate the same lesson** — condense them
