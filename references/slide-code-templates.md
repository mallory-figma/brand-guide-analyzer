# Brand Book — Reusable Visual Building Blocks

This file contains reusable Figma code functions for generating Brand Book slides.
These are NOT section-specific — they are building blocks that work for ANY brand
and ANY section. The skill assembles the right blocks per slide based on what was
found in the source.

CRITICAL: All content (colors, fonts, copy, rules) comes from the analysis.
These functions define HOW to build visuals. The WHAT comes from the source.

For section-specific guidance on which blocks to use per slide, see
`references/slide-code-templates-sections.md`.

For full implementation code of each function, see `references/layout-spec.md`
under "Building Block Functions — Full Code".

---

## How to Use This File

Every Figma:use_figma call should:
1. Include the **Core Helpers** from `references/layout-spec.md`
2. Include whichever **Building Block** functions the slide needs (also in layout-spec.md)
3. Call those functions with data extracted from the source

---

## Building Block Reference

### buildHeroColorBlocks(slide, colors)
Creates tall color blocks side by side spanning the right column.
Use for: Color Overview, any section overview that shows key brand colors.
Data: `colors = [{hex:"#0000CC", name:"Primary"}, ...]`

### buildSwatchGrid(slide, families)
Creates a full palette grid organized by color family with hex values, step labels, and role tags.
Use for: Palette Specs, full color libraries.
Data: `families = [{name:"Blue", colors:[{step:"200", hex:"#0000CC", role:"Primary BG"}, ...]}, ...]`

### buildColorBlockGrid(slide, colors, cols, startY)
Creates a grid of solid color blocks with name and note labels.
Use for: Background Colors, Neutrals, Color Schemes.
Data: `colors = [{hex:"#0000CC", name:"Blue 200", note:"Primary brand BG"}, ...]`

### buildPairingCards(slide, pairings, cols)
Creates "Aa" specimen cards showing text on background with WCAG badges.
Use for: Color Pairings, Type on Color.
Data: `pairings = [{bg:"#0000CC", text:"#FFFFFF", label:"Blue + White", wcag:"AA"}, ...]`

### buildSpecimenBlocks(slide, specimens, cols, rows)
Creates large type specimens on dark or light blocks.
Use for: Typography primary/secondary typeface pages.
Data: `specimens = [{chars:"Aa", size:80, fontFamily:"Inter", fontStyle:"Bold", bg:"#0A0A0A", fg:"#FFFFFF"}, ...]`

### buildTypeScaleTable(slide, tokens)
Creates a tabular layout of all typography tokens with size, weight, line-height, letterspacing.
Use for: Typography sizing — but consider applied specimens instead (preferred).
Data: `tokens = [{token:"display-xl", font:"Oswald SemiBold", size:"80px", lineHeight:"100%", letterSpacing:"-4%"}, ...]`

### buildHeadlineExamples(slide, examples)
Creates large headline text on light gray blocks stacked vertically.
Use for: Typography titles/headlines, messaging key phrases.
Data: `examples = [{text:"Think bigger.\nBuild faster.", size:64, fontFamily:"Inter", fontStyle:"Bold", color:"#000000"}, ...]`

### buildBodyExamples(slide, examples, cols)
Creates cards with subhead + body copy pairings.
Use for: Typography body/UI, messaging tone examples.
Data: `examples = [{subhead:"Your Spending", body:"Track categories, set limits...", subSize:24, bodySize:14}, ...]`

### buildComponentGrid(slide, nodeIds, cols, bg)
Places Figma component instances in a grid by looking up their node IDs.
Use for: Logo variations, illustration library, icon library.
Data: `nodeIds = ["1:234", "1:235", ...]` — IDs of components in the file.

### buildOnBackgrounds(slide, nodeId, backgrounds, cols)
Places the same component on multiple brand-colored backgrounds.
Use for: Logo color variations, illustration on color.
Data: `backgrounds = [{hex:"#0000CC", label:"On Blue 200"}, ...]`

### buildDontCards(slide, donts, cols)
Creates red-bordered "DON'T" cards with preview area and rule text.
Use for: "What to Avoid" in ANY section.
Data: `donts = [{bg:"#0000CC", fg:"#00007A", preview:"Low contrast\nblue on blue", rule:"Never use same-family text on background."}, ...]`

### buildDoCards(slide, dos, cols)
Creates green-bordered correct example cards with optional caption.
Use for: Guidance (Do) in any section.
Data: `dos = [{headline:"Dream it.", headSize:36, body:"Supporting copy here.", caption:"Correct size hierarchy"}, ...]`

### buildImagePlaceholders(slide, placeholders, cols)
Creates labeled placeholder blocks where real images can be added later.
Use for: Photography sections when no real photos exist in the file.
Data: `placeholders = [{label:"Lifestyle — teens using app", note:"Natural light, authentic moments"}, ...]`

NOTE: Prefer cloning real photos from the file over placeholders. See
`references/slide-code-templates-sections.md` Photography Section.

