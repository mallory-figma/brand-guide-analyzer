# Brand Book Layout Specification — Editorial Format

This reference defines the visual layout for generating Brand Book documentation
in Figma. All slides follow a consistent editorial format: white background,
left-column copy, right-column visuals, and a persistent footer.

Read this file before generating any Figma slides. Every dimension, font choice,
and spacing value here is intentional and must be followed exactly.

---

## Canvas & Slide Dimensions

| Property | Value |
|---|---|
| Slide width | 1440px |
| Slide height | 810px |
| Slide gap (vertical, between slides on canvas) | 80px |
| Background color | #FFFFFF |
| Slide corner radius | 0 (flat frames) |

---

## Column Layout

| Property | Value |
|---|---|
| Left padding (LP) | 64px |
| Left column width | 360px |
| Left column x position | 64px |
| Right column start (RX) | 480px |
| Right column end | 1440 − 64 = 1376px |
| Right column width (RW) | 1440 − 480 − 64 = 896px |

The left column contains all written guidelines copy. The right column contains
all visual examples — swatches, specimen blocks, example compositions, grids.

### Left column — MUST use auto layout

The left column MUST be built as a vertical auto layout frame to prevent text
overlaps. Never manually position text nodes with hardcoded y values on the left
side — text wraps differently depending on content length and will overlap.

```javascript
// Create the left column as an auto layout frame
const leftCol = figma.createFrame();
leftCol.name = "Left Column";
leftCol.resize(360, 700); // height will be overridden by auto layout
leftCol.x = 64;
leftCol.y = 40;
leftCol.fills = []; // transparent
leftCol.layoutMode = "VERTICAL";
leftCol.primaryAxisSizingMode = "AUTO"; // height hugs content
leftCol.counterAxisSizingMode = "FIXED"; // width stays at 360
leftCol.itemSpacing = 20; // default gap between elements
leftCol.paddingTop = 0;
leftCol.paddingBottom = 0;
leftCol.paddingLeft = 0;
leftCol.paddingRight = 0;
slide.appendChild(leftCol);
```

All text nodes in the left column should have:
- `textAutoResize = "HEIGHT"` — so they wrap and grow vertically
- `layoutSizingHorizontal = "FILL"` — so they stretch to the column width
- `layoutSizingVertical = "HUG"` — so they take only the space they need

```javascript
// Adding text to the auto layout left column
const heading = figma.createText();
heading.characters = "Color\nOverview";
heading.fontSize = 48;
// ... font settings ...
heading.textAutoResize = "HEIGHT";
heading.layoutSizingHorizontal = "FILL";
heading.layoutSizingVertical = "HUG";
leftCol.appendChild(heading);

const body = figma.createText();
body.characters = "Guidelines body copy goes here...";
body.fontSize = 15;
body.lineHeight = { value: 24, unit: "PIXELS" };
// ... font settings ...
body.textAutoResize = "HEIGHT";
body.layoutSizingHorizontal = "FILL";
body.layoutSizingVertical = "HUG";
leftCol.appendChild(body);
```

To add extra spacing between specific elements (e.g. more space after the title),
use a spacer frame or adjust the spacing on individual items:

```javascript
// Add a divider with extra spacing above it
const divider = figma.createRectangle();
divider.resize(360, 1);
divider.fills = [{ type: "SOLID", color: { r: 0, g: 0, b: 0 }, opacity: 0.08 }];
divider.layoutSizingHorizontal = "FILL";
leftCol.appendChild(divider);
```

For bullet lists, create each bullet as a horizontal auto layout frame containing
the bullet character and the text:

```javascript
const bulletRow = figma.createFrame();
bulletRow.layoutMode = "HORIZONTAL";
bulletRow.itemSpacing = 8;
bulletRow.fills = [];
bulletRow.layoutSizingHorizontal = "FILL";
bulletRow.layoutSizingVertical = "HUG";

const dot = figma.createText();
dot.characters = "•";
dot.fontSize = 15;
dot.layoutSizingVertical = "HUG";
bulletRow.appendChild(dot);

const bulletText = figma.createText();
bulletText.characters = "Rule text goes here...";
bulletText.fontSize = 14;
bulletText.textAutoResize = "HEIGHT";
bulletText.layoutSizingHorizontal = "FILL";
bulletText.layoutSizingVertical = "HUG";
bulletRow.appendChild(bulletText);

leftCol.appendChild(bulletRow);
```

### Right column

The right column can use either auto layout or manual positioning depending on
the content type. Grids of swatches, specimen blocks, and card layouts often
work better with manual positioning. Simple vertical stacks of example blocks
can use auto layout.

---

## Typography

Use the brand's own typefaces when available. When brand fonts are not loaded
in Figma, fall back to this stack:

| Role | Fallback Font | Style | Size |
|---|---|---|---|
| Section title (large) | Inter Bold (or brand display) | Bold | 48–56px |
| Section subtitle | Inter Medium (or brand body) | Medium | 18px |
| Subsection heading | Inter SemiBold | SemiBold | 24px |
| Body copy (guidelines text) | Inter Medium (or brand body) | Medium/Regular | 15px |
| Bullet list items | Same as body | Medium/Regular | 14px |
| Labels and captions | Same as body | Medium | 11–12px |
| Token names / code | Monospace or brand mono | Medium | 12px |
| Footer text | Same as body | Medium | 13px |

