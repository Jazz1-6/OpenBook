# Template - pack.meta.json

Structured metadata companion to `pack.md`. Enables incremental regeneration, validation, and downstream tooling.

---

## Schema

```json
{
  "version": "1.0",
  "generated_at": "2026-01-01T12:00:00Z",
  "mode": "skill",
  "subject": "CSE2006 - Java Programming",
  "exam": "CAT-2 (Open Book)",
  "page_budget": 10,
  "estimated_pages": 9.8,
  "source_manifest": [
    {
      "id": "S1",
      "file": "unit3.pdf",
      "type": "slides",
      "pages": 45,
      "role": "primary"
    },
    {
      "id": "S2",
      "file": "unit4.pdf",
      "type": "slides",
      "pages": 52,
      "role": "primary"
    },
    {
      "id": "S3",
      "file": "cat1-2024.pdf",
      "type": "past_paper",
      "pages": 4,
      "role": "historical"
    }
  ],
  "overhead": {
    "title_and_toc": 0.5,
    "quick_revision": 0.8,
    "index": 0.2,
    "total": 1.5
  },
  "content_pages": 8.5,
  "sections": [
    {
      "id": "3.1",
      "title": "Exceptions: Concept, Hierarchy, Types",
      "pages": 1.25,
      "score": 0.82,
      "sources": ["S1"],
      "formats": ["theory", "comparison", "code"],
      "file": "sections/3.1.md",
      "status": "generated"
    },
    {
      "id": "3.2",
      "title": "try / catch / Multiple catch",
      "pages": 0.75,
      "score": 0.61,
      "sources": ["S1"],
      "formats": ["theory", "code", "gotcha"],
      "file": "sections/3.2.md",
      "status": "generated"
    },
    {
      "id": "3.10",
      "title": "HOTS - Unit 3",
      "pages": 1.0,
      "score": 0.70,
      "sources": ["S1"],
      "formats": ["hots", "step-trace", "final-answer", "exampler"],
      "file": "sections/3.10.md",
      "status": "generated"
    }
  ],
  "validation": {
    "status": "PASS",
    "checked_at": "2026-01-01T12:05:00Z",
    "checks": {
      "A_page_budget": "PASS",
      "B_coverage": "PASS",
      "C_source_grounding": "PASS",
      "D_duplication": "PASS",
      "E_navigation": "PASS",
      "F_formatting": "PASS",
      "G_exam_utility": "PASS",
      "H_hots": "PASS",
      "I_step_trace_final_answer": "PASS",
      "J_safety": "PASS",
      "K_style_consistency": "PASS"
    },
    "issues": []
  },
  "render": {
    "style_applied": false,
    "style_file": "templates/render-style.md"
  },
  "regeneration_log": [
    {
      "section": "3.8",
      "action": "expand",
      "from_pages": 0.75,
      "to_pages": 1.0,
      "at": "2026-01-01T12:10:00Z"
    }
  ]
}
```

---

## Field Reference

| Field | Type | Required | Description |
|---|---|---|---|
| `version` | string | Yes | Schema version |
| `generated_at` | ISO 8601 | Yes | Pack creation timestamp |
| `mode` | `"skill"` \| `"prompt"` | Yes | Deployment mode used |
| `subject` | string | Yes | Subject name |
| `exam` | string | Yes | Exam type |
| `page_budget` | number | Yes | User-provided budget |
| `estimated_pages` | number | Yes | Estimated actual page count |
| `source_manifest` | array | Yes | One entry per source |
| `overhead` | object | Yes | Fixed overhead allocation |
| `content_pages` | number | Yes | `page_budget - overhead.total` |
| `sections` | array | Yes | One entry per section |
| `validation` | object | Yes | Last validation report |
| `render` | object | No | Render style status |
| `regeneration_log` | array | No | History of incremental rebuilds |

---

## Source Entry

| Field | Type | Description |
|---|---|---|
| `id` | string | Short ID (`S1`, `S2`, `S3`) |
| `file` | string | Original filename |
| `type` | string | `slides` \| `pdf` \| `docx` \| `md` \| `txt` \| `html` \| `past_paper` \| `syllabus` \| `notes` |
| `pages` | number | Source page count |
| `role` | string | `primary` \| `supporting` \| `historical` \| `reference` |

---

## Section Entry

| Field | Type | Description |
|---|---|---|
| `id` | string | Section number (`3.1`, `4.5`) |
| `title` | string | Section title |
| `pages` | number | Allocated pages |
| `score` | number | Allocation score (0-1) |
| `sources` | array | Source IDs referenced |
| `formats` | array | Formats used: `theory`, `formula`, `code`, `procedure`, `step-trace`, `comparison`, `hots`, `exampler`, `final-answer`, `quick-revision`, `gotcha` |
| `file` | string | Path to section file |
| `status` | string | `generated` \| `stale` \| `failed` |

---

## Overhead Entry

| Field | Type | Description |
|---|---|---|
| `title_and_toc` | number | Pages reserved for title + TOC |
| `quick_revision` | number | Pages reserved for quick revision sheet |
| `index` | number | Pages reserved for index |
| `total` | number | Sum of overhead |

---

## Validation Entry

| Field | Type | Description |
|---|---|---|
| `status` | `"PASS"` \| `"FAIL"` | Overall result |
| `checked_at` | ISO 8601 | Last validation timestamp |
| `checks` | object | Per-check status (A-K) |
| `issues` | array | List of issue descriptions |

---

## Render Entry

| Field | Type | Description |
|---|---|---|
| `style_applied` | boolean | Whether `render-style.md` was applied |
| `style_file` | string | Path to render style spec |

---

## Regeneration Log Entry

| Field | Type | Description |
|---|---|---|
| `section` | string | Section ID |
| `action` | string | `regenerate` \| `expand` \| `compress` \| `add-hots` \| `add` \| `remove` |
| `from_pages` | number | Pages before |
| `to_pages` | number | Pages after |
| `at` | ISO 8601 | Timestamp |

---

## Usage

- **Skill Mode:** Written automatically to `./openbook-output/pack.meta.json`
- **Prompt Mode:** Optional - user may request it as a JSON block at end of pack
- **Regeneration:** Read `pack.meta.json` before rebuilding a section; write updated version after
- **Validation:** Read to confirm all checks passed before delivery
- **Downstream tools:** Can parse for page counts, section lists, source attribution