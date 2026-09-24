# OpenBook — Agent Skill

Type: Reusable agent skill
Version: 2.1.0
Entry point: SKILL.md
Modes: Skill Mode (agentic hosts) / Prompt Mode (chat hosts)

## Trigger

Invoke when user says `/openbook`, or asks for an open-book exam reference pack from source materials.

## What This Skill Does

Transforms exam materials into a compact, source-grounded, page-budgeted reference pack optimized for rapid retrieval during open-book exams.

Full behavioral spec: see `OPENBOOK-PROMPT.md`.

This file adds filesystem-aware behavior for agentic hosts.

## Filesystem Behavior

Reading:
- Read all files in user-provided source directory
- Support: PDF, PPTX, DOCX, MD, TXT, HTML
- Tag every source with short ID (e.g., S1, S2)
- Build source manifest at start

Writing:
- Output directory: `./openbook-output/`
- Files:
  - `pack.md` — full reference pack
  - `pack.meta.json` — page estimate, section list, source manifest
  - `sections/§X.Y.md` — individual sections for incremental regen

Regeneration:
- When user asks to regenerate §X.Y:
  1. Overwrite `sections/§X.Y.md`
  2. Rebuild only that section from sources
  3. Update `pack.md`, TOC, page estimate in `pack.meta.json`
  4. Do NOT rebuild other sections

## Rule Modules

Load and apply in order:
1. `rules/analysis.md`
2. `rules/allocation.md`
3. `rules/generation.md`
4. `rules/validation.md`
5. `rules/regeneration.md`

## Templates

- `templates/pack-structure.md`
- `templates/section-formats.md`
- `templates/pack-meta.md`
- `templates/render-style.md` (optional render spec)

## Example

- `examples/cse2006-java.md`

## Behavior Contract

1. Always ask for page limit if missing.
2. Always tag content with source refs.
3. Never predict exam questions.
4. Always run validation before final output.
5. Always support incremental regeneration.
6. Prefer tables/bullets/code over prose.
7. Apply `render-style.md` only if host renders to PDF/DOCX.

## Invocation

```
/openbook
```

Follow `OPENBOOK-PROMPT.md` Phases 1–6 using the rule modules above.