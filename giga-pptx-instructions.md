---
name: giga-pptx
description: "Generate branded Giga UNICEF PowerPoint presentations from natural language prompts using python-pptx. Use this skill whenever the user asks to create a presentation, deck, slides, or .pptx for Giga, UNICEF, partnerships, or any topic that should use Giga branding. Also trigger when the user mentions 'partnership deck', 'Giga slides', 'branded presentation', 'investor deck', 'country deck', or requests any PPTX output in Giga/UNICEF context. This skill downloads the official 20-slide Giga template from GitHub and programmatically replaces text using python-pptx, preserving all animations, logos, images, and brand formatting."
---

# Giga PPTX Generator

## Step 0: Always Verify Template Version

```python
import subprocess, json, os
TEMPLATE_URL = "https://github.com/mdelprete91-cpu/presentazione/raw/main/PPT%20giga.pptx"
TEMPLATE_PATH = "/tmp/giga_template.pptx"
result = subprocess.run(
    ["curl", "-sL", "https://api.github.com/repos/mdelprete91-cpu/presentazione/git/trees/main"],
    capture_output=True, text=True)
tree = json.loads(result.stdout)
remote_size = next((i["size"] for i in tree.get("tree", []) if "PPT giga" in i.get("path", "")), None)
local_size = os.path.getsize(TEMPLATE_PATH) if os.path.exists(TEMPLATE_PATH) else 0
if local_size != remote_size:
    subprocess.run(["curl", "-L", "-o", TEMPLATE_PATH, TEMPLATE_URL], capture_output=True, check=True)
```

Run EVERY TIME before generating.

## Core Principle: USE ALL 20 SLIDES

Every deck uses all 20 template slides. The only exceptions are Slide 13 (Chart) and Slide 14 (Image) which move after Thank You if unused.

## Brand Rules

Primary blue `#277AFF` | Dark `#161616` | Headings: Manrope | Body: Open Sans | Left-aligned | No bold/italic/bullets | 10.00 x 5.62 in

## The Golden Rule

**ONLY replace `.text` on existing runs.** Never create shapes, resize boxes, add runs, or change fonts.

---

## Content-Function Matching Engine

This is the intelligence behind layout selection. Inspired by how professional AI presentation tools (Gamma, Beautiful.ai) match content to layouts: classify by **communicative function**, not by content topic.

### The 8 Communicative Functions

| Function | What it does in the narrative | Signal words / patterns |
|---|---|---|
| **headline** | Hits the audience with a big number or bold statement | "X% of...", "first ever", "#1", big metric + context |
| **structure** | Organizes, transitions, signals a new section | "next", "moving on", section breaks, act transitions |
| **explain** | Breaks down a concept into parallel components | "pillars", "areas", "features", how something works |
| **prove** | Demonstrates with data, metrics, evidence | numbers, percentages, before/after, KPIs |
| **sequence** | Shows progression through time or steps | dates, phases, "then", roadmap, timeline |
| **narrate** | Tells a story with structured text (what/give/get) | partnerships, proposals, detailed descriptions |
| **show** | Visual impact, map, screenshot, image-driven | "see the map", "here is", visual evidence |
| **relate** | People, partners, teams, stakeholders | "our partners", "team", contacts, logos |

### Layout Selection Matrix

Each function maps to multiple layouts. Choose based on **item count** and **context**.

```
HEADLINE → how many key numbers?
  2 numbers with context     → Stats 2 Large (12)
  3-6 numbers               → Stats 3+3 (11)
  4 numbers + explanation    → Card 4-col Icons (10)

STRUCTURE → what kind of transition?
  Section break              → Section Divider (5)
  Overview of deck           → Agenda (2)

EXPLAIN → how many items?
  2 items (or 4 as 2x2)     → Card 2x2 (6)
  3 items                   → Card 3-col (8)
  4 items                   → Card 4-col (7) or Card 2x2 (6)
  4 items + icons relevant  → Card 4-col Icons (10)

PROVE → what kind of proof?
  6 standalone metrics       → Stats 3+3 (11)
  2 "wow" metrics + context  → Stats 2 Large (12)
  4 metrics with labels      → Card 4-col (7) or Card 4-col Icons (10)

SEQUENCE → how many steps?
  4 milestones              → Timeline 4 (15)
  5 milestones              → Timeline 5 (16)
  4 phases with detail      → Card 4-col (7)
  3 phases with detail      → Card 3-col (8)

NARRATE → how much text?
  Short (5 fields)          → Partnership Right (3) or Left (4)
  Long (2 sections)         → Partner Left Long (17) or Right Long (18)

SHOW → visual anchor
  Map / screenshot          → Full Image (9)
  Title + image reference   → Title Image (14)

RELATE → who?
  Partner logos             → Partners (19)
  3 teams/people            → Card 3-col (8)
  Contacts + CTA            → Thank You (20)
```

### Key Rule: SAME LAYOUT CAN REPEAT

Layouts CAN be used more than once when content genuinely fits. A deck about AI school mapping might use Card 4-col for both "How the AI Model Was Built" (explain, 4 steps) AND "Prediction Accuracy" (prove, 4 stats). This is fine and often better than forcing a less suitable layout for variety.

**Prefer variety, but never sacrifice content fit for variety.**

The priority order is:
1. Best layout for this specific content (function + item count)
2. Avoid repeating the same layout for the PREVIOUS slide if possible
3. Alternate between dense/light and white/blue where natural

### Narrative Arc Template

