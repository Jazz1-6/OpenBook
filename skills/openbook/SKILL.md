---
name: openbook
description: Adaptive, source-grounded, open-book exam reference pack optimization skill. Transforms source materials into compact, page-budgeted study guides optimized for rapid retrieval. Use when asked to create an open-book exam reference pack, study guide, or exam notes, or when /openbook is invoked.
---

# OpenBook

**Skill:** Exam reference pack optimizer
**Version:** 2.1.0
**Modes:** Skill Mode (agentic hosts) | Prompt Mode (chat hosts)

&nbsp;

## Trigger

Invoke when the user:
- Says `/openbook`
- Asks for an "open-book exam reference pack"
- Asks for "exam notes" or a "study guide" from source materials

&nbsp;

## What This Skill Does

Transforms exam materials into a **compact, source-grounded, page-budgeted** reference pack optimized for **rapid retrieval** during open-book exams.

&nbsp;

## Filesystem Behavior

**Reading:**
- Read all files in user-provided source directory
- Support: PDF, PPTX, DOCX, MD, TXT, HTML
- Tag every source with a short ID (`S1`, `S2`, ...)
- Build source manifest at start

**Writing:**
- Output directory: `./openbook-output/`
- `pack.md` — full reference pack
- `pack.meta.json` — page estimate, section list, source manifest
- `sections/§X.Y.md` — per-section files for incremental regen

&nbsp;

## Regeneration

When user asks to regenerate `§X.Y`:
1. Overwrite `sections/§X.Y.md`
2. Rebuild only that section from sources
3. Update `pack.md`, TOC, page estimate in `pack.meta.json`
4. Do NOT rebuild other sections

&nbsp;

## Rule Modules

Load and apply in this exact order:

| Order | File | Purpose |
|---|---|---|
| 1 | `rules/analysis.md` | Extract topics, syllabus, patterns |
| 2 | `rules/allocation.md` | Distribute page budget |
| 3 | `rules/generation.md` | Produce section content |
| 4 | `rules/validation.md` | Audit checks A–K |
| 5 | `rules/regeneration.md` | Incremental section rebuild |

&nbsp;

## Templates

| File | Purpose |
|---|---|
| `templates/pack-structure.md` | Output skeleton |
| `templates/section-formats.md` | Per-section format specs |
| `templates/pack-meta.md` | Metadata schema |
| `templates/render-style.md` | Optional visual style for PDF/DOCX |

&nbsp;

## Examples

- `examples/cse2006-java.md` — Java OOP reference pack
- `examples/dbms-examples.md` — DBMS reference pack

&nbsp;

## Behavior Contract

1. Always ask for page limit if missing
2. Always tag content with source refs
3. Never predict exam questions
4. Always run validation before final output
5. Always support incremental regeneration
6. Prefer tables/bullets/code over prose
7. Apply `render-style.md` only if host renders to PDF/DOCX

&nbsp;

## Invocation

```
/openbook
```

Follow `OPENBOOK-PROMPT.md` Phases 1–6 using the rule modules above.
