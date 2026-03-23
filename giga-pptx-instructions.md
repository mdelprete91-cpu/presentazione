# Giga PPTX Instructions (commit ccc3691d19, 21 slides, 1,988,270 bytes)

## Setup

```bash
pip install python-pptx --break-system-packages -q
```

Template path: `/tmp/giga_template.pptx` (downloaded by launcher bootstrap)

## Step 1: Thumbnail Analysis (run before every generation)

Before planning content, visually inspect the template to understand each slide layout.

```bash
# Convert template to PDF then to slide images
soffice --headless --convert-to pdf /tmp/giga_template.pptx --outdir /tmp/
pdftoppm -jpeg -r 150 /tmp/giga_template.pdf /tmp/tpl-slide
```

```python
# Create thumbnail grid for quick visual reference
from PIL import Image, ImageDraw
import glob

slides = sorted(glob.glob("/tmp/tpl-slide-*.jpg"))
cols = 4
rows = (len(slides) + cols - 1) // cols
img0 = Image.open(slides[0])
tw, th = img0.size[0] // 2, img0.size[1] // 2
label_h = 30
grid = Image.new("RGB", (tw * cols, (th + label_h) * rows), "white")
draw = ImageDraw.Draw(grid)
for i, path in enumerate(slides):
    col, row = i % cols, i // cols
    img = Image.open(path).resize((tw, th))
    x, y = col * tw, row * (th + label_h)
    grid.paste(img, (x, y))
    draw.text((x + 5, y + th + 5), f"Slide {i+1}", fill="black")
grid.save("/tmp/template_thumbnails.jpg", quality=85)
```

**View the thumbnail grid** to confirm which slide is which layout, then plan your content mapping. This is especially important after template updates.

## Brand Rules

Primary blue `#277AFF` | Dark `#161616`/`#000000` | Headings: Manrope | Body: Open Sans | Left-aligned | No bold/italic/bullets | 10.00 x 5.62 in

## The Golden Rule

**ONLY replace `.text` on existing runs.** Never create shapes, resize boxes, add runs, or change fonts.

## Core Principle: USE ALL 21 SLIDES

Every deck uses all 21 slides. Only Slide 14 (Chart) and Slide 15 (Image) move after Thank You if unused.

## ⚠️ TEXT LENGTH IS CRITICAL

**If text is too long, it WILL overflow and overlap other elements.** Always count characters before inserting. If your content exceeds the limit, SHORTEN IT. Never exceed the max shown in the template map below. When in doubt, be shorter.

## ⚠️ DO NOT BREAK LINES UNNECESSARILY

Many title fields have P[0] and P[1] (two lines). **P[1] is OPTIONAL.** If the title fits on one line, put it ALL in P[0] and set P[1] to "". Only use P[1] when the text genuinely needs two lines to stay within the MAX character limit.

**WRONG:** P[0]="What is" P[1]="Giga?" → forces an ugly line break
**CORRECT:** P[0]="What is Giga?" P[1]="" → clean single line

Same for subtitles and body text: never insert "\n" or split text across paragraphs unless the box is too narrow for the full text on one line. Check the MAX chars per line in the template map. If your text fits within that limit, keep it on one line.

---

## Content-Function Matching Engine

### The 8 Functions

| Function | What it does | Signal words |
|---|---|---|
| **headline** | Big number or bold statement | "X%", "#1", wow metric |
| **structure** | Organizes, transitions | section breaks |
| **explain** | Breaks down a concept | "pillars", "areas", "how it works" |
| **prove** | Demonstrates with data | numbers, before/after, KPIs |
| **sequence** | Progression through time | dates, phases, roadmap |
| **narrate** | Structured story (what/give/get) | partnerships, proposals |
| **show** | Visual impact, map, image | visual evidence |
| **relate** | People, partners, teams | contacts, logos |

### Layout Selection Matrix

```
HEADLINE →
  1 number     → Stats 1 Large (12)
  2 numbers    → Stats 2 Large (11)
  3-6 numbers  → Stats 3+3 (10)

EXPLAIN → by item count
  4 items as 2x2 → Card 2x2 (5)
  3 items        → Card 3-col (7)
  4 items        → Card 4-col (6)
  4 + icons      → Card 4-col Icons (9)

SEQUENCE →
  4 steps → Timeline 4 (15)
  5 steps → Timeline 5 (16)

NARRATE →
  Short (5 fields) → Partnership Right (2) or Left (3)
  Long (2 sections) → Partner Long (17 or 18)
```

