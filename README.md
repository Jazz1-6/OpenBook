# OpenBook

Adaptive, source-grounded, open-book examination reference pack optimization skill.

## Two Modes

| Mode | Host | File |
|---|---|---|
| Prompt Mode | ChatGPT, DeepSeek chat, Claude chat, Gemini chat | OPENBOOK-PROMPT.md |
| Skill Mode | Claude Code, Cursor, DeepSeek agent, Gemini CLI | SKILL.md + rules/ |

## Quick Start — Prompt Mode

1. Open ChatGPT or DeepSeek chat
2. Paste OPENBOOK-PROMPT.md as first message
3. Upload source materials
4. Say /openbook
5. Answer page-limit question
6. Receive pack

## Quick Start — Skill Mode

1. Place openbook/ in host's skill directory
2. Invoke /openbook
3. Follow prompts

## Structure

```
openbook/
├── README.md
├── PRD.md
├── OPENBOOK-PROMPT.md
├── SKILL.md
├── rules/
│   ├── analysis.md
│   ├── allocation.md
│   ├── generation.md
│   ├── validation.md
│   └── regeneration.md
├── templates/
│   ├── pack-structure.md
│   ├── section-formats.md
│   ├── pack-meta.md
│   └── render-style.md
└── examples/
    ├── cse2006-java.md
    └── dbms-sample.md
```

## Optional Render Style

OpenBook outputs Markdown. If you want the final PDF/DOCX to match the CSE2006 Java study guide look, apply `templates/render-style.md` during conversion. Rendering is host-dependent and fully optional.