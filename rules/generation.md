# Rule Module — Generation

## Purpose

Produce each section's content.

## Principles

1. Compress before expanding.
2. Retrieval first. Tables > bullets > prose.
3. Trace every claim.
4. Preserve course terminology.
5. No duplication. Cross-reference.
6. HOTS only where relevant.

## Format Selection

| Topic has… | Use format |
|---|---|
| Definition + key points | Theory |
| Formula(s) | Formula |
| Runnable code | Code |
| Steps | Procedure |
| Step-by-step trace | Step-trace |
| Comparable items | Comparison |
| HOTS question(s) | HOTS question |
| Worked example with ID | Exampler question |
| Closing answer | Final Answer |

Combine formats within a section as needed.

## Section Structure

    ### X.Y Topic Name  [S1, S3]
    <content>

Source tags: `[S1]`, `[S1, S3]`, `[general]`.

## Content Rules

- Theory: definition max 2 sentences, max 5 bullets, 1-line exam relevance.
- Formula: fenced block, variable table, when-to-use line.
- Code: minimal, runnable, comment gotchas only, `OUTPUT:` block, 1-line gotcha.
- Procedure: numbered steps, edge cases.
- Step-trace: numbered steps showing execution, closed with a Final Answer line.
- Comparison: 2–4 columns, max 8 rows.
- HOTS: question, type tag, concise answer.
- Exampler question: `HWQ<nn>` ID, step-trace or code + `OUTPUT:`, closed with Final Answer.
- Final Answer: one-line closing summary for trace/trace-like answers.
- Quick revision: one line per point, grouped, no explanations.

## Compression Tactics

1. Prose → bullets
2. Bullets → table
3. Drop examples (keep rule)
4. Drop redundancy (cross-reference)
5. Compress code (keep gotcha, drop boilerplate)

NEVER compress by removing required formulas, definitions, HOTS for high-frequency topics, or source tags.

## Anti-Patterns

- Generic summaries
- Exam predictions
- Padding with restated definitions
- Decorative formatting hurting retrieval
- Fake HOTS (just definitions)
- Code without purpose line
- Trace answers without a Final Answer line
- Exampler without an HWQ ID

## Output

Write each section to `sections/§X.Y.md`, then concatenate into `pack.md`.