# Giga PPTX Instructions (commit ccc3691d19, 21 slides, 1,988,270 bytes)

## Setup

```bash
pip install python-pptx --break-system-packages -q
```

Template is already downloaded by the launcher bootstrap. Path: `/tmp/giga_template.pptx`

## Brand Rules

Primary blue `#277AFF` | Dark `#161616`/`#000000` | Headings: Manrope | Body: Open Sans | Left-aligned | No bold/italic/bullets | 10.00 x 5.62 in

## The Golden Rule

**ONLY replace `.text` on existing runs.** Never create shapes, resize boxes, add runs, or change fonts.

```python
# CORRECT
para.runs[0].text = "New text"
# WRONG
# shape.text_frame.text = "..."
```

## Core Principle: USE ALL 21 SLIDES

Every deck uses all 21 template slides. Every slide gets populated with content. The only exceptions are Slide 14 (Title+Chart) and Slide 15 (Title+Image) which move after Thank You if unused.

---

## Content-Function Matching Engine

Classify each content block by **communicative function**, then match to the best layout.

### The 8 Functions

| Function | What it does | Signal words |
|---|---|---|
| **headline** | Big number or bold statement | "X%", "#1", "first ever", wow metric |
| **structure** | Organizes, transitions | section breaks, "moving on" |
| **explain** | Breaks down a concept | "pillars", "areas", "how it works" |
| **prove** | Demonstrates with data | numbers, before/after, KPIs |
| **sequence** | Progression through time | dates, phases, roadmap |
| **narrate** | Structured story (what/give/get) | partnerships, proposals |
| **show** | Visual impact, map, image | "see the map", visual evidence |
| **relate** | People, partners, teams | "our partners", contacts |

### Layout Selection Matrix

```
HEADLINE → how many numbers?
  1 number + description     → Stats 1 Large (12) — 80pt stat
  2 numbers with context     → Stats 2 Large (11)
  3-6 numbers               → Stats 3+3 (10)
  4 numbers + explanation    → Card 4-col Icons (9)

STRUCTURE →
  Section break              → Section Divider (4)
  Overview of deck           → Agenda (1)

EXPLAIN → how many items?
  2 items (or 4 as 2x2)     → Card 2x2 (5)
  3 items                   → Card 3-col (7)
  4 items                   → Card 4-col (6) or Card 2x2 (5)
  4 items + icons relevant  → Card 4-col Icons (9)

PROVE →
  1 hero metric + context    → Stats 1 Large (12)
  2 "wow" metrics + context  → Stats 2 Large (11)
  6 standalone metrics       → Stats 3+3 (10)
  4 metrics with labels      → Card 4-col (6)

SEQUENCE →
  4 milestones              → Timeline 4 (15)
  5 milestones              → Timeline 5 (16)
  3-4 phases with detail    → Card 3-col (7) or Card 4-col (6)

NARRATE → how much text?
  Short (5 fields)          → Partnership Right (2) or Left (3)
  Long (2 sections)         → Partner Left Long (17) or Right Long (18)

SHOW →
  Map / screenshot          → Full Image (8)
  Title + image reference   → Title Image (14)

RELATE →
  Partner logos             → Partners (19)
  Contacts + CTA            → Thank You (20)
```

### Key Rule: SAME LAYOUT CAN REPEAT

Layouts CAN be used more than once when content genuinely fits. Prefer variety, but never sacrifice content fit for variety. Priority: (1) best layout for content, (2) avoid repeating the PREVIOUS slide if possible, (3) alternate dense/light naturally.

### Narrative Arc (21 slides)

```
ACT 1: OPENING
  0  Title
  1  Agenda

ACT 2: CONTEXT (narrate + show)
  2  Partnership Right — the opportunity
  3  Partnership Left — the model/partner
  4  Section Divider — transition

ACT 3: CORE (explain + prove + show)
  5  Card 2x2 — 4 concepts
  6  Card 4-col — 4 numbered items
  7  Card 3-col — 3 items
  8  Full Image / Map — visual anchor (always include)
  9  Card 4-col Icons — 4 services/tools
  10 Stats 3+3 — 6 metrics
  11 Stats 2 Large — 2 headline numbers
  12 Stats 1 Large — 1 hero metric

ACT 4: FORWARD (sequence + narrate)
  15 Timeline 4 — 4 milestones
  16 Timeline 5 — 5 milestones
  17 Partnership Left Long — deep dive
  18 Partnership Right Long — deep dive

ACT 5: CLOSING
  19 Partners — logos
  20 Thank You — contacts

HIDDEN (after Thank You if unused):
  13 Title + Chart
  14 Title + Image
```

