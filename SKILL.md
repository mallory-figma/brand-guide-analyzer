---
name: brand-guide-analyzer
description: Analyze formal brand guides or loose brand source sets (PDFs, decks, screenshots, uploaded assets, and Figma brand boards), infer color, typography, logo, illustration, and photography systems, then build and refine a Brand Book directly in Figma using MCP. Use when the user wants to analyze a brand, extract tokens, create guidelines, generate a brand book, condense or refine existing brand-book slides, or turn live Figma brand assets into documentation.
---

# Brand Guide Analyzer + Brand Book Generator

Build or refine a brand book in Figma from either a formal guide or incomplete brand materials.

This skill is adaptive:
- It can start from a real brand guide PDF.
- It can start from a loose source set: deck PDFs, screenshots, brand images, and a Figma file with assets.
- It can skip full analysis and go straight into slide refinement when the user points to an existing Figma page or node.

Do not force the user through the full pipeline if the task is clearly a refinement request.

## Required Tools

- Figma MCP with `get_design_context`, `get_metadata`, `get_screenshot`, and `use_figma`
- If the `figma-use` skill is available, load it before every `use_figma` call
- Use `get_design_context` as the primary inspection tool, `get_metadata` for structure, and `get_screenshot` for visual QA

## Reference Files

Keep this skill operational. Detail lives in references:

| File | When to read |
|---|---|
| `references/layout-spec.md` | Before any Figma generation — dimensions, auto layout patterns, footer specs, token binding code, building block function implementations |
| `references/slide-code-templates.md` | Before generating slides — the building block library with section-to-block mapping tables |
| `references/slide-code-templates-sections.md` | When generating a specific section — full slide patterns per section type |
| `references/refinement-patterns.md` | In refinement mode — slide surgery, comparison variants, condensation, applied-example rebuilds |
| `references/source-inference.md` | In Loose Source or Figma-First mode — how to infer a brand system from incomplete inputs |

## Entry Modes

Choose one mode first based on what the user provided and what they're asking for.

### 1. Formal Guide Mode
The user provides a real brand guide or style guide PDF with explicit rules.

### 2. Loose Source Mode
The user provides incomplete brand materials such as a deck, sales presentation, screenshots, or uploaded brand images. No formal guide.

### 3. Figma-First Mode
The Figma file already contains brand assets (logos, illustrations, photography, color references) and the user wants guidelines generated from what's there.

### 4. Refinement Mode
The user references an existing Figma page, frame, or node and asks for changes. Triggers include:
- "another pass"
- "fix this slide"
- "make a version to compare"
- "condense these slides"
- "rework this page"
- "turn this into a do/don't"
- "use the screen and my edits"

In refinement mode, skip the full analysis pipeline unless the user explicitly asks for it.

## Source Hierarchy

Resolve conflicts in this order:
1. Explicit rules from a formal brand guide
2. Live Figma assets, components, pages, and styles
3. Uploaded images, screenshots, and loose reference materials
4. Inferred best practices

When the source is incomplete, do not stall. Infer a usable system and label inferred parts when communicating with the user.

## Core Principles

- Treat the Figma file as both input and output.
- Prefer live brand assets over recreated placeholders.
- Prefer applied examples over abstract spec tables when the source supports it.
- Preserve user edits.
- Work incrementally.
- Run visual QA after every meaningful generation or refinement pass.
- Return structured node IDs after each pass so future refinements are safe.

## Quick Triage

Before substantial work, decide:
1. Is this generation or refinement?
2. Are the sources formal, loose, or Figma-first?
3. Is there enough information to proceed without blocking questions?
4. Which sections will be explicit vs inferred?

If the user already provided enough context, continue without asking the full question set.

## Minimal Intake — Blocking Questions

Some questions ARE blocking. They materially change what you build. Always ask
these unless the user has explicitly answered them in the prompt.

**Generation mode — required questions (ask one at a time, wait for each answer):**

1. **Where should this be built?**
   - Existing Figma file (paste URL) OR create a new file
   - If creating new and the user has multiple workspaces, ask which workspace
   - After creating a new file, ALWAYS return the file URL to the user

2. **Color implementation:**
   - Variables / Styles / Both
   - If multiple palettes detected, also ask about variable modes
     (Primitives + Semantic with modes, OR single flat collection)

3. **Documentation layout format:**
   - Multi-page (each section gets its own Figma page)
   - Single-page sections (one page, sections stacked vertically)

4. **Brand assets:**
   - Are there logo/illustration/photo assets in the file I should use?
   - If yes, wait for node link(s)
   - If no, proceed with placeholders or skip those sections

**Skip a question only if the user already answered it.** For example:
- User said "create a new file in my Acme workspace" → skip the workspace question
- User said "use variables only" → skip the implementation question
- User said "I want it as separate pages" → skip the layout question

