# Rule Module — Regeneration

## Purpose

Rebuild individual sections without rebuilding the whole pack.

## Commands

### `regenerate §X.Y`

1. Rebuild only §X.Y from sources
2. Preserve source tags
3. Update `sections/§X.Y.md`
4. Recompute section page estimate
5. Update TOC if title changed
6. Re-validate §X.Y + neighbors

### `expand §X.Y`

1. Increase allocation for §X.Y
2. Pull from lowest-scored topic in same unit
3. Deepen content
4. Update allocation table
5. Re-validate

### `compress §X.Y`

1. Decrease allocation
2. Redistribute freed pages to highest-scored topic in same unit
3. Apply compression tactics
4. Re-validate

### `add HOTS §X.Y`

1. Generate 2–4 HOTS questions
2. Match types to topic profile
3. Append to §X.Y
4. Re-validate

### `add §X.Y` (new section)

1. Verify topic in scope
2. Score it
3. Take pages from lowest-scored topic
4. Generate section
5. Update TOC, index, page estimate
6. Re-validate

### `remove §X.Y`

1. Confirm with user
2. Remove section
3. Redistribute pages
4. Update TOC, index
5. Re-validate

## Propagation

Any change propagates to:

- `pack.md`
- TOC
- Quick revision sheet
- Index / cross-reference
- Page estimate in `pack.meta.json`
- Validation report

## Global Validation

Re-check page budget, cross-refs, duplication, TOC accuracy.
Warn user before saving if global checks fail.