### Title treatment
- Letter spacing: −1.5px to −2px at display sizes
- Line height: ~108% of font size (tight)
- Color: #000000

### Body copy treatment
- Line height: 24px (1.6×)
- Color: #000000 at 60% opacity
- Max width: left column width minus 16px
- textAutoResize: HEIGHT

### Label treatment
- Color: #000000 at 30–45% opacity
- Letter spacing: 0.5–1.5px when uppercase

---

## Footer

Every slide includes a persistent footer:

| Element | Position | Style |
|---|---|---|
| Brand name ("Cheddar Brand Book") | x=64, y=774 | 13px, #000 at 40% opacity |
| Section label ("Color", "Typography", etc.) | x=1176, y=774, right-aligned (200px wide) | 13px, #000 at 40% opacity |
| Divider line | Full width, y=762 | 1px, #000 at 8% opacity |

---

## Swatch & Color Block Specifications

### Palette swatch grid
- Grid: 6 columns
- Swatch card: rounded rect, cornerRadius=0 or 8px
- Color block height: 72–120px (scales with available space)
- Info area below color block: hex value, step name, optional role tag
- Border: 1px #000 at 6% opacity

### Background color blocks (large)
- 2×3 or 3×2 grid in the right column
- Corner radius: 8px
- Include: color name, hex, usage note
- Text color: white on dark backgrounds, black on light

### Color pairing cards
- Show "Aa" specimen text on the background
- Include WCAG badge (✓ AA or ✓ AAA)
- Corner radius: 8–12px

---

## Do / Don't Cards

### Correct examples (Do)
- Left border: 4px, #D4FFD4 (light green)
- Background: #F8F8F8 or #F5F5F5
- Corner radius: 12–16px
- Caption below: ✅ + description, 11px, #000 at 45% opacity

### Incorrect examples (Don't)
- Left border: 4px, #FFD4D4 (light red)
- Background: #F8F8F8 or #F5F5F5 (or the offending color if illustrating a color mistake)
- Corner radius: 12–16px
- Caption below: ❌ + description, 11px, #000 at 45% opacity

### Badge styles
- DO badge: background #22CC66 at 15%, text #22CC66, 10px, pill shape
- DON'T badge: background #FF3333 at 12%, text #FF3333, 10px, pill shape

---

## Section Slide Templates

Each brand section generates a predictable set of slides. The skill should
generate slides based on what sections were detected in the PDF analysis.

### Color Section (generate when section type = "color")
1. **Color Overview** — Intro + 4 hero color blocks
2. **Color Schemes** — Grouped palette rows (primary, secondary, campaign, etc.)
3. **Background Colors** — Approved BG color blocks with usage notes
4. **Palette Specs** — Full swatch grid with hex, role labels
5. **Color Pairings** — Approved text-on-background combos with WCAG badges
6. **Neutrals** — Neutral palette blocks with usage rules
7. **What to Avoid** — 4-column Don't examples
8. **Color In Use** — Example compositions (requires user-provided examples)