### Key Rules

- Layouts CAN repeat if content fits. Prefer variety but never sacrifice fit.
- Avoid repeating the PREVIOUS slide layout if possible.
- Map slide (8) is ALWAYS included.

### Narrative Arc

```
0 Title → 1 Agenda → 2 PartnerRight → 3 PartnerLeft → 4 SectionDiv
→ 5 Card2x2 → 6 Card4col → 7 Card3col → 8 Map → 9 Card4colIcons
→ 10 Stats3x3 → 11 Stats2Large → 12 Stats1Large
→ 15 Timeline4 → 16 Timeline5 → 17 PartnerLeftLong → 18 PartnerRightLong
→ 19 Partners → 20 ThankYou
Hidden: 13 Chart, 14 Image
```

---

## Template Map with Text Limits

**MAX = absolute maximum characters. Do NOT exceed.**

### Slide 1: Title (white) — 4 shapes
```
[0] P[0] title line 1 | 51pt | MAX 19 chars per line
[0] P[1] title line 2 | 51pt | MAX 19 chars
[1] P[0] subtitle     | 17pt | MAX 59 chars/line, up to 5 lines
[2] FIXED   [3] IMAGE
```
Example: "Djibouti School" / "Mapping" (not "Djibouti School Mapping AI-Powered Geolocation & Government Validation")

### Slide 2: Agenda (blue) — 12 shapes
```
[8] "Agenda" | 51pt | MAX 8 chars
[0]-[5] items 1-6   | 17pt | MAX 30 chars each
[9]-[11] items 7-9   | 17pt | MAX 30 chars each (set "" if unused)
[6] FIXED   [7] IMAGE
```

### Slide 3: Partnership Image Right (white) — 13 shapes
```
[1] P[0] title line 1 | 21pt | MAX 30 chars
[1] P[1] title line 2 | 21pt | MAX 30 chars
[2][4][6][8][10] labels | 13pt | MAX 15 chars (short: "What", "Location", etc.)
[3][5][7][9] body       | 11pt | MAX 51 chars (ONE sentence only)
[11] body (details)     | 11pt | MAX 51 chars
[0] IMAGE   [12] IMAGE
```

### Slide 4: Partnership Image Left (white) — 13 shapes
Title is [0] (not [1]). Image at [12].
```
[0] P[0] title line 1 | 21pt | MAX 30 chars
[0] P[1] title line 2 | 21pt | MAX 30 chars
[2][4][6][8][10] labels | 13pt | MAX 15 chars
[3][5][7][9] body       | 11pt | MAX 51 chars
[11] body (details)     | 11pt | MAX 51 chars
[12] IMAGE   [1] FIXED
```

### Slide 5: Section Divider (blue) — 3 shapes
```
[2] section title | 51pt | MAX 19 chars/line x 2 lines = 38 total
```

### Slide 6: Card 2x2 (white) — 19 shapes
```
[0] title        | 21pt | MAX 58 chars
[5][9][13][17] labels  | 18pt | MAX 25 chars
[6][10][14][18] body   | 11pt | MAX 170 chars (4 lines, ~43 chars/line)
[1] IMAGE   [2] FIXED
```

### Slide 7: Card 4-col (white) — 21 shapes
```
[4] title              | 28pt | MAX 35 chars
[9][10][11][12] numbers | 18pt | MAX 5 chars ("01", "83", "51%")
[17][18][19][20] subtitles | 13pt | MAX 14 chars
[13][14][15][16] body      | 11pt | MAX 72 chars (4 lines, ~18 chars/line)
```

### Slide 8: Card 3-col (white) — 17 shapes
```
[4] title           | 28pt | MAX 35 chars
[6][10][14] numbers | 18pt | MAX 5 chars
[8][12][16] subtitles | 13pt | MAX 20 chars
[7][11][15] body      | 11pt | MAX 108 chars (4 lines, ~27 chars/line)
```

### Slide 9: Full Image / Map (white) — 6 shapes ⭐ ALWAYS INCLUDE
```
[4] title | 28pt | MAX 35 chars
[3][5] IMAGE — DO NOT TOUCH
```

### Slide 10: Card 4-col + Icons (white) — 22 shapes
```
[4] title              | 28pt | MAX 35 chars
[14][15][16][17] subtitles | 13pt | MAX 14 chars
[10][11][12][13] body      | 11pt | MAX 72 chars
[18-21] IMAGE icons — DO NOT TOUCH
```

