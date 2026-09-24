# Rule Module — Analysis

## Purpose

Extract structure, scope, and exam signal from source materials.

## Inputs

- Source files
- Syllabus (optional)
- Past papers (optional)
- Subject name
- Exam type

## Outputs

- Topic map
- Syllabus scope
- Question-type profile
- Historical patterns
- Concept dependency graph
- HOTS relevance verdict
- Redundancy map

## Step 1 — Source Manifest

| ID | File | Type | Pages | Role |
|---|---|---|---|---|

Role: primary / supporting / historical / reference.

## Step 2 — Topic Map

Extract every heading, subheading, labeled concept.

## Step 3 — Syllabus Scope

If syllabus given, mark each topic in-scope / out-of-scope / unclear.
If no syllabus, treat all source topics as in-scope, ask user to confirm.

## Step 4 — Question-Type Profile

Classify each topic by dominant question type:

- Theory
- Numerical
- Code
- Trace
- Design
- Comparison
- Debug

## Step 5 — Historical Patterns (if past papers given)

Build frequency table:

| Topic | Times seen | Types | Last seen |
|---|---|---|---|

**CRITICAL:** Historical observations only. Never frame as predictions.

## Step 6 — Concept Dependencies

Build DAG: which topics require which.

## Step 7 — HOTS Relevance

Decide Yes / No / Partial per subject.

## Step 8 — Redundancy Map

Find concepts repeated across sources.

| Concept | Appears in | Canonical location |
|---|---|---|

## User-Facing Summary

Present compact summary then move to allocation.