```
ACT 1: OPENING
  Slide 1  → Title (always)
  Slide 2  → Agenda (always)

ACT 2: CONTEXT (narrate + show)
  Slide 3  → narrate → Partnership Right or Left
  Slide 4  → narrate → Partnership Left or Right (mirror of 3)
  Slide 5  → structure → Section Divider

ACT 3: CORE (explain + prove + show)
  Slide 6  → explain → Card 2x2 / 3-col / 4-col (based on item count)
  Slide 7  → explain or prove → Card 4-col / 3-col / 4-col Icons
  Slide 8  → explain → Card 3-col / 2x2 / 4-col
  Slide 9  → show → Full Image / Map (always include)
  Slide 10 → explain or prove → Card 4-col Icons
  Slide 11 → prove → Stats 3+3
  Slide 12 → headline → Stats 2 Large

ACT 4: FORWARD (sequence + narrate)
  Slide 15 → sequence → Timeline 4
  Slide 16 → sequence → Timeline 5
  Slide 17 → narrate → Partnership Left Long
  Slide 18 → narrate → Partnership Right Long

ACT 5: CLOSING
  Slide 19 → relate → Partners
  Slide 20 → relate → Thank You

HIDDEN (after Thank You if unused):
  Slide 13 → show → Title + Chart
  Slide 14 → show → Title + Image
```

### Content Planning Checklist

Before writing code, plan all 18+ slides in a table:

```
| # | Function  | Layout          | Title                    | Key content summary |
|---|-----------|-----------------|--------------------------|---------------------|
| 1 | -         | Title           | Djibouti School Mapping  | + subtitle          |
| 2 | structure | Agenda          | Agenda                   | 9 items             |
| 3 | narrate   | PartnerRight    | Country Context          | What/Give/Get...    |
| 4 | narrate   | PartnerLeft     | Where We Started         | Initial data        |
| 5 | structure | SectionDivider  | Our Approach             |                     |
| 6 | explain   | Card2x2         | AI Model (4 steps)       | 4 items             |
| 7 | prove     | Card4col        | AI Accuracy (4 metrics)  | 83/115/248/51%      |
| 8 | explain   | Card3col        | Validation (3 phases)    | 3 items             |
| 9 | show      | FullImage       | Map                      | GigaMaps            |
...
```

This table IS the deliverable of the planning phase. Code follows the table.

---

## Template Map (commit 4c4d46307e)

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

### Slide 4: Partnership Image Left (white) — 13 shapes
```
[1] P[0] title line 1 | 21pt | 4.69x0.75in   [1] P[1] line 2
[3] label   [4] body | 4.69x0.21in
[5] label   [6] body
[7] label   [8] body
[9] label   [10] body
[11] label  [12] body | 4.69x0.43in
[0] IMAGE left   [2] FIXED footer
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

### Slide 9: Full Image / Map (white) — 5 shapes ⭐ ALWAYS INCLUDE
```
[4] title | 28pt | 8.96x0.97in
[3] IMAGE map 8.96x2.98in — DO NOT TOUCH
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

### Slide 13: Title + Chart (white) — OPTIONAL
```
[2] title | 28pt | 3.34x3.39in    [3] CHART — not editable
```

### Slide 14: Title + Image (white) — OPTIONAL
```
[2] title | 28pt | 3.44x3.17in
```

### Slide 15: Timeline 4 (white) — 16 shapes
```
[0] title | 28pt | 4.20x1.46in
Below bar: [3] date [4] desc  [5] date [6] desc
Above bar: [7] date [8] desc  [9] date [10] desc
[11-15] bar — DO NOT EDIT
```

### Slide 16: Timeline 5 (white) — 19 shapes
```
[0] title | 28pt | 3.47x0.46in
Below: [9] date [10] desc  [11] date [12] desc
Above: [13] date [14] desc [15] date [16] desc [17] date [18] desc
[3-8] bar — DO NOT EDIT
```

### Slide 17: Partnership Left Long (white) — 9 shapes
```
[2] P[0] title line 1 | 21pt | 4.69x0.75in   [2] P[1] line 2
[3] label [4] body | 4.69x1.07in    [5] label [6] body | 4.69x1.07in
[0] IMAGE   [7] IMAGE logo   [8] FIXED footer
```

### Slide 18: Partnership Right Long (white) — 9 shapes
```
[2] P[0] title line 1 | 21pt | 4.69x0.75in   [2] P[1] line 2
[3] label [4] body | 4.69x0.88in    [5] label [6] body | 4.69x0.88in
[0] IMAGE   [7] IMAGE logo   [8] FIXED footer
```

### Slide 19: Partners (blue) — 530 shapes
```
[529] P[0] "Our" | 51pt white   [529] P[1] "partners"
```

### Slide 20: Thank You (blue) — 11 shapes
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
    sldIdLst = prs.slides._sldIdLst
    sldIds = list(sldIdLst)
    for sid in sldIds: sldIdLst.remove(sid)
    for idx in new_order: sldIdLst.append(sldIds[idx])

def duplicate_slide(prs, source_index):
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

## Generation Flow

```python
# 0. Version check
# 1. Plan content table (function + layout + content for all 20 slides)
# 2. Load template
prs = Presentation("/tmp/giga_template.pptx")
# 3. Edit ALL slides
# 4. Reorder: move unused chart slides after ThankYou
reorder_slides(prs, [0,1,2,3,4,5,6,7,8,9,10,11,14,15,16,17,18,19,12,13])
# 5. Save
prs.save("/tmp/output.pptx")
```

## QA
```bash
python -m markitdown output.pptx
python -m markitdown output.pptx | grep -iE "Insert text|One sentence|Lorem|Example:|Hong Kong|placeholder|06 \| Impact|Atlas"
```