### Slide 11: Stats 3+3 (white) — 21 shapes
```
[18] P[0] title line 1 | 28pt | MAX 14 chars/line
[18] P[1] title line 2 | 28pt | MAX 14 chars
Stats [1][4][7][10][13][16]   | 51pt | MAX 4 chars ("61%", "2.2M", "#1")
Labels [2][5][8][11][14][17]  | 11pt | MAX 44 chars (2 lines, ~22/line)
```

### Slide 12: Stats 2 Large (white) — 7 shapes
```
[0] P[0] title line 1 | 28pt | MAX 13 chars/line
[0] P[1] title line 2 | 28pt | MAX 13 chars
[3] stat1 | 51pt | MAX 3 chars
[4] stat2 | 51pt | MAX 3 chars
[6] desc1 | 11pt | MAX 186 chars (6 lines)
[5] desc2 | 11pt | MAX 217 chars (7 lines)
```

### Slide 13: Stats 1 Large (white) — 5 shapes — NEW
```
[0] P[0] title line 1 | 28pt | MAX 13 chars/line
[0] P[1] title line 2 | 28pt | MAX 13 chars
[3] stat | 80pt | MAX 9 chars ("428", "100%", "#1")
[4] description | 11pt | MAX 225 chars (5 lines)
[1] IMAGE   [2] FIXED
```

### Slide 14: Title + Chart (white) — OPTIONAL
```
[2] title | 28pt | MAX 65 chars (multiline)
```

### Slide 15: Title + Image (white) — OPTIONAL
```
[2] title | 28pt | MAX 65 chars (multiline)
```

### Slide 16: Timeline 4 (white) — 16 shapes
```
[0] title | 28pt | MAX 32 chars (2 lines)
Below bar: [3][5] dates | 14pt | MAX 16 chars
           [4][6] desc  | 11pt | MAX 63 chars (3 lines)
Above bar: [7][9] dates | 14pt | MAX 15 chars
           [8][10] desc | 11pt | MAX 60 chars (3 lines)
[11-15] bar — DO NOT EDIT
```

### Slide 17: Timeline 5 (white) — 19 shapes
```
[0] title | 28pt | MAX 13 chars
Below: [9][11] dates | 14pt | MAX 12 chars
       [10][12] desc | 11pt | MAX 48 chars (3 lines)
Above: [13][15][17] dates | 14pt | MAX 12 chars
       [14][16][18] desc | 11pt | MAX 48 chars (3 lines)
[3-8] bar — DO NOT EDIT
```

### Slide 18: Partnership Left Long (white) — 9 shapes
```
[1] P[0] title line 1 | 21pt | MAX 30 chars
[1] P[1] title line 2 | 21pt | MAX 30 chars
[2] label | MAX 15 chars   [3] body | 11pt | MAX 204 chars (4 lines)
[4] label | MAX 15 chars   [5] body | 11pt | MAX 204 chars (4 lines)
[7] IMAGE   [6] IMAGE logo   [8] FIXED footer
```

### Slide 19: Partnership Right Long (white) — 9 shapes
```
[2] P[0] title line 1 | 21pt | MAX 30 chars
[2] P[1] title line 2 | 21pt | MAX 30 chars
[3] label | MAX 15 chars   [4] body | 11pt | MAX 153 chars (3 lines)
[5] label | MAX 15 chars   [6] body | 11pt | MAX 153 chars (3 lines)
[0] IMAGE   [7] IMAGE   [8] FIXED
```

### Slide 20: Partners (blue) — 530 shapes
```
[529] P[0] | 51pt | MAX 8 chars
[529] P[1] | 51pt | MAX 8 chars
```

### Slide 21: Thank You (blue) — 11 shapes
```
[6] heading | 51pt | MAX 19 chars
Contact 1: [9] name | 17pt | MAX 22 chars
  [7] P[0] role  | 11pt | MAX 38 chars
  [7] P[1] city  | MAX 38 chars
  [7] P[2] email | MAX 38 chars
Contact 2: [10] name | 17pt | MAX 22 chars
  [8] P[0] role  | MAX 38 chars
  [8] P[1] city  | MAX 38 chars
  [8] P[2] email | MAX 38 chars
Social [1]-[5]: keep as-is unless asked
```

---

## Helper Functions

