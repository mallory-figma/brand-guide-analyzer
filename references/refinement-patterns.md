# Refinement Patterns

This file documents step-by-step patterns for refining existing brand-book
slides. Use these when the skill is in **Refinement Mode** — when the user
references an existing Figma page or node and asks for changes.

The patterns assume:
- The target page/frame already exists
- The user may have made manual edits
- You should inspect before changing
- You should preserve user edits
- You should run visual QA after every pass

## General Refinement Loop

For ANY refinement request, follow this sequence:

1. **Identify the target node**
   - Get the page name, frame name, or node ID from the user
   - Use `get_metadata` to confirm the node exists and check its hierarchy
   - Note any sibling slides for context

2. **Inspect the current state**
   - `get_design_context` on the target node to read structure
   - `get_screenshot` on the target node to see current visual state
   - Note user edits — text changes, manual repositioning, color tweaks, deleted elements

3. **Plan the change**
   - Identify what specifically the user wants different
   - Decide: in-place edit OR comparison variant
   - If ambiguous or comparative, default to comparison variant

4. **Execute the change**
   - For in-place: modify only the targeted children, preserve siblings
   - For variant: clone the frame, place adjacent (below or to the right), modify the clone, name it clearly

5. **Visual QA**
   - `get_screenshot` of the result
   - Check for clipping, overlap, broken auto layout, hierarchy issues
   - Fix before declaring done

6. **Report**
   - Which node was changed or created
   - In-place or variant
   - The key design change
   - QA result
   - New node IDs

## Pattern 1: Rebuild Type Scale as Applied Specimens

**Trigger:** "Rework this typography scale so it shows the type applied" or "make this typography slide visual instead of a table"

**Why:** Type scale tables are abstract. Applied specimens at each size show exactly how the type performs in real use.

**Steps:**

1. Inspect the existing slide. Read the type tokens being documented (display sizes, body sizes, weights).
2. Get the actual font family and weight from the existing text styles — don't reinvent the scale.
3. Decide on representative copy for each scale level. Use the brand's actual headline/body samples if available, otherwise pick neutral content that demonstrates the size.
4. Build a new right column that stacks specimen blocks vertically:
   - Each block has the actual type at its actual size
   - A small label below or beside (font name, size, line height)
   - Optional: a "USED FOR" tag (Display, Headline, Body, etc.)
5. Apply the corresponding text style to each specimen so the slide is bound to the type system.
6. Replace the table on the right side, keep the left column copy.
7. Visual QA — check that nothing is clipped at the largest sizes.

**Code shape:**

```javascript
// Each specimen is a stacked block in the right column
const specimens = [
  { text: "Display Large", style: "Headline/Display Large" },
  { text: "Display Medium", style: "Headline/Display Medium" },
  { text: "Headline", style: "Headline/Default" },
  { text: "Body Large for paragraphs and longer reading.", style: "Body/Large" },
  { text: "Body. The default reading size for most product UI.", style: "Body/Default" },
  { text: "Caption — labels and metadata", style: "Caption" },
];

// Stack vertically using auto layout, apply each text style by ID
```

## Pattern 2: Rebuild Logo Slide as Safe-Zone Diagram

**Trigger:** "Turn this logo slide into a safe-zone diagram" or "show clear space measurements"

**Why:** A measured diagram is more useful than a paragraph describing clear space. Designers can see and copy the exact spacing rule.

**Steps:**

