# Brand Guide Analyzer

A skill for designers that turns brand guides, decks, screenshots, or existing Figma assets into a complete Brand Book directly in Figma.

Built for use with any AI assistant that supports custom skills and has the [Figma MCP](https://help.figma.com/hc/en-us/articles/32132100833559-Guide-to-the-Figma-MCP-Server) connected.

## What it does

Give the skill a brand guide PDF, a deck, a Figma file with logos and assets, or any combination — and it will:

1. Analyze the source materials and extract or infer the brand system
2. Detect the source's own structure (or fall back to standard sections)
3. Ask you the blocking setup questions one at a time
4. Create color variables, paint styles, and text styles in Figma
5. Generate full Brand Book pages with editorial left/right slide layouts
6. Run visual QA after every pass with screenshots
7. Support iterative refinement — fix individual slides, condense overlapping pages, create comparison variants

## Four entry modes

The skill picks the right mode based on what you give it:

- **Formal Guide Mode** — A real brand guide PDF with explicit rules
- **Loose Source Mode** — Decks, screenshots, uploaded brand images, no formal guide
- **Figma-First Mode** — Existing Figma file with brand assets but no documentation
- **Refinement Mode** — Pointing at an existing Brand Book page and iterating

## Installation

1. Clone or download this repo:
   ```bash
   git clone https://github.com/YOUR-USERNAME/brand-guide-analyzer.git
   ```

2. Place the skill folder where your AI tool can read it. For most setups:
   ```
   ~/Desktop/skills/brand-guide-analyzer/
   ```

3. Make sure the Figma MCP is connected in your tool. The skill needs `use_figma`, `get_design_context`, `get_metadata`, and `get_screenshot`.

4. In a new conversation, point your AI at the skill:
   ```
   Read all 6 skill files at ~/Desktop/skills/brand-guide-analyzer/
   then build a Brand Book in Figma from this source. [attach materials]
   ```

## How to use it

### Generation

```
I want to build a Brand Book in Figma from [PDF / deck / Figma file].
Follow the brand-guide-analyzer skill — detect the source structure first,
ask blocking questions one at a time, then generate.
```

The skill will:
1. Inspect what you gave it
2. Summarize what it found
3. Ask 4 blocking questions (file location, color implementation, layout format, brand assets)
4. Set up tokens and styles
5. Generate sections one at a time
6. QA each section visually
7. Return a structured map of created node IDs

### Refinement

```
The typography slide on this page [paste node URL] is too abstract.
Rebuild it with applied specimens instead of a spec table.
```

Or:

```
These two illustration slides overlap — condense them into one
combined slide. Keep the originals.
```

The skill will inspect the live state, plan the change, modify only what was requested, preserve any user edits, and run visual QA.

## File structure

| File | What it does |
|---|---|
| `SKILL.md` | Main skill file — entry modes, workflow, principles, blocking questions |
| `references/layout-spec.md` | Dimensions, auto layout, footer specs, token binding code, full building block implementations |
| `references/slide-code-templates.md` | Building block reference — quick lookup for which function does what |
| `references/slide-code-templates-sections.md` | Per-section slide patterns (Color, Typography, Logo, etc.) with applied-example variants |
| `references/refinement-patterns.md` | 10 step-by-step patterns for slide surgery (rebuild as specimens, condense overlap, fix auto layout, create comparison variants) |
| `references/source-inference.md` | How to derive a brand system from incomplete sources |

## Core principles

- **Inspect before you write** — Always read live Figma state before changing anything
- **Preserve user edits** — When in doubt, create a comparison variant
- **Applied over abstract** — Real specimens, measured diagrams, and live do/don't spreads beat spec tables
- **Mirror the source structure** — Use the brand guide's own organization when it has one
- **Run visual QA** — Screenshot every pass, fix issues before declaring done

## Requirements

- An AI assistant that supports custom skills
- Figma MCP connected with read and write access to your Figma files
- A Figma file you have edit access to (or permission to create new files)

## Limitations

- The skill needs the Figma MCP to write to Figma — it won't work standalone
- Generated slides use Inter as a fallback when the brand's actual fonts aren't loaded in Figma
- Component cloning works best when brand assets are already components in the file
- Very large brand guides (50+ pages) may need multiple passes — start with core sections (Color, Typography, Logo)

## Contributing

Issues and PRs welcome. The skill evolves based on real production runs — if you find a pattern that should be added, share it.

## License

MIT
