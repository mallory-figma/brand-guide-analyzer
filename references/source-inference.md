# Source Inference

This file documents how to derive a coherent brand system when the source is
incomplete. Use this in **Loose Source Mode** and **Figma-First Mode** —
when there is no formal brand guide and the inputs are decks, screenshots,
uploaded images, or a Figma file with assets but no rules.

The goal is to produce a usable brand system that feels coherent, even if
parts are inferred. Label inferred parts when communicating with the user.

## Source Hierarchy (Reminder)

1. **Explicit** — directly stated rules from a formal guide
2. **Source-derived** — strongly implied by repeated live examples
3. **Inferred** — reasonable best practice used to fill gaps

When sources conflict, apply this order. Don't pretend inferred rules came
from a formal guide.

## Inspection Pass — What to Look For

### From a Figma file

Use `get_metadata` and `get_design_context` to inspect:

- **Pages and their names** — pages like "Brand", "Logos", "Illustrations",
  "Photography", "Marketing" are signal-rich
- **Component sets** — multi-variant components often represent system thinking
  (logo variations, illustration libraries, icon sets)
- **Local variables and styles** — already-existing tokens are explicit signal
- **Frames with names like "Hero", "Cover", "Title slide"** — show the brand's
  preferred composition style
- **Repeated colors across frames** — recurring fills are the brand palette
- **Repeated typefaces and weights** — recurring fonts are the brand type system

### From uploaded images and screenshots

- **Color usage** — note dominant colors, accent colors, neutrals
- **Typography** — note typefaces, weights, hierarchy
- **Layout patterns** — note column structures, spacing rhythm, photo treatment
- **Logo placement** — note where logos sit in compositions
- **Visual language** — note illustration style (3D, flat, hand-drawn),
  photography treatment (lifestyle, product, abstract)

### From a deck PDF

- **Title slides** — show preferred typography pairings
- **Recurring layouts** — show composition rules
- **Color blocking** — shows palette logic
- **Charts and data viz** — show secondary color usage and accent patterns
- **Footer/header treatments** — show brand signature elements

## Color System Inference

When colors are not explicitly defined:

1. **Sample from live examples.** Pick the 5-10 most-used colors across all
   visible materials. Use a consistent set of recurring values rather than
   one-off colors that appear only once.

2. **Group into families.** Cluster similar hues together (all blues, all
   greens, etc.). Within a family, identify dark/light variants.

3. **Assign roles based on usage:**
   - Background: any color used as a full-bleed surface
   - Accent: a color that appears smaller but consistently for emphasis
   - Neutral: gray scale and black/white usage
   - Brand: the most identifying color — what the brand "is"

4. **Detect modes:** if you see the same brand element rendered in two
   different palettes (e.g. a deck with both blue-dominant and pink-dominant
   slides), that's a multi-mode signal. Use Primitives + Semantic collections.

5. **Don't invent steps.** If you only see two blues, don't generate a 6-step
   blue scale. Document the two you see and label as inferred if you add more.

### Common color inference mistakes

- **Don't sample from anti-aliasing edges.** Pick colors from solid fills, not
  blended pixels.
- **Don't include accidental colors.** A one-off color in a single graphic
  isn't part of the system.
- **Don't make up step numbers.** If the brand has named "Cobalt" and "Sky",
  use those names. Don't force them into "Blue 200 / Blue 500" unless that's
  how the brand thinks.

## Typography System Inference

When typography is not explicitly defined:

1. **Identify all typefaces in use.** Look at headlines, body, UI labels,
   captions, marketing copy.

2. **Establish roles:**
   - Display/headline: largest, most expressive
   - Body: most common reading size
   - UI: labels and microcopy
   - Mono (if present): code, data, metadata

3. **Build the scale from observed sizes.** Note actual sizes in use. Build
   a scale that matches what you saw, not a generic 12-14-16-18-24 scale.

4. **Note weight usage.** If you only see Regular and Bold, don't add Medium
   to the system. If you see Light occasionally, consider whether it's part
   of the system or an outlier.

5. **Note specific quirks:**
   - Sentence case vs. title case in headlines
   - Letter spacing on caps
   - Italic usage (rare? common? specific to a role?)
   - Mixed typeface usage in headlines

### Typography signals from a Figma file

- Local text styles → explicit
- Components with consistent typography → source-derived
- Frames with hand-set typography → likely outliers, weaker signal

## Logo System Inference