When asking, ask ONE question at a time. Wait for the answer. Do not combine
questions into one message. This is the most common UX failure mode.

**Refinement mode — usually no upfront questions.** Inspect the live state
first, then ask only if the request is genuinely ambiguous (e.g. "make it
better" — ask "what specifically should change?").

## Workflow Overview

Use the smallest workflow that fits the request.

### Generation Workflow
1. Inspect the source materials (PDF, uploads, Figma file)
2. Extract or infer the brand system
3. **Detect the source's own structure** (see Structure Detection below)
4. Summarize findings in plain language
5. Ask blocking questions one at a time (file, color implementation, layout, assets)
6. Set up tokens and styles in Figma
7. Generate brand-book pages or sections
8. Run visual QA
9. Iterate based on feedback
10. Return a structured map of created IDs

### Refinement Workflow
1. Inspect the referenced page or node first (`get_design_context` + `get_screenshot`)
2. Read live structure and current visual state
3. Compare against the user's feedback
4. Modify only the targeted slide or section
5. Preserve user edits
6. Create a comparison variant if requested
7. Run visual QA immediately after the change
8. Return updated/created IDs

## Analysis Rules

Do not output raw JSON to the user unless they explicitly ask for it.

Internally extract:
- color families and recurring palette modes
- typography roles and visible hierarchy
- logo structures, safe zone clues, and minimum-size guidance
- illustration patterns
- photography treatment patterns
- recurring composition rules
- misuse patterns
- any explicit rules from the source

Classify each rule internally as one of:
- `explicit`: directly stated in the source
- `source-derived`: strongly implied by repeated live examples
- `inferred`: reasonable best practice used to fill a gap

Apply in order: `explicit` first, then `source-derived`, then `inferred`.

For Loose Source and Figma-First modes, see `references/source-inference.md` for how to derive a coherent system from messy inputs.

## Structure Detection — Mirror the Source First

CRITICAL: Before defaulting to the standard section template (Color / Typography
/ Logo / Illustration / Photography), check whether the source has its own
structure. If it does, mirror that structure.

### Detecting source structure

Look for these signals in the source:

**In a brand guide PDF:**
- Table of contents at the front
- Section title pages or splash pages with headers like "01 Identity", "02 Voice", "03 Visual System"
- Persistent navigation in headers/footers showing section names
- Repeated section dividers with consistent naming
- Numbered page sections in a sidebar

**In a Figma file:**
- Page names already organized by topic ("Identity", "Voice & Tone", "Application")
- Frame names suggesting structure ("Section 1", "Chapter 2")
- Existing brand-book pages that establish the rhythm

**In a deck PDF:**
- Recurring slide template with section headers
- Agenda or outline slide near the start
- Section divider slides between content groups

### Decision rules

**Source has clear structure:**
Use the source's section names AND section order. For example, if the brand
guide is organized as:
- Identity
- Voice
- Visual System
- Application

Then create pages named:
- 📖 Identity — Brand Book
- 📖 Voice — Brand Book
- 📖 Visual System — Brand Book
- 📖 Application — Brand Book

The CONTENT inside each section still uses the standard slide patterns (auto
layout left column, building blocks on the right) — but the section organization
mirrors the source. If the source has an "Identity" section that combines logo
and color, build it that way rather than splitting them.

**Source has partial structure:**
Use what's there. Fill gaps with the standard template. For example, if the
source has clear "Identity" and "Voice" sections but no visual system structure,
keep "Identity" and "Voice" as named, then add Color/Typography/Illustration/
Photography as additional sections following defaults.

**Source has no clear structure:**
Fall back to the standard template based on what tokens/assets you found:
1. Color
2. Typography
3. Logo / Identity
4. Illustration
5. Photography
6. Iconography (if present)
7. Voice / Messaging (if present)

### Communicating structure to the user

When summarizing findings, surface the structure decision. Examples:

```
The brand guide is organized as Identity / Voice / Visual System /
Application — I'll mirror that structure in the Brand Book.
```

```
The source doesn't have a clear section structure, so I'll use the
standard layout: Color, Typography, Logo, Illustration, Photography.
```

```
The Figma file has existing pages for "Brand", "Logos", and
"Illustrations" — I'll build alongside those rather than duplicating.
For Color, Typography, and Photography I'll use the standard template.
```

This lets the user correct you if you misread the structure.

### When to deviate from the source structure

Only deviate when:
- The user explicitly asks for a different organization
- The source structure is so unusual it would be confusing as a brand book
  (in which case, surface this and ask)
- The source has duplicate or conflicting structures (e.g. a TOC that doesn't
  match the actual section dividers)

## Copy Rules

Use source-derived copy whenever possible.

When the source is strong:
- Reuse the brand guide's actual language and rules
- Keep the slide copy close to the source intent

When the source is incomplete:
- Write concise, practical brand guidance inferred from visible patterns
- Avoid fake specificity (don't invent measurements that aren't there)
- Don't pretend inferred rules came from a formal guide

Do not write generic filler copy if the live assets can teach the rule visually.

## Figma-First Inspection

When a Figma file is available, inspect it early for:
- Pages that already contain logos, illustrations, photography, or brand examples
- Existing components and instances
- Local variables and styles
- Reusable assets that can be cloned or instanced into the brand book

Prefer:
- `createInstance()` for components
- `.clone()` for real example frames or images
- Existing variables/styles when they already represent the brand system

## Token Setup

Before major slide generation, create or align:
- Color variables
- Paint styles when useful
- Text styles
- Semantic roles when multiple palette modes are present

Default patterns:
- Single palette: one color collection is acceptable
- Multiple palettes: use primitive + semantic collections with modes
- Create both variables and styles when the user wants a reusable documentation file

Use clear naming. Match the brand if the file already has conventions.

For full token creation code, see `references/layout-spec.md` under "Token Creation — Code Reference."

## Generation Preferences

Prefer these outputs when the source supports them:
- Applied type specimens instead of only type tables
- Measured logo diagrams instead of vague logo rules
- Live do/don't spreads instead of generic rule cards
- Cloned or instanced real examples instead of placeholders
- Condensed merged slides when two slides teach the same idea

If a fixed template fights the content, adapt the slide rather than forcing the content into the template. Bespoke Figma scripts are encouraged for targeted slide rewrites, diagrams, and condensed slides.

## Applied Example Preference

The right column should teach the rule, not just decorate the slide. Prefer:
- Real examples over abstract specs
- Applied mockups over isolated swatches
- Measured diagrams (with arrows, labels, dimension lines) over text descriptions
- Side-by-side positive/negative comparisons over rule lists
- Cloned live frames over reconstructed approximations

Avoid empty placeholders unless the user specifically wants space reserved.

Examples of when to choose applied over abstract:
- Typography sizing → applied specimens at each scale, not a token table
- Logo clear space → measured diagram with safe-zone overlays, not a paragraph
- Color pairings → real component examples on background, not just Aa cards
- Photography treatment → real photo do/don't using assets in the file

For the full applied-example pattern catalog, see `references/refinement-patterns.md`.

## Condense Overlap Pattern

When two slides communicate the same principle:
1. Identify the shared lesson
2. Combine the strongest copy into one left-column rule set
3. Use the right side for clearer examples
4. Keep the original slides unless the user explicitly wants them replaced
5. Name the new frame clearly so it is easy to compare

Common targets for condensation:
- Treatment rules + misuse → one combined treatment slide
- Composition rules + what not to do → one applied composition slide
- Spec-heavy slide + applied-example slide → one applied slide with key spec callouts
- Two overlapping illustration guidance slides → one focused guidance slide

For step-by-step condensation patterns, see `references/refinement-patterns.md`.

## Refinement Patterns

Common refinement requests this skill handles natively:
- Rebuild a typography scale slide as applied specimens
- Rebuild a logo safe-zone slide as a measured diagram
- Create a combined do/don't slide from overlapping guidance slides
- Fix left-column auto layout and bullet wrapping
- Create a second slide for comparison instead of overwriting
- Turn live photo or illustration assets into teaching examples

Each of these has a step-by-step pattern in `references/refinement-patterns.md`.

## Preserve User Edits

If the user has edited a frame:
- Inspect the current live frame before changing anything
- Do not rebuild blindly from an older plan
- Do not overwrite their changes unless they explicitly ask
- If the request is comparative or ambiguous, create a new variant

If unexpected differences appear between the assumed state and the live file, stop and re-inspect before proceeding.

## Comparison Variant Rule

Create a new slide instead of replacing the current one when the user asks to:
- Compare options
- Do another pass
- Keep the current frame
- Make a new one using the same idea
- Preserve what they already changed

Use the current live slide as the source of truth for the comparison variant. Place the variant adjacent to the original (immediately below or beside it) and name it clearly:
- `01a — Color Overview` (original)
- `01b — Color Overview (variant)` (new)

## Visual QA Loop

After each major generation step or refinement pass:
1. Inspect structure with `get_metadata` if needed
2. Take a screenshot with `get_screenshot`
3. Check for:
   - Clipped or overlapping text
   - Broken bullet auto layout
   - Visual imbalance between positive and negative examples
   - Incorrect hierarchy or spacing
   - Content that teaches the wrong lesson
   - Color contrast or legibility problems
4. Fix the issue before moving on

Do not assume the first generated slide is good enough without a screenshot pass.

## Figma Implementation Rules

- If available, load `figma-use` before every `use_figma` call
- Use `await figma.setCurrentPageAsync(page)` for every non-default page (never set `figma.currentPage` directly)
- Always `await figma.loadFontAsync()` before editing text
- Inter font style is `"Semi Bold"` with a space, not `"SemiBold"`
- Keep `use_figma` calls focused and incremental — 1-2 slides per call to stay under the 50K char limit
- Return all created and mutated node IDs in structured data
- Prefer `get_design_context` for reading a node before editing it
- Use `get_metadata` when you need hierarchy, IDs, or structure
- Use `get_screenshot` for final visual verification

## Left Column Rule

The left column must use vertical auto layout.

Never manually stack long copy and bullets with hardcoded y positions when the content can wrap.

Default structure:
- Eyebrow
- Title
- Intro paragraph
- Optional second paragraph
- Subhead
- Bullets
- Divider
- Second subhead
- Bullets

All text should be height-auto and all bullet rows should hug content vertically.

For the full auto-layout left-column pattern, see `references/layout-spec.md`.

## Right Column Rule

The right side should teach the rule, not just decorate the slide.

Prefer:
- Real examples
- Applied mockups
- Measured diagrams
- Clear misuse comparisons
- Side-by-side positive and negative examples

For the building block library that powers right-column visuals, see `references/slide-code-templates.md`.

## Structured ID Map

After every generation or refinement pass, return a structured summary:

```
Created/updated:
  - Page: "📖 Color — Brand Book" (id: 5:120)
    - 01 Color Overview (id: 5:121)
    - 02 Color Schemes (id: 5:135)
    ...
  - Variables: "Brand — Primitives" (collection id: VariableCollectionId:1:2)
    - Blue/200 (id: VariableID:1:5)
    ...
  - Text styles: 8 created
```

This makes future refinement passes safer and faster — the user can reference IDs directly.

## Communicating With The User

Summaries should be plain language, concise, and action-oriented.

A good update includes:
- What you learned (1-2 sentences)
- What you created or changed
- Whether content was inferred or explicit
- What still needs review, if anything

Do not dump internal extraction objects or raw schemas unless the user asks for them.

## Completion Output

### After generation
Tell the user:
- Which sections or slides were created
- Which pages were updated
- Whether tokens and styles were added
- Which parts were inferred vs. explicit
- Any slides that still need real examples or manual review
- The structured ID map

### After refinement
Tell the user:
- Which page and slide changed
- Whether it was updated in place or a comparison variant was created
- The key design change made
- Whether visual QA passed
- The IDs of any new nodes

## Default Success Criteria

The skill is successful when:
- The brand system feels coherent even if the source was incomplete
- The Figma file uses live tokens, styles, and assets where possible
- The slides teach with applied examples
- User edits are preserved
- Refinement passes are fast and safe
- The output looks visually checked, not merely generated

## Edge Cases

### No Formal Guide
Proceed from loose materials. Label results as inferred where appropriate.

### User Wants One Page or Slide Fixed
Go straight to refinement mode. Skip analysis.

### User Provides Only a Figma File
Inspect the file first. Use the live assets and page structure as the main source of truth. See `references/source-inference.md`.

### Fonts Are Missing in Figma
Use the closest available fallback for the guideline UI. Note the intended font in the slide metadata or a comment.

### Sources Conflict
Apply the source hierarchy: explicit guide rules first, then live Figma assets, then loose references, then inferred best practices.

### Large or Messy Source Sets
Prioritize the core visible systems in this order:
1. Color
2. Typography
3. Logo
4. Illustration
5. Photography

Then extend the brand book if the user wants more.

### User Asks "Make Another Version To Compare"
Always create a new variant. Never replace the current slide. Place the new variant next to the original.

### User Asks To "Condense" or "Combine" Slides
Use the condense overlap pattern. By default, create a new combined slide and leave the originals in place unless the user says to delete them.

### Refinement Request Lands Mid-Generation
Pause generation, switch to refinement mode for the requested change, then resume generation if the user wants to continue.

## Examples Of Requests This Skill Should Handle

- "Analyze this brand guide and build a brand book in Figma."
- "This isn't a real guide, but use this deck and these brand images."
- "Use the logos, illustrations, and photography already in this Figma file."
- "Rework this typography scale so it shows the type applied."
- "This logo slide is wrong. Turn it into a safe-zone diagram."
- "Condense these two overlapping slides into one new slide."
- "Fix the auto layout in the bullets."
- "Make another version so I can compare."
- "Do the same condensation pass for photography."
- "Use my edits and just clean up the right side."