### buildMiniCompositions(slide, examples)
Creates small example compositions showing color/type combinations.
Use for: Color In Use, Brand In Use.
Data: `examples = [{label:"Primary", bg:"#0000CC", accent:"#7DEB00", support:"#7040E0"}, ...]`

---

## Section to Building Block Mapping (Quick Reference)

For full slide-by-slide guidance per section, see
`references/slide-code-templates-sections.md`.

### Color (8 slides)
| Slide | Right Column Building Block |
|---|---|
| 01 — Color Overview | `buildHeroColorBlocks` |
| 02 — Color Schemes | `buildColorBlockGrid` (one row per scheme) |
| 03 — Background Colors | `buildColorBlockGrid` (2x3 grid) |
| 04 — Palette Specs | `buildSwatchGrid` |
| 05 — Color Pairings | `buildPairingCards` |
| 06 — Neutrals | `buildColorBlockGrid` (tall columns) |
| 07 — What to Avoid | `buildDontCards` |
| 08 — Color In Use | `buildMiniCompositions` |

### Typography (8 slides)
| Slide | Right Column Building Block |
|---|---|
| 01 — Typography Overview | `buildHeadlineExamples` (1-2 hero headlines) |
| 02 — Primary Typeface | `buildSpecimenBlocks` (6 blocks, large type) |
| 03 — Titles & Headlines | `buildHeadlineExamples` (2 headline examples) |
| 04 — Body & UI | `buildBodyExamples` (subhead + body cards) |
| 05 — Sizing & Spacing | Applied specimens (preferred) or `buildTypeScaleTable` |
| 06 — Secondary Typefaces | `buildSpecimenBlocks` |
| 07 — Guidance (Do) | `buildDoCards` (2x2 correct examples) |
| 08 — Guidance (Don't) | `buildDontCards` (2x2 incorrect examples) |

### Logo / Identity (6-7 slides)
| Slide | Right Column Building Block |
|---|---|
| 01 — Identity Overview | `buildComponentGrid` (all logo assets) |
| 02 — Wordmark | `buildOnBackgrounds` (wordmark on 2-3 surfaces) |
| 03 — Icon | `buildComponentGrid` (icon variants) |
| 04 — Clear Space | Bespoke measured diagram (preferred) |
| 05 — App Icons & Partnerships | `buildComponentGrid` |
| 06 — Product Identity | `buildOnBackgrounds` or `buildColorBlockGrid` |
| 07 — What to Avoid | `buildDontCards` |

### Illustration (5-6 slides)
| Slide | Right Column Building Block |
|---|---|
| 01 — Overview | `buildComponentGrid` (hero selection, 3-4 cols) |
| 02 — Full Library | `buildComponentGrid` (all assets, 5 cols) |
| 03 — On Color | `buildOnBackgrounds` (one illustration, 6 BGs) |
| 04 — Composition Rules | `buildDoCards` + `buildDontCards` |
| 05 — What to Avoid | `buildDontCards` |

### Photography (4-5 slides)
| Slide | Right Column Building Block |
|---|---|
| 01 — Overview | Cloned hero photo + supporting examples |
| 02 — Shot Types | Cloned real photos labeled by type |
| 03 — Treatment Rules | `buildDoCards` + `buildDontCards` with real photos |
| 04 — What to Avoid | `buildDontCards` with real photos |

### Messaging / Voice & Tone / Beliefs (2-4 slides)
| Slide | Right Column Building Block |
|---|---|
| 01 — Overview | `buildHeadlineExamples` (key brand phrases) |
| 02 — Details / Principles | `buildBodyExamples` (tone examples in cards) |
| 03 — Guidance | `buildDoCards` + `buildDontCards` |

---

## Important Notes

1. **All building block function implementations are in `references/layout-spec.md`**
   under the "Building Block Functions — Full Code" section. Copy the function
   code you need into each `Figma:use_figma` call.

2. **Always include the Core Helpers** (h, F, isDarkBg, makeLeftCol, addText,
   addBullet, addDivider, addFooter, RX, RW constants) at the top of every call.

3. **Data arrays come from the source analysis** — never hardcode brand-specific
   values. The building blocks are brand-agnostic structures that accept any data.

4. **Generate 1-2 slides per `Figma:use_figma` call** to stay under the 50K char limit.

5. **Find the page and calculate sY** at the start of every call after the first:
   ```javascript
   await figma.setCurrentPageAsync(
     figma.root.children.find(p => p.name === "📖 Color — Brand Book")
   );
   const page = figma.currentPage;
   let sY = Math.max(...page.children.map(n => n.y + n.height)) + 80;
   ```

6. **Bespoke slides are encouraged** when templates don't fit. See
   `references/slide-code-templates-sections.md` "When to Use a Bespoke Slide
   Instead of a Template".

7. **In refinement mode**, prefer surgical edits over full rebuilds. See
   `references/refinement-patterns.md`.