When logo rules are not explicit:

1. **Find the logo asset(s).** Component sets are best. Otherwise look for
   frames named "Logo", "Wordmark", "Mark", or similar.

2. **Note variations:**
   - Wordmark only
   - Mark/icon only
   - Lockup with tagline
   - Stacked vs. horizontal
   - Colorways (mono, full color, knockout)

3. **Infer clear space from observed examples.** Look at how the logo is
   placed near other elements. The minimum gap you observe is a reasonable
   inferred clear space, often expressed as 1× the cap height.

4. **Infer minimum size from smallest observed usage.** If you see the logo
   used at 24px in a small UI context, that's a reasonable minimum.

5. **Identify backgrounds the logo appears on.** The colorways used signal
   the approved background palette.

### When to ask the user

If you can't find a logo asset, ask:
"I don't see a logo asset in the file. Can you share the logo or point me to where it lives?"

Don't fabricate a logo from descriptions.

## Illustration System Inference

When illustration rules are not explicit:

1. **Find the asset library.** Component sets are best. Look for pages named
   "Illustrations", "Icons", "Graphics", or component sets with multi-variant
   structures.

2. **Identify the style.** Examples:
   - 3D rendered (with reflections, depth, materials)
   - Flat with limited palette
   - Outline / line-art
   - Hand-drawn / sketchy
   - Photographic collage

3. **Note recurring elements:**
   - Color palette used in illustrations
   - Treatment (drop shadows? glows? flat?)
   - Subject matter (objects, characters, scenes)

4. **Infer composition rules from observed usage.** How are illustrations
   placed in real layouts? Floating? Bleeding off edges? Integrated with type?

5. **Note any organic background shapes.** Wavy shapes, blobs, gradient
   washes are often part of the illustration system.

## Photography System Inference

When photography rules are not explicit:

1. **Find photographic assets.** Look for pages named "Photography",
   "Imagery", "Photos", or frames containing real images.

2. **Categorize shot types:**
   - Lifestyle (people in context)
   - Product (clean, isolated)
   - Detail (close-up, texture)
   - Environmental (wide, contextual)
   - Abstract (mood, texture)

3. **Note treatment:**
   - Color grading (warm? cool? high-contrast?)
   - Cropping conventions (full-bleed? framed?)
   - Filtering or overlays
   - Black and white vs. color usage

4. **Infer the brand's photography intent.** Authentic and candid? Polished
   and curated? Energetic and saturated? Soft and natural?

## When to Stop Inferring

Don't fill every gap. Some sections may legitimately not exist for a brand.

If the brand has no photography in any source, don't generate a photography
section. Tell the user it wasn't found.

If the brand has no formal voice/messaging guidance, you can either:
- Skip the messaging section
- Generate a brief inferred voice section based on observed copy patterns,
  clearly labeled as inferred

## Communicating Inferences to the User

When summarizing what you found, be honest about the source:

```
What I found:
  • Color — 6 families across 24 values (source-derived from Figma file)
  • Typography — 2 typefaces, 4 weights (source-derived)
  • Logo — wordmark + 3 variants in component set (explicit)
  • Illustration — 20-variant 3D set (explicit)
  • Photography — not found
  • Voice — not explicitly defined

I'll generate sections for what I found. If you want a Photography or
Voice section, I can infer one from your existing materials, or you
can share more reference.
```

## Inference Output Format

When you proceed to generation, internally tag every rule you write with
its source classification. Example:

```
Color → "Blue 200 is the primary brand background"
  source: source-derived (appears as background in 70% of slides in the deck)

Typography → "Headlines use Inter Bold at -2% letter-spacing"
  source: source-derived (consistent across all observed headlines)

Logo → "Maintain 1× cap-height clear space"
  source: inferred (no explicit guidance, standard best practice)
```

This metadata doesn't need to appear in the user-facing output, but it
informs your phrasing. Source-derived rules can be written as confident
statements. Inferred rules should be phrased as recommendations or
defaults.

Example phrasing differences:

- Explicit: "Headlines must always be set in Inter Bold."
- Source-derived: "Headlines are set in Inter Bold."
- Inferred: "We recommend Inter Bold for headlines, based on observed usage."

## When You're Wrong

If the user corrects an inference ("actually we don't use Inter, that was
just a placeholder"), update your internal model and regenerate the affected
slides. Don't argue with the user about what their brand is.

Inferences are starting points, not final answers.
