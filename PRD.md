# PRD.md — OpenBook Skill

> Adaptive, Source-Grounded Open-Book Examination Reference Pack Optimization Skill
>
> Version: 2.1.0
> Status: Proposed
> Product Type: Agent Skill / Agent Capability

---

# 1. Product Overview

## 1.1 Product Name

**OpenBook**

## 1.2 Product Type

**Reusable Agent Skill / Agent Capability**

OpenBook is NOT a standalone AI agent.

It is a specialized skill that can be invoked by compatible host agents such as:

- Claude Code
- DeepSeek-powered coding agents
- ChatGPT (Custom GPT or chat)
- Other agentic coding environments supporting skills, instructions, filesystem access, and tool execution

The host agent remains responsible for:

- Conversation
- Reasoning
- Tool execution
- File access
- Document generation
- Environment management

OpenBook provides the specialized workflow for transforming examination material into an adaptive, compact, source-grounded examination reference pack.

---

# 2. Core Product Principle

OpenBook is not primarily a note generator.

It is an **examination reference optimization system**.

The core optimization objective is:

> Maximize the amount of useful, retrievable, source-grounded examination information within the user's available page and format constraints.

The system should optimize for:

1. Information relevance
2. Source fidelity
3. Retrieval speed
4. Page efficiency
5. Exam-question coverage
6. Conceptual completeness
7. HOTS applicability
8. Readability
9. Traceability
10. Accuracy

The system must NOT optimize merely for the amount of text generated.

---

# 3. Problem Statement

Students preparing for open-book examinations may have access to large quantities of material:

- Lecture PPTs
- PDFs
- Textbooks
- Syllabus documents
- Previous examination papers
- Assignments
- Class notes
- Question banks
- Faculty-provided study material
- Reference documents

The problem is not simply lack of information.

The problem is:

> **Finding the right information quickly during the examination while operating under page, time, and cognitive constraints.**

Traditional AI note generators often produce generic summaries.

This creates several problems:

1. Important and unimportant topics receive similar space.
2. Historical question patterns are not systematically analyzed.
3. Repeated concepts are duplicated.
4. HOTS questions may receive insufficient treatment.
5. Page limits are handled poorly.
6. Important formulas and procedures may be buried inside paragraphs.
7. The document may be difficult to navigate under exam pressure.
8. Generated claims may not be traceable to source material.
9. Content may be technically correct but poorly aligned with course terminology.
10. Regenerating one section may require regenerating the entire document.
11. The AI may produce a visually polished document that is not actually useful during an examination.
12. Previous papers can be misinterpreted as predictions rather than historical evidence.

OpenBook addresses these problems through an adaptive analysis → compression → allocation → generation → validation workflow.

---

# 4. Product Vision

OpenBook should transform:

> "Here are all my examination materials."

into:

> "Here is a compact, source-grounded, exam-optimized reference pack that I can navigate quickly."

The system should automatically determine:

- What content matters
- How much space each topic deserves
- Which question types are historically represented
- Which concepts are prerequisites for others
- Whether HOTS material is relevant
- Which information needs examples
- Which topics need formulas
- Which topics need algorithms
- Which topics need procedures
- Which topics need diagrams
- Which topics need comparison tables
- Which concepts can be merged
- Which information is redundant
- Which sections deserve additional space
- Which sections can be compressed
- How the final pack should be organized
- How quickly information can be located

---

# 5. Goals

## 5.1 Primary Goals

### G1 — Adaptive Examination Analysis

Analyze supplied examination materials and identify:

- Syllabus scope
- Topics
- Subtopics
- Question types
- Historical question patterns
- Recurring concepts
- Concept dependencies
- Numerical/problem-solving requirements
- HOTS characteristics

---

### G2 — Source-Grounded Content

Generate content primarily from user-provided sources.

The system should preserve course-specific terminology where possible.

Every significant generated section should be traceable to its source material.

---

### G3 — Page-Budget Optimization

Respect explicit user-defined page limits.

If no page limit is supplied, the skill should ask for one when page optimization is central to the request or use an explicit default only when appropriate.

The system must intelligently allocate space instead of compressing all topics equally.

---

### G4 — Subject Adaptability

OpenBook must work across different subjects without assuming a fixed structure.

Supported examples include:

- Digital Logic
- Computer Architecture
- DBMS
- Operating Systems
- Data Structures
- Algorithms
- Computer Networks
- Mathematics
- Physics
- Programming
- Machine Learning
- Other theory-heavy or problem-solving subjects

The output structure must be determined by the source material and examination characteristics.

---

### G5 — HOTS Adaptability

Automatically determine whether HOTS-style material is relevant.

Possible HOTS patterns include:

- Application
- Analysis
- Design
- Scenario-based reasoning
- Case-based questions
- Multi-step problems
- Debugging
- Derivation
- Comparison
- Optimization
- Interpretation
- Decision-making

HOTS must not be forced into subjects where it provides little value.

---

### G6 — Rapid Retrieval

Optimize the final reference pack for fast information retrieval.

The user should be able to locate:

- Formulas
- Definitions
- Procedures
- Algorithms
- Diagrams
- Comparisons
- Examples
- Problem-solving patterns
- Historical question mappings

with minimal navigation effort.

---

### G7 — Automated Quality Control

The skill must generate, audit, repair, and re-audit the reference pack.

Quality control should evaluate:

- Accuracy
- Completeness
- Source grounding
- Page count
- Duplication
- Navigation
- Formatting
- Exam utility
- HOTS coverage where relevant

---

### G8 — Incremental Regeneration

Allow individual sections to be regenerated without unnecessarily rebuilding the entire pack.

Changes must propagate to:

- Page allocation
- Navigation
- Indexes
- Cross-references
- Global validation

---

### G9 — Host-Agent Compatibility

OpenBook must remain a reusable skill.

It should not require:

- A dedicated backend
- A dedicated web application
- A permanent server
- A custom AI agent
- A proprietary UI

It should operate through capabilities provided by the host agent.

---

### G10 — Multi-Host Deployment Modes

OpenBook must support two deployment modes:

**Skill Mode** — for agentic hosts with filesystem, persistent instructions, and tool execution (e.g., Claude Code, Cursor, DeepSeek agent, Gemini CLI).

**Prompt Mode** — for chat-only hosts with file upload but no filesystem or persistent skill support (e.g., ChatGPT, DeepSeek chat, Claude chat, Gemini chat).

Both modes must:

- Follow identical analytical, allocation, generation, and validation rules
- Produce equivalent reference packs
- Preserve source grounding and traceability
- Degrade gracefully when host capabilities are limited

The skill must not assume any single host's proprietary features.

---

# 6. Non-Goals

OpenBook is NOT intended to:

- Predict exact examination questions.
- Guarantee that a topic will appear.
- Claim that a historical question will repeat.
- Replace the student's learning process.
- Act as a general-purpose chatbot.
- Become a standalone AI agent.
- Make institutional examination-policy decisions.
- Circumvent examination rules.
- Misrepresent external information as faculty-provided material.
- Optimize based solely on historical question frequency.
- Produce arbitrary numerical probabilities for future questions.

Past examination papers are evidence of **historical examination patterns**, not guarantees of future questions.

---

# 7. Target Users

## Primary User

Students preparing for open-book examinations.

Typical characteristics:

- Multiple source documents
- Limited preparation time
- Limited reference-pack page count
- Previous examination papers
- Question banks
- Need for rapid lookup
- Need for concise but sufficient explanations

---

# 8. User Invocation

The skill should support a simple invocation:

```text
/openbook
```

In Prompt Mode (chat hosts without skill support), the user pastes the OpenBook prompt at the start of a session and then uploads source materials.

---

# 9. Deployment Modes

## 9.1 Skill Mode

Target hosts:
- Claude Code
- Cursor / Windsurf
- DeepSeek agent
- Gemini CLI / Antigravity
- Any agentic framework with filesystem + tool access

Delivery:
- `SKILL.md` + `rules/` + `templates/` folder structure
- Host loads the skill on invocation

## 9.2 Prompt Mode

Target hosts:
- ChatGPT (chat + Custom GPT)
- DeepSeek chat
- Claude chat
- Gemini chat
- Any chat AI with file upload but no skill support

Delivery:
- Single `OPENBOOK-PROMPT.md` file
- Pasted as first message, system prompt, or Custom GPT instruction
- Sources uploaded as attachments

Both modes share the same rule set and must produce equivalent output quality.