---

## Template Map

### Slide 1: Title (white) — 4 shapes
```
[0] P[0].R[0] title line 1 | Manrope 51pt | 8.96x1.50in
[0] P[1].R[0] title line 2
[1] P[0].R[0] subtitle | Manrope 17pt | 9.16x1.87in
[2] FIXED footer   [3] IMAGE logos
```

### Slide 2: Agenda (blue) — 12 shapes
```
[8] "Agenda" | Manrope 51pt white | 3.85x3.72in
[0] item 1 | 17pt white | 4.64in wide
[1] item 2   [2] item 3   [3] item 4
[4] item 5   [5] item 6
[9] item 7   [10] item 8   [11] item 9  (set "" if unused)
[6] FIXED footer   [7] IMAGE logo
```

### Slide 3: Partnership Image Right (white) — 13 shapes
```
[1] P[0] title line 1 | 21pt | 4.69x0.75in   [1] P[1] line 2
[2] label   [3] body | 4.69x0.21in
[4] label   [5] body
[6] label   [7] body
[8] label   [9] body
[10] label  [11] body | 4.69x0.39in
[0] IMAGE right   [12] IMAGE logo
```

### Slide 4: Partnership Image Left (white) — 13 shapes — RESTRUCTURED
Title is now [0]. Image moved to [12].
```
[0] P[0] title line 1 | 21pt | 4.69x0.75in   [0] P[1] line 2
[2] label   [3] body | 4.69x0.21in
[4] label   [5] body
[6] label   [7] body
[8] label   [9] body
[10] label  [11] body | 4.69x0.43in
[12] IMAGE left   [1] FIXED footer
```

### Slide 5: Section Divider (blue) — 3 shapes
```
[2] section title | 51pt white | 8.96x2.89in
```

### Slide 6: Card 2x2 (white) — 19 shapes
```
[0] title | 21pt | 8.96x0.38in
[5] label [6] body | 3.97x0.93in    [9] label [10] body
[13] label [14] body                 [17] label [18] body
[1] IMAGE   [2] FIXED footer
```

### Slide 7: Card 4-col (white) — 21 shapes
```
[4] title | 28pt | 8.96x0.97in
[9][10][11][12] numbers | 18pt #277AFF | 1.72x0.29in
[17][18][19][20] subtitles | 13pt
[13][14][15][16] body | 11pt | 1.72x1.04in
```

### Slide 8: Card 3-col (white) — 17 shapes
```
[4] title | 28pt | 8.96x0.97in
[6][10][14] numbers | 18pt #277AFF | 2.47x0.29in
[8][12][16] subtitles | 13pt
[7][11][15] body | 11pt | 2.47x1.04in
```

### Slide 9: Full Image / Map (white) — 6 shapes — ALWAYS INCLUDE
```
[4] title | 28pt | 8.96x0.97in
[3] IMAGE map 8.96x2.98in — DO NOT TOUCH
[5] IMAGE overlay — DO NOT TOUCH
```

### Slide 10: Card 4-col + Icons (white) — 22 shapes
```
[4] title | 28pt
[14][15][16][17] subtitles | 13pt | 1.72x0.29in
[10][11][12][13] body | 11pt | 1.72x1.04in
[18-21] IMAGE icons — DO NOT TOUCH
```

### Slide 11: Stats 3+3 (white) — 21 shapes
```
[18] P[0] title line 1 | 28pt | 3.72x3.79in   [18] P[1] line 2
Left:  [1] stat [2] label  [4] stat [5] label  [7] stat [8] label
Right: [10] stat [11] label [13] stat [14] label [16] stat [17] label
Stats: 51pt #277AFF 2.01x0.75in   Labels: 11pt 2.01x0.48in
```