1. Find the logo component or vector in the file. Use `get_metadata` to locate it. If not found, ask the user for the node ID.
2. Inspect the logo's bounding box dimensions.
3. Determine the safe-zone unit. Common conventions:
   - 1× cap height (the height of the logo's main letterform)
   - 1× the logo's height
   - A specific fraction the brand specifies
4. Create a diagram on the right column:
   - Logo instance centered
   - A dashed rectangle around the logo at the safe-zone offset
   - Dimension lines (small lines with arrows) showing the unit on each side
   - A label naming the unit (e.g. "1×" or "X = cap height")
   - Optional: a small reference of the unit (a small box labeled "X")
5. Bind colors to the brand's neutral variables.
6. Replace the right column. Keep the left column rules text.
7. Visual QA — verify the dimension lines are crisp and the diagram reads clearly.

**Diagram construction notes:**

- Use thin (1px) strokes with a subtle color (50% opacity neutral)
- Dashed strokes for the safe zone boundary, solid for dimension lines
- Place arrows or end caps on dimension lines (small triangles or perpendicular ticks)
- Keep the logo at a comfortable size — large enough that the safe zone doesn't dominate

## Pattern 3: Condense Two Overlapping Slides

**Trigger:** "Condense these two slides" or "combine these — they're saying the same thing"

**Why:** Two slides teaching the same lesson dilute the message. One stronger slide does the work better.

**Steps:**

1. Inspect both source slides — read all left-column copy and all right-column visuals.
2. Identify the shared lesson. Articulate it in one sentence.
3. Pull the strongest 3-5 rules from both slides into a unified bullet list.
4. Decide on the right-column treatment. Best options:
   - Side-by-side do/don't if the slides included examples
   - A single applied example that demonstrates the unified principle
   - A two-column layout with the most useful visuals from each source
5. Create a new combined slide — do NOT delete the originals unless the user explicitly asks.
6. Name the new slide clearly: e.g. `04 — Composition (combined)` or `04+05 — Composition`.
7. Place the new slide adjacent to the originals (immediately after the second one in the page).
8. Visual QA — ensure the combined slide isn't more crowded than the originals it replaces.

**Naming pattern:**

- If the user wants the new slide to replace the originals later, mark it with `(combined)` so they can identify and clean up
- If the user wants both, give it a fresh number that fits the section

## Pattern 4: Fix Bullet Auto Layout

**Trigger:** "The bullets are broken" or "fix the wrapping in the left column"

**Why:** Bullets often break when content gets longer than expected. The text overflows or overlaps the next bullet.

**Steps:**

1. Inspect the left column with `get_design_context`.
2. Verify the left column frame:
   - `layoutMode = "VERTICAL"`
   - `primaryAxisSizingMode = "AUTO"` (height hugs content)
   - `counterAxisSizingMode = "FIXED"` (width stays at 360)
   - `itemSpacing` set (typically 16-20)
3. Verify each bullet row:
   - Is itself a horizontal auto layout frame
   - `layoutSizingHorizontal = "FILL"` on the row
   - `layoutSizingVertical = "HUG"` on the row
   - Inside the row: bullet text node with `layoutSizingVertical = "HUG"`
   - Inside the row: text content node with `textAutoResize = "HEIGHT"`, `layoutSizingHorizontal = "FILL"`, `layoutSizingVertical = "HUG"`
4. If any of these properties are missing, set them.
5. If text was set with `textAutoResize = "WIDTH_AND_HEIGHT"`, change to `"HEIGHT"`.
6. Visual QA — verify bullets wrap cleanly and don't overlap.

**Common failure cases:**

- Text node has fixed width instead of FILL → text won't wrap
- Row frame is not auto layout → bullet and text don't align
- Left column is fixed height instead of AUTO → content gets clipped

## Pattern 5: Create Comparison Variant

**Trigger:** "Make another version so I can compare" or "do another pass but keep this one"

**Why:** The user wants to evaluate options side by side without losing the current state.

**Steps:**

1. Inspect the source slide thoroughly — note all the user's manual edits.
2. Clone the source frame. Place the clone adjacent:
   - For multi-page layout: directly below the original with normal slide gap
   - For single-page sections: below the original within the same section
3. Rename the clone clearly:
   - Original: `01 — Color Overview`
   - Variant: `01b — Color Overview (variant)` OR `01b — Color Overview (applied)` if the variant has a clear distinguishing approach
4. Apply the requested changes to the variant only — never touch the original.
5. Visual QA the variant. Ensure both slides are visible and easy to compare.
6. Report: "Created variant `01b — Color Overview (variant)` next to the original. Original preserved."

**Cloning rules:**

- Use `frame.clone()` to get an exact copy including all bound styles, variables, and component instances
- After cloning, set `clone.x` and `clone.y` to the new position
- Update `clone.name` immediately

## Pattern 6: Build Live Do/Don't Spread from File Assets

**Trigger:** "Make a do/don't using the photos in the file" or "use the actual illustrations to show what's wrong"

**Why:** Generic don't cards are abstract. Real assets teaching real lessons are stronger.

**Steps:**

1. Find the relevant assets in the file. For photography: look on a Photography page. For illustration: look on an Illustration page or component set.
2. Inspect 2-4 candidate assets. Pick examples that clearly illustrate the rule.
3. Plan the spread:
   - Top row or left side: 2-3 DO examples
   - Bottom row or right side: 2-3 DON'T examples
4. For each example:
   - Clone or instance the asset into the slide
   - Add a clear caption ("Natural light" / "Heavy filtering")
   - Add a DO or DON'T badge (small pill, green or red)
5. For DON'T examples that need illustration of misuse (e.g. wrong color treatment, wrong crop):
   - Clone the original asset
   - Apply the misuse (recolor, crop oddly, distort)
   - Caption the specific problem
6. Visual QA — verify the lesson is unmistakable. The DO and DON'T should be obvious from glancing.

**Asset placement:**

- Use `clone()` for raster images and complex frames
- Use `createInstance()` for components
- Resize uniformly across DO/DON'T pairs so they're visually comparable

## Pattern 7: Tighten the Right Side Without Touching the Left

**Trigger:** "The copy is fine, just clean up the right side" or "keep my edits, fix the visuals"

**Steps:**

1. Inspect the slide. Identify the left column (auto-layout frame) and the right column content.
2. Confirm the left column has user edits — read the text. Do NOT modify it.
3. Identify the right-column content. Common cleanups:
   - Realign blocks to a consistent grid
   - Fix spacing inconsistencies between examples
   - Apply text styles to right-column text that's currently raw
   - Bind right-column fills to color variables
   - Rebuild a problematic visual using a building block
4. Replace only the right-column children. Leave the left column alone.
5. Visual QA — confirm the left column is unchanged and the right side is improved.

## Pattern 8: Apply Token Bindings to an Existing Slide

**Trigger:** "Bind the colors and text styles" or "this slide should use the variables"

**Steps:**

1. Inspect the slide. Find any node with raw hex fills or manual font settings.
2. Look up local variables and styles to find matches.
3. For each colored node:
   - Read the current fill hex
   - Find the matching color variable by hex match (or by closest match if exact isn't found)
   - Bind the variable using `figma.variables.setBoundVariableForPaint`
4. For each text node:
   - Read current font/size/weight
   - Find the matching text style by spec
   - Apply with `node.textStyleId = matchingStyle.id`
5. If a match isn't found, leave the raw value and log it for the user.
6. Visual QA — verify the slide looks identical (binding shouldn't change appearance).

See `references/layout-spec.md` "Token Binding — Code Reference" for the binding helpers.

## Pattern 9: Reframe a Section as Single Combined Page

**Trigger:** "Combine all the photography slides into one big page" or "make this section one continuous layout"

**Why:** Some sections (especially Photography or Illustration) read better as a single long-scroll page than as discrete slides.

**Steps:**

1. Inspect the section's current slides. Read the content of each.
2. Plan a single tall frame with stacked content blocks:
   - Hero block at the top (large title + intro)
   - Content blocks below, separated by visual breaks
   - Each block teaches one principle, with copy on the left and visual on the right
3. Build the new combined frame on the same page or a new dedicated page.
4. Place existing slide content into blocks rather than recreating it.
5. Name the new frame clearly: `Photography — Combined`.
6. Keep the original slides unless the user asks to delete them.
7. Visual QA on the long frame — check vertical rhythm and that no block feels squashed.

## Pattern 10: Repair a Slide with Visible Issues from Screenshot

**Trigger:** Visual QA reveals issues you need to fix without user feedback (clipping, overlap, etc.)

**Steps:**

1. Take a screenshot.
2. Identify specific issues:
   - Text clipped at the bottom of a frame → frame too short or text too large
   - Text overlapping another node → broken auto layout
   - Visual imbalance → uneven spacing or sizing
   - Missing content → asset failed to load or wasn't bound
3. Fix the most impactful issue first. Don't try to fix everything at once.
4. Take a new screenshot. Confirm the fix worked.
5. Iterate up to 2-3 times. If it still looks broken after that, surface the issue to the user instead of continuing to retry.

## Refinement Mode Summary

The refinement workflow is:

```
INSPECT → PLAN → EXECUTE → QA → REPORT
```

The most common mistake is jumping to EXECUTE without INSPECT. Always inspect
the live state first, especially when the user has been editing.

When uncertain whether to edit in place or create a variant, choose variant.
The user can always delete a variant they don't want, but they can't recover
edits that were overwritten.