```python
from pptx import Presentation
import copy

def replace_text(slide, shape_idx, replacements):
    """Replace text preserving formatting. {para_idx: "text"} or {para_idx: {run_idx: "text"}}"""
    shape = slide.shapes[shape_idx]
    if not shape.has_text_frame: return
    for p_idx, value in replacements.items():
        para = shape.text_frame.paragraphs[p_idx]
        if isinstance(value, dict):
            for r_idx, text in value.items():
                if r_idx < len(para.runs): para.runs[r_idx].text = text
        else:
            if para.runs:
                para.runs[0].text = str(value)
                for r in para.runs[1:]: r.text = ""

def reorder_slides(prs, new_order):
    """Reorder slides by list of 0-based indices."""
    sldIdLst = prs.slides._sldIdLst
    sldIds = list(sldIdLst)
    for sid in sldIds: sldIdLst.remove(sid)
    for idx in new_order: sldIdLst.append(sldIds[idx])

def duplicate_slide(prs, source_index):
    """Duplicate slide with animations. Returns new 0-based index."""
    source = prs.slides[source_index]
    new_slide = prs.slides.add_slide(source.slide_layout)
    for elem in list(new_slide.shapes._spTree): new_slide.shapes._spTree.remove(elem)
    for elem in source.shapes._spTree: new_slide.shapes._spTree.append(copy.deepcopy(elem))
    for rel in source.part.rels.values():
        if "image" in rel.reltype or "chart" in rel.reltype:
            new_slide.part.rels.get_or_add(rel.reltype, rel.target_part)
    ns = '{http://schemas.openxmlformats.org/presentationml/2006/main}'
    for tag in ['timing', 'transition']:
        elem = source._element.find(ns + tag)
        if elem is not None: new_slide._element.append(copy.deepcopy(elem))
    return len(prs.slides) - 1
```

## Slide Index Reference

```
 0 Title            1 Agenda           2 PartnerRight      3 PartnerLeft
 4 SectionDivider   5 Card2x2          6 Card4col          7 Card3col
 8 FullImage/Map    9 Card4colIcons   10 Stats3x3         11 Stats2Large
12 Stats1Large     13 TitleChart**    14 TitleImage**     15 Timeline4
16 Timeline5       17 PartnerLeftLng  18 PartnerRightLng  19 Partners
20 ThankYou
** = optional, hide after ThankYou if unused
```

## Generation Flow

```python
prs = Presentation("/tmp/giga_template.pptx")
# 1. Plan content (respect MAX chars in template map!)
# 2. Edit ALL slides
# 3. Reorder chart slides after ThankYou:
reorder_slides(prs, [0,1,2,3,4,5,6,7,8,9,10,11,12,15,16,17,18,19,20,13,14])
# 4. Save
prs.save("/tmp/output.pptx")
```

## QA (Required — do NOT skip)

### Step 1: Content QA

```bash
python -m markitdown /tmp/output.pptx
python -m markitdown /tmp/output.pptx | grep -iE "Insert text|One sentence|Lorem|Example:|Hong Kong|placeholder|06 \| Impact|Atlas|Section title"
```

If grep returns results, fix them before proceeding.

### Step 2: Visual QA

Convert the generated deck to images and inspect for overflow, overlap, or layout issues.

```bash
soffice --headless --convert-to pdf /tmp/output.pptx --outdir /tmp/
pdftoppm -jpeg -r 150 /tmp/output.pdf /tmp/qa-slide
```

```python
# Create QA grid
from PIL import Image, ImageDraw
import glob

slides = sorted(glob.glob("/tmp/qa-slide-*.jpg"))
cols = 4
rows = (len(slides) + cols - 1) // cols
img0 = Image.open(slides[0])
tw, th = img0.size[0] // 2, img0.size[1] // 2
label_h = 30
grid = Image.new("RGB", (tw * cols, (th + label_h) * rows), "white")
draw = ImageDraw.Draw(grid)
for i, path in enumerate(slides):
    col, row = i % cols, i // cols
    img = Image.open(path).resize((tw, th))
    x, y = col * tw, row * (th + label_h)
    grid.paste(img, (x, y))
    draw.text((x + 5, y + th + 5), f"Slide {i+1}", fill="black")
grid.save("/tmp/qa_grid.jpg", quality=85)
```

**View the QA grid.** Check every slide for:
- Text overflow or cut off at box edges
- Overlapping elements (title over subtitle, body over next label)
- Leftover placeholder text
- Empty slides that should have content
- Text too small to read (content too long for the box)

If any issues found: fix the text (usually shorten it), re-save, re-render, re-check.

### Step 3: Deliver

Only after both QA steps pass clean, copy to outputs:

```python
import shutil
shutil.copy("/tmp/output.pptx", "/mnt/user-data/outputs/presentation.pptx")
```