### Slide 12: Stats 2 Large (white) — 7 shapes
```
[0] P[0] title line 1 | 28pt | 3.46x3.15in   [0] P[1] line 2
[3] stat1 51pt   [6] desc1 11pt | 2.91x1.59in
[4] stat2 51pt   [5] desc2 11pt | 2.91x1.87in
```

### Slide 13: Stats 1 Large (white) — 5 shapes — NEW
Single hero metric with extra-large number.
```
[0] P[0] title line 1 | 28pt | 3.46x4.23in   [0] P[1] line 2
[3] stat | Manrope 80pt #277AFF | 4.14x2.20in
[4] description | Open Sans 11pt | 4.12x1.32in
[1] IMAGE logo   [2] FIXED footer
```

### Slide 14: Title + Chart (white) — 4 shapes — OPTIONAL
```
[2] title | 28pt | 3.34x3.39in
[3] CHART — not editable
```

### Slide 15: Title + Image (white) — 3 shapes — OPTIONAL
```
[2] title | 28pt | 3.44x3.17in
```

### Slide 16: Timeline 4 (white) — 16 shapes
```
[0] title | 28pt | 4.20x1.46in
Below bar: [3] date [4] desc  [5] date [6] desc
Above bar: [7] date [8] desc  [9] date [10] desc
[11-15] bar — DO NOT EDIT
```

### Slide 17: Timeline 5 (white) — 19 shapes
```
[0] title | 28pt | 3.47x0.46in
Below: [9] date [10] desc  [11] date [12] desc
Above: [13] date [14] desc [15] date [16] desc [17] date [18] desc
[3-8] bar — DO NOT EDIT
```

### Slide 18: Partnership Left Long (white) — 9 shapes
```
[2] P[0] title line 1 | 21pt | 4.69x0.75in   [2] P[1] line 2
[3] label [4] body | 4.69x1.07in    [5] label [6] body | 4.69x1.07in
[0] IMAGE   [7] IMAGE logo   [8] FIXED footer
```

### Slide 19: Partnership Right Long (white) — 9 shapes
```
[2] P[0] title line 1 | 21pt | 4.69x0.75in   [2] P[1] line 2
[3] label [4] body | 4.69x0.88in    [5] label [6] body | 4.69x0.88in
[0] IMAGE   [7] IMAGE logo   [8] FIXED footer
```

### Slide 20: Partners (blue) — 530 shapes
```
[529] P[0] "Our" | 51pt white   [529] P[1] "partners"
```

### Slide 21: Thank You (blue) — 11 shapes
```
[6] heading | 51pt white
Contact 1: [9] name 17pt | [7] P[0] role P[1] city P[2] email 11pt
Contact 2: [10] name | [8] P[0] role P[1] city P[2] email
Social: [1]-[5] (keep as-is unless asked)
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

## Slide Index Reference (21 slides)

```
 0 Title            1 Agenda           2 PartnerRight      3 PartnerLeft
 4 SectionDivider   5 Card2x2          6 Card4col          7 Card3col
 8 FullImage/Map    9 Card4colIcons   10 Stats3x3         11 Stats2Large
12 Stats1Large*    13 TitleChart**    14 TitleImage**     15 Timeline4
16 Timeline5       17 PartnerLeftLng  18 PartnerRightLng  19 Partners
20 ThankYou

* = NEW slide (80pt single hero stat)
** = optional, hide after ThankYou if unused
```

## Generation Flow

```python
prs = Presentation("/tmp/giga_template.pptx")

# 1. Plan content table (function + layout for all 21 slides)
# 2. Edit ALL slides with replace_text()

# 3. Reorder: move unused chart/image slides after ThankYou
reorder_slides(prs, [0,1,2,3,4,5,6,7,8,9,10,11,12,15,16,17,18,19,20,13,14])

# 4. Save
prs.save("/tmp/output.pptx")
```

## QA

```bash
python -m markitdown output.pptx
python -m markitdown output.pptx | grep -iE "Insert text|One sentence|Lorem|Example:|Hong Kong|placeholder|06 \| Impact|Atlas"
```
