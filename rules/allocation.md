# Rule Module — Allocation

## Purpose

Distribute page budget across topics.

## Inputs

- Topic map
- Question-type profile
- Historical frequency
- Concept dependency graph
- HOTS verdict
- Page budget

## Step 1 — Reserve Overhead

- 5% title + TOC
- 8% quick revision sheet
- 2% index

Total overhead: 15%. Content: 85% of page budget.

## Step 2 — Score Each Topic

```
Score = w1*frequency + w2*syllabus_weight + w3*centrality + w4*question_density + w5*HOTS_weight
```

Defaults: `w1=0.30, w2=0.20, w3=0.25, w4=0.15, w5=0.10`

Normalize factors to 0–1 before weighting.
If no past papers: `w1=0`, renormalize.

## Step 3 — Score to Pages

```
pages_i = (score_i / sum_scores) * content_pages
```

Round to nearest 0.25.

## Step 4 — Floors and Ceilings

- Floor: every in-scope topic gets at least 0.25 page
- Ceiling: no topic exceeds 25% of content pages

## Step 5 — Dependency Adjustment

Prerequisite topics get at least as many pages as dependents.

## Step 6 — Allocation Table

| Section | Score | Pages | % of content |
|---|---|---|---|

Present to user. Offer to adjust.

## Rules

1. Never allocate equally unless scores equal.
2. Always reserve overhead.
3. Always enforce floor and ceiling.
4. Never exceed page budget.