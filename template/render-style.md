# Template — Render Style

Optional render spec for producing PDFs / DOCX that match the CSE2006 Java study guide style. Hosts that render Markdown may apply this. Hosts that do not render may ignore it.

---

## Purpose

OpenBook outputs Markdown. This file describes the **visual style** the Markdown should be rendered in so the final PDF looks like a compact, exam-ready study guide — not a generic Markdown dump.

The goal: **dense but readable, retrieval-first, exam-comfortable**.

---

## Page & Typography

| Element | Spec |
|---|---|
| Page size | A4 |
| Margins | 2 cm all sides |
| Body font | Serif (e.g., Georgia, Cambria) — 11 pt |
| Heading font | Sans-serif (e.g., Calibri, Inter) — bold |
| Line spacing | 1.15 |
| Paragraph spacing | 0.6 em |

---

## Headings

| Level | Style | Notes |
|---|---|---|
| H1 (Pack title) | 22 pt bold, centered, thin rule below | One per pack |
| H2 (Unit) | 16 pt bold, left, colored accent bar (e.g., #2C3E50) | One per unit |
| H3 (§X.Y) | 13 pt bold, left, source tag right-aligned in muted gray | One per section |
| H4 (sub-block) | 11 pt bold italic | For "Why", "Gotcha", "Exam relevance" |

Numbering: automatic for §X.Y headings; manual for sub-blocks.

---

## Source Tags

Format: `[S1]`, `[S1, S3]`, `[general]`

Style:

- Rendered inline right of H3 heading
- Font: 9 pt, muted gray (#7F8C8D)
- Never bold
- Never in a box

---

## Code Blocks

- Background: light gray (#F5F5F5)
- Border-left: 3 px solid #4A90E2
- Font: monospace (Consolas, Fira Code) — 10 pt
- Padding: 8 px
- Line numbers: off
- Language label: top-right corner in tiny caps (e.g., `JAVA`)
- Keyword highlighting: subtle, no neon colors
- Comments: muted green (#6A9955)

---

## Output / Trace Blocks

- Background: pale yellow (#FFFBE6)
- Border-left: 3 px solid #F0C040
- Font: monospace
- Prefixed with `OUTPUT:` or `TRACE:` label in small caps / uppercase
- Label rendered in bold small caps for visual separation from code blocks

---

## Final Answer Lines

- Bold label: `Final answer:`
- Rendered in small caps or uppercase (`FINAL ANSWER:`) for visual parity with the reference study guide
- Value: normal weight
- Small horizontal rule above when closing a trace block

---

## Tables

- Header row: light blue background (#EAF2FB), bold
- Borders: 1 px solid #CCCCCC
- Cell padding: 6 px
- Font: 10 pt
- No zebra striping (keeps it cleaner in print)
- Alignment: left default; right-align numeric columns

---

## Callouts

| Type | Background | Border-left | Icon |
|---|---|---|---|
| Exam relevance | #E8F5E9 | #4CAF50 | none |
| Gotcha | #FFF3E0 | #FF9800 | none |
| Note | #E3F2FD | #2196F3 | none |

Callouts use blockquote markdown (`> ...`) and are rendered with the above styling.

---

## Comparison Tables

- 2–4 columns max
- Header bold, centered
- First column left-aligned (label), others centered
- Row height: compact

---

## HOTS Questions

- Question label `**Q [Type]**` — bold, dark blue (#1A5276)
- Answer label `**A:**` — bold, dark green (#1E8449)
- Small vertical space between Q and A
- No background tint (keep print-friendly)

---

## Exampler Questions

- ID label `HWQ<nn>` — bold, dark purple (#6A1B9A)
- Step-trace blocks rendered as monospace
- `OUTPUT:` blocks rendered with pale-yellow background
- `Final answer:` line rendered in small caps
- No background tint

---

## Quick Revision Sheet

- Bulleted list
- One line per point
- Grouped by unit
- Muted background (#F8F9FA) with left border
- Font: 10.5 pt

---

## Index / Cross-References

- Two-column table
- First column: concept
- Second column: §X.Y
- Right-align section refs
- Compact row height

---

## Page Breaks

- Before each Unit (H2)
- Before Quick Revision Sheet
- Before Index
- Never mid-section unless a section exceeds 1.5 pages

---

## Colors (Summary)

| Purpose | Hex |
|---|---|
| Heading accent | #2C3E50 |
| Code border | #4A90E2 |
| Output block | #F0C040 |
| Table header | #EAF2FB |
| Exam relevance | #4CAF50 |
| Gotcha | #FF9800 |
| Note | #2196F3 |
| Muted text | #7F8C8D |
| Exampler ID | #6A1B9A |

---

## PDF Generation Suggestions

Hosts may render via:

- Pandoc + a CSS file
- Pandoc + a LaTeX template
- Markdown → HTML → print-to-PDF
- Paste into Word/Docs with a style sheet

This file describes the *target style*, not the tool.

---

## Optional

Rendering is host-dependent. This spec is a **suggestion**, not a requirement. Packs remain valid without it.