### Typography Section (generate when section type = "typography")
1. **Typography Overview** — Intro + hero composition showing type in context
2. **Primary Typeface** — Specimen blocks on dark backgrounds
3. **Titles & Headlines** — Rules + example headline blocks
4. **Body & UI** — Rules + subhead/body pairing examples
5. **Sizing & Spacing** — Type scale table
6. **Type on Color** — Type-on-background pairings across brand palette
7. **Typographic Guidance (Do)** — 2×2 correct examples
8. **Typographic Guidance (Don't)** — 2×2 incorrect examples

### Logo Section (generate when section type = "logo")
1. **Logo Overview** — Intro + large logo display
2. **Logo Variations** — All approved versions (wordmark, mark, icon)
3. **Color Variations** — Logo on approved backgrounds with colorway rules
4. **Clear Space & Minimum Size** — Diagram + minimum size examples
5. **Logo Placement** — Context examples (nav, social, print)
6. **What Not to Do** — 2×3 Don't grid
7. **Logo In Use** — Real examples (requires user-provided designs)

### Illustration Section (generate when section type = "illustration")
1. **Illustration Overview** — Intro + hero grid of illustration assets
2. **Illustration Library** — Full asset grid
3. **Background Elements** — Rules for organic shapes, patterns, textures
4. **Composition Rules** — Do/Don't layout examples
5. **Illustration on Color** — Same illustration on multiple brand backgrounds
6. **What Not to Do** — 2×3 Don't grid

### Photography Section (generate when section type = "photography")
1. **Photography Overview** — Art direction principles
2. **Shot Types** — Approved photo styles with example blocks
3. **Treatment Rules** — Color grading, cropping, composition
4. **Photography on Color** — Photos within brand-colored layouts
5. **What Not to Do** — Incorrect photo treatments

### Other Sections
For sections typed as "copy", "spacing", "motion", "iconography", or "other":
Generate 2–4 slides using the standard left/right layout with rules on the left
and visual examples on the right. Adapt content based on the section's notes
and sub-sections from the analysis JSON.

---

## Figma Implementation Notes

### Font loading
Always call `figma.loadFontAsync()` before creating text nodes. Load the brand's
actual fonts first; if they fail, fall back to Inter.

### Component instances
When logo, illustration, or icon components exist in the file, use
`component.createInstance()` to place them in slides rather than recreating them.
This maintains live links to the source components.

### Frame naming
Name every slide frame with its number and title:
`"01 — Color Overview"`, `"02 — Color Schemes"`, etc.
Prefix the page name with 📖 and the section name.

### Cloning user examples
When the user provides design examples (website pages, social posts, campaign
assets), use `.clone()` to copy them into guideline slides. Scale with `.resize()`
and set `clipsContent = true` with `cornerRadius = 8`.

---

## Token Creation — Code Reference

### Creating a flat variable collection (no modes)

```javascript
const collection = figma.variables.createVariableCollection("Brand — Colors");
const variable = figma.variables.createVariable("Blue/200", collection.id, "COLOR");
variable.setValueForMode(collection.modes[0].modeId, { r: 0, g: 0, b: 0.8 });
```

### Creating Primitives + Semantic collections (with modes)

```javascript
// ── PRIMITIVES ──
const prims = figma.variables.createVariableCollection("Brand — Primitives");
prims.renameMode(prims.modes[0].modeId, "Value");

const blue200 = figma.variables.createVariable("Blue/200", prims.id, "COLOR");
blue200.setValueForMode(prims.modes[0].modeId, { r: 0, g: 0, b: 0.8 });
const magenta400 = figma.variables.createVariable("Magenta/400", prims.id, "COLOR");
magenta400.setValueForMode(prims.modes[0].modeId, { r: 0.94, g: 0.38, b: 0.69 });
// ... repeat for all colors

// ── SEMANTIC ──
const semantic = figma.variables.createVariableCollection("Brand — Semantic");
semantic.renameMode(semantic.modes[0].modeId, "Primary");
const campaignModeId = semantic.addMode("Campaign");

const brandBg = figma.variables.createVariable("brand/background", semantic.id, "COLOR");
brandBg.setValueForMode(semantic.modes[0].modeId, {
  type: "VARIABLE_ALIAS", id: blue200.id
});
brandBg.setValueForMode(campaignModeId, {
  type: "VARIABLE_ALIAS", id: magenta400.id
});
```

### Creating color styles

```javascript
const style = figma.createPaintStyle();
style.name = "Primary/Blue 200";
style.paints = [{ type: "SOLID", color: { r: 0, g: 0, b: 0.8 } }];
```

### Creating text styles

```javascript
const style = figma.createTextStyle();
style.name = "Headline/Display Large";
style.fontName = { family: "Oswald", style: "SemiBold" };
style.fontSize = 80;
style.letterSpacing = { value: -3.2, unit: "PIXELS" };
style.lineHeight = { value: 100, unit: "PERCENT" };
```

---

## Token Binding — Code Reference

All Brand Book slide elements MUST bind to the variables, styles, and text styles
created in Phase 3. Never use raw hex values or manual font settings in slides.
This section contains the code patterns to use.

### Lookup helpers (establish at the start of each code block)

```javascript
const collections = figma.variables.getLocalVariableCollections();
const primColl = collections.find(c => c.name.includes("Primitives") || c.name.includes("Colors"));
const allColorVars = primColl ? primColl.variableIds.map(id => figma.variables.getVariableById(id)) : [];
function getColorVar(name) { return allColorVars.find(v => v.name === name); }

const paintStyles = figma.getLocalPaintStyles();
function getColorStyle(name) { return paintStyles.find(s => s.name.includes(name)); }

const textStyles = figma.getLocalTextStyles();
function getTextStyle(name) { return textStyles.find(s => s.name.includes(name)); }
```

### Binding color variables to fills

```javascript
// Bind a node's fill to a color variable
function bindFill(node, varName) {
  const v = getColorVar(varName);
  if (v) {
    const modeId = Object.keys(v.valuesByMode)[0];
    const rawColor = v.valuesByMode[modeId];
    const paint = { type: "SOLID", color: rawColor };
    node.fills = [figma.variables.setBoundVariableForPaint(paint, "color", v)];
  }
}

// Usage:
bindFill(swatchRect, "Blue/200");
bindFill(backgroundFrame, "Green/400");
```

### Binding color styles to fills

```javascript
// Bind a node's fill to a paint style
function bindFillStyle(node, styleName) {
  const s = getColorStyle(styleName);
  if (s) node.fillStyleId = s.id;
}

// Usage:
bindFillStyle(swatchRect, "Blue 200");
```

### Applying text styles

```javascript
// Apply a text style to a text node
function applyTextStyle(textNode, styleName) {
  const s = getTextStyle(styleName);
  if (s) textNode.textStyleId = s.id;
}

// Usage:
applyTextStyle(headlineNode, "Display Large");
applyTextStyle(bodyNode, "Body/Large");
```

### What to bind where

| Slide element | Color binding | Text style binding |
|---|---|---|
| Palette swatch fill | Primitive variable (e.g. `Blue/200`) | — |
| Background example block | Primitive or semantic variable | — |
| Combo preview background | Primitive variable | — |
| Type-on-color background | Primitive variable | — |
| Organic shape fill | Primitive variable | — |
| Do/Don't card brand color | Primitive variable | — |
| Slide title | — | Headline display style |
| Slide subtitle | — | Headline small or subhead style |
| Guidelines body copy | — | Body/Large style |
| Bullet rules | — | Body/Medium style |
| Labels and captions | — | Caption style |
| Footer text | — | Caption style |
| Specimen headlines | — | The display style being documented |
| Specimen body text | — | The body style being documented |
| Token name / code text | — | Mono style (if available) |

### Fallback behavior

If a variable or style lookup returns null (e.g. the name doesn't match exactly),
fall back to raw values with a console warning. This can happen if fonts fail to
load or if variable names have slight mismatches. The slide should still render —
it just won't be bound to the token system until manually fixed.

```javascript
function bindFillSafe(node, varName, fallbackHex) {
  const v = getColorVar(varName);
  if (v) {
    const modeId = Object.keys(v.valuesByMode)[0];
    const paint = { type: "SOLID", color: v.valuesByMode[modeId] };
    node.fills = [figma.variables.setBoundVariableForPaint(paint, "color", v)];
  } else {
    // Fallback to raw hex
    const r = parseInt(fallbackHex.slice(1,3),16)/255;
    const g = parseInt(fallbackHex.slice(3,5),16)/255;
    const b = parseInt(fallbackHex.slice(5,7),16)/255;
    node.fills = [{ type: "SOLID", color: { r, g, b } }];
  }
}
```

---

## Building Block Functions — Full Code

These are the complete implementations of every building block referenced in
slide-code-templates.md. Include the relevant functions in each use_figma call.

### buildHeroColorBlocks

```javascript
async function buildHeroColorBlocks(slide, colors) {
  await figma.loadFontAsync({family:"Inter",style:"Medium"});
  const blockW=(RW-(colors.length-1)*8)/colors.length;
  colors.forEach((c,i)=>{
    const block=F(blockW,682,c.hex,slide);
    block.x=RX+i*(blockW+8); block.y=64;
    const lbl=figma.createText();
    lbl.fontName={family:"Inter",style:"Medium"};
    lbl.characters=c.name; lbl.fontSize=13;
    lbl.fills=[{type:"SOLID",color:h(isDarkBg(c.hex)?"#FFFFFF":"#000000")}];
    lbl.x=16; lbl.y=650; block.appendChild(lbl);
  });
}
```

### buildSwatchGrid

```javascript
async function buildSwatchGrid(slide, families) {
  await figma.loadFontAsync({family:"Inter",style:"Medium"});
  await figma.loadFontAsync({family:"Inter",style:"Bold"});
  const maxCols=Math.max(...families.map(f=>f.colors.length));
  const cols=Math.min(maxCols,8);
  const gap=8;
  const swW=(RW-(cols-1)*gap)/cols;
  const swH=72, infoH=50;
  families.forEach((fam,fi)=>{
    const famY=50+fi*(swH+infoH+36);
    const famLbl=figma.createText();
    famLbl.fontName={family:"Inter",style:"Bold"};
    famLbl.characters=fam.name.toUpperCase();
    famLbl.fontSize=11; famLbl.letterSpacing={value:1.5,unit:"PIXELS"};
    famLbl.fills=[{type:"SOLID",color:h("#000000"),opacity:0.25}];
    famLbl.x=RX; famLbl.y=famY; slide.appendChild(famLbl);
    fam.colors.forEach((sw,ci)=>{
      const card=F(swW,swH+infoH,null,slide,8);
      card.strokes=[{type:"SOLID",color:h("#000000"),opacity:0.06}];
      card.strokeWeight=1;
      card.x=RX+ci*(swW+gap); card.y=famY+20;
      const cb=figma.createRectangle(); cb.resize(swW,swH);
      cb.fills=[{type:"SOLID",color:h(sw.hex)}];
      if(sw.hex==="#FFFFFF"||sw.hex==="#ffffff"){cb.strokes=[{type:"SOLID",color:h("#E0E0E0")}];cb.strokeWeight=1;}
      card.appendChild(cb);
      const sl=figma.createText(); sl.fontName={family:"Inter",style:"Bold"};
      sl.characters=(fam.name+" "+sw.step)||sw.hex; sl.fontSize=11;
      sl.fills=[{type:"SOLID",color:h("#000000"),opacity:0.5}];
      sl.x=8; sl.y=swH+6; card.appendChild(sl);
      const hl=figma.createText(); hl.fontName={family:"Inter",style:"Medium"};
      hl.characters=sw.hex; hl.fontSize=10;
      hl.fills=[{type:"SOLID",color:h("#000000"),opacity:0.3}];
      hl.x=8; hl.y=swH+22; card.appendChild(hl);
      if(sw.role){
        const rl=figma.createText(); rl.fontName={family:"Inter",style:"Bold"};
        rl.characters=sw.role; rl.fontSize=9;
        rl.fills=[{type:"SOLID",color:h("#0000CC")}];
        rl.x=8; rl.y=swH+36; card.appendChild(rl);
      }
    });
  });
}
```

### buildColorBlockGrid

```javascript
async function buildColorBlockGrid(slide, colors, cols, startY) {
  await figma.loadFontAsync({family:"Inter",style:"Medium"});
  cols=cols||3; startY=startY||64;
  const gap=16;
  const bw=(RW-(cols-1)*gap)/cols;
  const rows=Math.ceil(colors.length/cols);
  const bh=(810-startY-80-(rows-1)*gap)/rows;
  colors.forEach((c,i)=>{
    const col=i%cols, row=Math.floor(i/cols);
    const block=F(bw,bh,c.hex,slide,8);
    block.x=RX+col*(bw+gap); block.y=startY+row*(bh+gap);
    if(c.hex==="#FFFFFF"||c.hex==="#ffffff"){block.strokes=[{type:"SOLID",color:h("#E0E0E0")}];block.strokeWeight=1;}
    const dark=isDarkBg(c.hex); const tc=dark?"#FFFFFF":"#000000";
    const nm=figma.createText(); nm.fontName={family:"Inter",style:"Medium"};
    nm.characters=c.name||c.hex; nm.fontSize=14;
    nm.fills=[{type:"SOLID",color:h(tc)}]; nm.x=20; nm.y=bh-52; block.appendChild(nm);
    if(c.note){const nt=figma.createText(); nt.fontName={family:"Inter",style:"Medium"};
    nt.characters=c.note; nt.fontSize=12; nt.fills=[{type:"SOLID",color:h(tc),opacity:0.6}];
    nt.x=20; nt.y=bh-30; block.appendChild(nt);}
  });
}
```

### buildPairingCards

```javascript
async function buildPairingCards(slide, pairings, cols) {
  await figma.loadFontAsync({family:"Inter",style:"Bold"});
  await figma.loadFontAsync({family:"Inter",style:"Medium"});
  cols=cols||3; const gap=16;
  const cw=(RW-(cols-1)*gap)/cols;
  const rows=Math.ceil(pairings.length/cols);
  const ch=(810-128-(rows-1)*gap)/rows;
  pairings.forEach((p,i)=>{
    const col=i%cols, row=Math.floor(i/cols);
    const card=F(cw,ch,p.bg,slide,12);
    card.x=RX+col*(cw+gap); card.y=64+row*(ch+gap);
    const aa=figma.createText(); aa.fontName={family:"Inter",style:"Bold"};
    aa.characters="Aa"; aa.fontSize=Math.min(56,ch*0.35);
    aa.fills=[{type:"SOLID",color:h(p.text)}]; aa.x=24; aa.y=20; card.appendChild(aa);
    const lbl=figma.createText(); lbl.fontName={family:"Inter",style:"Medium"};
    lbl.characters=p.label; lbl.fontSize=11;
    lbl.fills=[{type:"SOLID",color:h(p.text),opacity:0.6}];
    lbl.x=24; lbl.y=ch-28; card.appendChild(lbl);
    if(p.wcag){const badge=F(50,20,null,card,100);
    badge.fills=[{type:"SOLID",color:h(p.text),opacity:0.12}];
    badge.x=cw-66; badge.y=12;
    const bt=figma.createText(); bt.fontName={family:"Inter",style:"Bold"};
    bt.characters="✓ "+p.wcag; bt.fontSize=9;
    bt.fills=[{type:"SOLID",color:h(p.text),opacity:0.7}];
    bt.x=8; bt.y=4; badge.appendChild(bt);}
  });
}
```

### buildSpecimenBlocks

```javascript
async function buildSpecimenBlocks(slide, specimens, cols, rows) {
  cols=cols||3; rows=rows||2;
  const gap=8; const sw=(RW-(cols-1)*gap)/cols;
  const sh=(810-128-(rows-1)*gap)/rows;
  for(let i=0;i<specimens.length&&i<cols*rows;i++){
    const sp=specimens[i]; const col=i%cols, row=Math.floor(i/cols);
    await figma.loadFontAsync({family:sp.fontFamily,style:sp.fontStyle});
    const block=F(sw,sh,sp.bg||"#0A0A0A",slide,12);
    block.x=RX+col*(sw+gap); block.y=40+row*(sh+gap);
    const t=figma.createText(); t.fontName={family:sp.fontFamily,style:sp.fontStyle};
    t.characters=sp.chars; t.fontSize=sp.size||64;
    t.lineHeight={value:sp.size||64,unit:"PIXELS"};
    t.letterSpacing={value:-(sp.size||64)*0.04,unit:"PIXELS"};
    t.fills=[{type:"SOLID",color:h(sp.fg||"#FFFFFF")}];
    t.x=24; t.y=Math.floor(sh/2)-Math.floor((sp.size||64)/2);
    block.appendChild(t);
  }
}
```

### buildTypeScaleTable

```javascript
async function buildTypeScaleTable(slide, tokens) {
  await figma.loadFontAsync({family:"Inter",style:"Medium"});
  await figma.loadFontAsync({family:"Inter",style:"Semi Bold"});
  const tableX=RX, tableW=RW;
  const headers=["Token","Font","Size","Line Height","Letterspacing"];
  const colWidths=[180,180,80,100,tableW-540];
  const rowH=44;
  let hx=tableX;
  headers.forEach((header,i)=>{
    const ht=figma.createText(); ht.fontName={family:"Inter",style:"Semi Bold"};
    ht.characters=header.toUpperCase(); ht.fontSize=10;
    ht.letterSpacing={value:1.2,unit:"PIXELS"};
    ht.fills=[{type:"SOLID",color:h("#000000"),opacity:0.28}];
    ht.x=hx+4; ht.y=54; slide.appendChild(ht); hx+=colWidths[i];
  });
  const hLine=figma.createRectangle(); hLine.resize(tableW,1);
  hLine.fills=[{type:"SOLID",color:h("#000000"),opacity:0.1}];
  hLine.x=tableX; hLine.y=74; slide.appendChild(hLine);
  tokens.forEach((tok,ri)=>{
    const rowY=84+ri*rowH;
    if(ri%2===0){const bg=figma.createRectangle(); bg.resize(tableW,rowH-2);
    bg.fills=[{type:"SOLID",color:h("#000000"),opacity:0.02}]; bg.x=tableX; bg.y=rowY; slide.appendChild(bg);}
    const cells=[tok.token,tok.font,tok.size,tok.lineHeight,tok.letterSpacing];
    let cx=tableX;
    cells.forEach((cell,ci)=>{const ct=figma.createText();
    ct.fontName={family:"Inter",style:ci===0?"Semi Bold":"Medium"};
    ct.characters=cell||""; ct.fontSize=ci===0?12:13;
    ct.fills=[{type:"SOLID",color:h("#000000"),opacity:ci===0?0.8:0.55}];
    ct.x=cx+4; ct.y=rowY+14; slide.appendChild(ct); cx+=colWidths[ci];});
    const rLine=figma.createRectangle(); rLine.resize(tableW,1);
    rLine.fills=[{type:"SOLID",color:h("#000000"),opacity:0.04}];
    rLine.x=tableX; rLine.y=rowY+rowH; slide.appendChild(rLine);
  });
}
```

### buildHeadlineExamples

```javascript
async function buildHeadlineExamples(slide, examples) {
  const gap=16; const totalH=810-128;
  const blockH=(totalH-(examples.length-1)*gap)/examples.length;
  for(let i=0;i<examples.length;i++){
    const ex=examples[i];
    await figma.loadFontAsync({family:ex.fontFamily,style:ex.fontStyle});
    const block=F(RW,blockH,"#F5F5F5",slide,12);
    block.x=RX; block.y=40+i*(blockH+gap);
    const t=figma.createText(); t.fontName={family:ex.fontFamily,style:ex.fontStyle};
    t.characters=ex.text; t.fontSize=ex.size||48;
    t.lineHeight={value:(ex.size||48)+4,unit:"PIXELS"};
    t.letterSpacing={value:-((ex.size||48)*0.03),unit:"PIXELS"};
    t.fills=[{type:"SOLID",color:h(ex.color||"#000000")}];
    t.textAutoResize="HEIGHT"; t.resize(RW-80,100);
    t.x=40; t.y=Math.floor(blockH/2)-Math.floor((ex.size||48)/2);
    block.appendChild(t);
  }
}
```

### buildBodyExamples

```javascript
async function buildBodyExamples(slide, examples, cols) {
  await figma.loadFontAsync({family:"Inter",style:"Medium"});
  await figma.loadFontAsync({family:"Inter",style:"Semi Bold"});
  cols=cols||2; const gap=12;
  const cw=(RW-(cols-1)*gap)/cols;
  const rows=Math.ceil(examples.length/cols);
  const ch=(810-128-(rows-1)*gap)/rows;
  examples.forEach((ex,i)=>{
    const col=i%cols, row=Math.floor(i/cols);
    const card=F(cw,ch,"#F5F5F5",slide,12);
    card.x=RX+col*(cw+gap); card.y=40+row*(ch+gap);
    const hd=figma.createText(); hd.fontName={family:"Inter",style:"Semi Bold"};
    hd.characters=ex.subhead; hd.fontSize=ex.subSize||24;
    hd.fills=[{type:"SOLID",color:h("#000000")}]; hd.x=24; hd.y=24; card.appendChild(hd);
    const bd=figma.createText(); bd.fontName={family:"Inter",style:"Medium"};
    bd.characters=ex.body; bd.fontSize=ex.bodySize||14;
    bd.lineHeight={value:20,unit:"PIXELS"};
    bd.fills=[{type:"SOLID",color:h("#000000"),opacity:0.55}];
    bd.textAutoResize="HEIGHT"; bd.resize(cw-48,100);
    bd.x=24; bd.y=(ex.subSize||24)+40; card.appendChild(bd);
  });
}
```

### buildComponentGrid

```javascript
async function buildComponentGrid(slide, nodeIds, cols, bg) {
  cols=cols||3; const gap=12;
  const cellW=(RW-(cols-1)*gap)/cols;
  const rows=Math.ceil(nodeIds.length/cols);
  const cellH=Math.min((810-128-(rows-1)*gap)/rows,300);
  for(let i=0;i<nodeIds.length;i++){
    const col=i%cols, row=Math.floor(i/cols);
    const cell=F(cellW,cellH,bg||"#F5F5F5",slide,8);
    cell.x=RX+col*(cellW+gap); cell.y=40+row*(cellH+gap);
    const comp=await figma.getNodeByIdAsync(nodeIds[i]);
    if(comp&&comp.createInstance){
      const inst=comp.createInstance();
      const scale=Math.min((cellW-40)/inst.width,(cellH-40)/inst.height);
      inst.resize(inst.width*scale,inst.height*scale);
      inst.x=Math.floor((cellW-inst.width*scale)/2);
      inst.y=Math.floor((cellH-inst.height*scale)/2);
      cell.appendChild(inst);
    }
  }
}
```

### buildOnBackgrounds

```javascript
async function buildOnBackgrounds(slide, nodeId, backgrounds, cols) {
  await figma.loadFontAsync({family:"Inter",style:"Medium"});
  cols=cols||3; const gap=12;
  const bw=(RW-(cols-1)*gap)/cols;
  const rows=Math.ceil(backgrounds.length/cols);
  const bh=(810-128-(rows-1)*gap)/rows;
  const comp=await figma.getNodeByIdAsync(nodeId);
  backgrounds.forEach((bg,i)=>{
    const col=i%cols, row=Math.floor(i/cols);
    const block=F(bw,bh,bg.hex,slide,8);
    block.x=RX+col*(bw+gap); block.y=40+row*(bh+gap);
    if(comp&&comp.createInstance){
      const inst=comp.createInstance();
      const scale=Math.min((bw-48)/inst.width,(bh-60)/inst.height);
      inst.resize(inst.width*scale,inst.height*scale);
      inst.x=Math.floor((bw-inst.width*scale)/2);
      inst.y=Math.floor((bh-inst.height*scale)/2)-10;
      block.appendChild(inst);
    }
    const dark=isDarkBg(bg.hex);
    const lbl=figma.createText(); lbl.fontName={family:"Inter",style:"Medium"};
    lbl.characters=bg.label; lbl.fontSize=11;
    lbl.fills=[{type:"SOLID",color:h(dark?"#FFFFFF":"#000000"),opacity:0.45}];
    lbl.x=14; lbl.y=bh-24; block.appendChild(lbl);
  });
}
```

### buildDontCards

```javascript
async function buildDontCards(slide, donts, cols) {
  await figma.loadFontAsync({family:"Inter",style:"Bold"});
  await figma.loadFontAsync({family:"Inter",style:"Medium"});
  cols=cols||2; const gap=16;
  const dw=(RW-(cols-1)*gap)/cols;
  const rows=Math.ceil(donts.length/cols);
  const dh=(810-128-(rows-1)*gap)/rows;
  donts.forEach((d,i)=>{
    const col=i%cols, row=Math.floor(i/cols);
    const card=F(dw,dh,null,slide,16);
    card.strokes=[{type:"SOLID",color:h("#000000"),opacity:0.06}]; card.strokeWeight=1;
    card.x=RX+col*(dw+gap); card.y=40+row*(dh+gap);
    const border=figma.createRectangle(); border.resize(4,dh);
    border.fills=[{type:"SOLID",color:h("#FFD4D4")}]; card.appendChild(border);
    const prevH=Math.floor(dh*0.55);
    const prev=F(dw,prevH,d.bg||"#F5F5F5",card);
    const pt=figma.createText(); pt.fontName={family:"Inter",style:"Bold"};
    pt.characters=d.preview; pt.fontSize=20;
    pt.fills=[{type:"SOLID",color:h(d.fg||"#CCCCCC")}];
    pt.textAlignHorizontal="CENTER"; pt.textAutoResize="HEIGHT";
    pt.resize(dw-32,60); pt.x=16; pt.y=Math.floor(prevH/2)-20; prev.appendChild(pt);
    const badge=F(56,20,null,card,100);
    badge.fills=[{type:"SOLID",color:h("#FF3333"),opacity:0.12}];
    badge.x=16; badge.y=prevH+12;
    const bt=figma.createText(); bt.fontName={family:"Inter",style:"Bold"};
    bt.characters="DON'T"; bt.fontSize=9; bt.letterSpacing={value:1,unit:"PIXELS"};
    bt.fills=[{type:"SOLID",color:h("#FF3333")}]; bt.x=8; bt.y=4; badge.appendChild(bt);
    const rt=figma.createText(); rt.fontName={family:"Inter",style:"Medium"};
    rt.characters=d.rule; rt.fontSize=12;
    rt.fills=[{type:"SOLID",color:h("#000000"),opacity:0.5}];
    rt.textAutoResize="HEIGHT"; rt.resize(dw-32,40);
    rt.x=16; rt.y=prevH+38; card.appendChild(rt);
  });
}
```

### buildDoCards

```javascript
async function buildDoCards(slide, dos, cols) {
  await figma.loadFontAsync({family:"Inter",style:"Bold"});
  await figma.loadFontAsync({family:"Inter",style:"Medium"});
  cols=cols||2; const gap=16;
  const dw=(RW-(cols-1)*gap)/cols;
  const rows=Math.ceil(dos.length/cols);
  const dh=(810-128-(rows-1)*gap)/rows;
  dos.forEach((d,i)=>{
    const col=i%cols, row=Math.floor(i/cols);
    const card=F(dw,dh,"#F8F8F8",slide,16);
    card.x=RX+col*(dw+gap); card.y=40+row*(dh+gap);
    const border=figma.createRectangle(); border.resize(4,dh);
    border.fills=[{type:"SOLID",color:h("#D4FFD4")}]; card.appendChild(border);
    if(d.headline){const hl=figma.createText(); hl.fontName={family:"Inter",style:"Bold"};
    hl.characters=d.headline; hl.fontSize=d.headSize||28;
    hl.fills=[{type:"SOLID",color:h("#000000")}]; hl.x=24; hl.y=24; card.appendChild(hl);}
    if(d.body){const bd=figma.createText(); bd.fontName={family:"Inter",style:"Medium"};
    bd.characters=d.body; bd.fontSize=14; bd.lineHeight={value:20,unit:"PIXELS"};
    bd.fills=[{type:"SOLID",color:h("#000000"),opacity:0.55}];
    bd.textAutoResize="HEIGHT"; bd.resize(dw-48,60);
    bd.x=24; bd.y=(d.headSize||28)+40; card.appendChild(bd);}
    if(d.caption){const ct=figma.createText(); ct.fontName={family:"Inter",style:"Medium"};
    ct.characters="✅  "+d.caption; ct.fontSize=11;
    ct.fills=[{type:"SOLID",color:h("#000000"),opacity:0.45}];
    ct.x=24; ct.y=dh-28; card.appendChild(ct);}
  });
}
```

### buildImagePlaceholders

```javascript
async function buildImagePlaceholders(slide, placeholders, cols) {
  await figma.loadFontAsync({family:"Inter",style:"Medium"});
  await figma.loadFontAsync({family:"Inter",style:"Semi Bold"});
  cols=cols||3; const gap=12;
  const pw=(RW-(cols-1)*gap)/cols;
  const rows=Math.ceil(placeholders.length/cols);
  const ph=(810-128-(rows-1)*gap)/rows;
  placeholders.forEach((p,i)=>{
    const col=i%cols, row=Math.floor(i/cols);
    const block=F(pw,ph,"#E8E8E8",slide,8);
    block.x=RX+col*(pw+gap); block.y=40+row*(ph+gap);
    const lbl=figma.createText(); lbl.fontName={family:"Inter",style:"Semi Bold"};
    lbl.characters=p.label; lbl.fontSize=12;
    lbl.fills=[{type:"SOLID",color:h("#000000"),opacity:0.5}];
    lbl.textAlignHorizontal="CENTER"; lbl.textAutoResize="HEIGHT";
    lbl.resize(pw-32,20); lbl.x=16; lbl.y=Math.floor(ph/2)-8; block.appendChild(lbl);
    if(p.note){const nt=figma.createText(); nt.fontName={family:"Inter",style:"Medium"};
    nt.characters=p.note; nt.fontSize=11;
    nt.fills=[{type:"SOLID",color:h("#000000"),opacity:0.3}];
    nt.textAlignHorizontal="CENTER"; nt.textAutoResize="HEIGHT";
    nt.resize(pw-32,20); nt.x=16; nt.y=Math.floor(ph/2)+12; block.appendChild(nt);}
  });
}
```

### buildMiniCompositions

```javascript
async function buildMiniCompositions(slide, examples) {
  await figma.loadFontAsync({family:"Inter",style:"Bold"});
  await figma.loadFontAsync({family:"Inter",style:"Medium"});
  const gap=16; const ew=(RW-(examples.length-1)*gap)/examples.length;
  const eh=810-148;
  examples.forEach((ex,i)=>{
    const card=F(ew,eh,ex.bg,slide,8);
    card.x=RX+i*(ew+gap); card.y=64;
    const strip=figma.createRectangle(); strip.resize(ew,8);
    strip.fills=[{type:"SOLID",color:h(ex.accent)}]; card.appendChild(strip);
    const hl=figma.createText(); hl.fontName={family:"Inter",style:"Bold"};
    hl.characters="Headline\nhere."; hl.fontSize=18;
    hl.lineHeight={value:22,unit:"PIXELS"};
    hl.fills=[{type:"SOLID",color:h(ex.accent)}];
    hl.x=16; hl.y=28; card.appendChild(hl);
    const sb=figma.createRectangle(); sb.resize(ew-32,36); sb.cornerRadius=6;
    sb.fills=[{type:"SOLID",color:h(ex.support)}]; sb.x=16; sb.y=100; card.appendChild(sb);
    [ex.bg,ex.accent,ex.support].forEach((c,ci)=>{
      const dot=figma.createRectangle(); dot.resize(16,5); dot.cornerRadius=2;
      dot.fills=[{type:"SOLID",color:h(c)}]; dot.x=16+ci*22; dot.y=eh-20; card.appendChild(dot);
    });
    const lbl=figma.createText(); lbl.fontName={family:"Inter",style:"Medium"};
    lbl.characters=ex.label; lbl.fontSize=10;
    lbl.fills=[{type:"SOLID",color:h("#FFFFFF"),opacity:0.4}];
    lbl.x=16; lbl.y=eh-38; card.appendChild(lbl);
  });
}
```
