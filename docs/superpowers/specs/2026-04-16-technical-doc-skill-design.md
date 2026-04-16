# Technical Documentation Skills — Design Spec

**Date:** 2026-04-16
**Status:** Approved
**Approach:** Core + Satellite skills (Approach A)

---

## Overview

Distill the Technical Documentation Standards project into 4 ECC-format AI skills targeting AI agents (Claude Code, etc.). The skills cover the full documentation lifecycle: generate, improve, and review technical documentation.

### Source Material

The project contains:
- 10 writing guide chapters (principles, README, architecture, dev guides, API docs, diagrams, style, review, metrics, version control)
- 7 ready-to-use templates (simple/complex README, API, architecture, dev guide, deployment, ADR)
- 2 full examples (simple TaskQueue library, complex e-commerce microservices)
- 10 diagram source templates (draw.io + PlantUML)
- Style guide and quality metrics system

### Design Decisions

| Decision | Rationale |
|----------|-----------|
| Core + satellite structure | Core stays focused on writing; specialized content (templates, diagrams, review) loads on demand |
| Inline templates | Agent has everything in-context, no file reads needed |
| Patterns over prose | Agent needs actionable rules ("section X needs Y"), not explanations |
| English language | Matches project's documentation language |
| ECC skill format | Works with Claude Code, Copilot CLI, and compatible agents |
| No examples in skills | Agent documents the target codebase; reference examples are human learning material |
| Version control & metrics reporting excluded | Operational/team concerns, not relevant to agent doc generation tasks |

---

## Skill Architecture

```
technical-doc-writer (core)
  ├── references → technical-doc-templates (when generating new docs)
  ├── references → technical-doc-diagrams (when creating diagrams)
  └── references → technical-doc-review (when reviewing docs)
```

Each satellite is independently invocable.

---

## Skill 1: `technical-doc-writer` (Core)

**File:** `skills/technical-doc-writer/SKILL.md`
**Estimated size:** ~3500 words

```yaml
name: technical-doc-writer
description: Generate, improve, and review technical documentation following
  comprehensive writing standards. Covers README, architecture, API docs,
  development guides, and deployment guides with inline style rules and
  document patterns.
```

### Internal Structure

```
1. Task Detection Rules
   ├── Generate triggers & workflow
   ├── Improve triggers & workflow
   └── Review triggers & workflow (lightweight — delegates to satellite)

2. Writing Style Rules (compact)
   ├── Voice & tense: active, present, second person
   ├── Headings: sentence case, no periods, H1→H2→H3 no skipping
   ├── Code blocks: always specify language, complete working examples
   ├── Emphasis: bold=UI/terms, code=technical, italic=new terms
   ├── Numbers: spell out 0-10, numerals for 11+ (except versions)
   ├── Lists: bullets=unordered, numbers=sequential steps
   └── Links: relative for internal, full URLs for external

3. Document Type Patterns (section order + 1-line description each)
   ├── README Simple (13 sections)
   ├── README Complex (extended sections)
   ├── API Documentation
   ├── Architecture Document
   ├── Development Guide
   ├── Deployment Guide
   └── Architecture Decision Record

4. Documentation Principles (distilled)
   ├── Section hierarchy: Essential → Common → Optional
   ├── Reader-first mindset rule
   └── Quality checklist (6 verification points)

5. Diagram Quick Rules (summary)
   ├── Tool: draw.io, format: .drawio + .png (max 800px)
   ├── Naming: kebab-case
   └── Delegates full rules to technical-doc-diagrams

6. Cross-references to Satellites
   ├── Need full templates → technical-doc-templates
   ├── Creating diagrams → technical-doc-diagrams
   └── Doing a review → technical-doc-review
```

### Task Detection

| Task Type | Trigger Examples | Workflow |
|-----------|-----------------|----------|
| **Generate** | "Write a README", "Create API docs", "Document this project" | Select pattern → analyze codebase → produce doc |
| **Improve** | "Fix this README", "Make this API doc better" | Compare against standards → identify gaps → rewrite |
| **Review** | "Review this doc", "Score this documentation" | Run checklist → produce structured feedback (delegates details to review satellite) |

---

## Skill 2: `technical-doc-templates` (Satellite)

**File:** `skills/technical-doc-templates/SKILL.md`
**Estimated size:** ~3000 words

```yaml
name: technical-doc-templates
description: Ready-to-use documentation templates for README (simple/complex),
  API documentation, architecture docs, development guides, deployment guides,
  and architecture decision records. Includes inline comments and placeholders.
```

### Templates Included

| # | Template | Sections | Source |
|---|----------|----------|--------|
| 1 | `readme-simple` | 13 | resources/templates/readme-simple.md |
| 2 | `readme-complex` | 18+ | resources/templates/readme-complex.md |
| 3 | `api-documentation` | 14 | resources/templates/api-documentation.md |
| 4 | `architecture-doc` | 10 | resources/templates/architecture-doc.md |
| 5 | `development-guide` | 9 | resources/templates/development-guide.md |
| 6 | `deployment-guide` | 10 | resources/templates/deployment-guide.md |
| 7 | `architecture-decision-record` | 9 | resources/templates/architecture-decision-record.md |

### Distillation Approach

Each template is compressed:
- Keep: section headings, `[PLACEHOLDERS]`, inline guidance comments, structural instructions
- Remove: verbose explanations, repeated prose, redundant examples
- Result: agent reads the template structure and fills in based on the target codebase

---

## Skill 3: `technical-doc-diagrams` (Satellite)

**File:** `skills/technical-doc-diagrams/SKILL.md`
**Estimated size:** ~1500 words

```yaml
name: technical-doc-diagrams
description: Create accessible, consistent technical diagrams following
  documentation standards. Covers draw.io workflows, color-blind friendly
  palettes, naming conventions, alt text requirements, and diagram type
  selection.
```

### Content

Distilled from `writing-guide/06-diagrams.md`:

- **When to use diagrams** — per doc type (architecture, deployment, sequence, ER, C4)
- **Tool rules** — draw.io primary, file formats (.drawio source + .png export)
- **Naming convention** — kebab-case matching the diagram subject
- **Sizing** — max 800px width for PNG
- **Accessibility checklist:**
  - Color-blind friendly palette (specific hex codes inlined)
  - 4.5:1 minimum contrast ratio
  - Descriptive alt text under 125 characters
  - Patterns/textures alongside color (never color alone)
  - 14px minimum font at 100% zoom
- **Diagram type decision tree** — which diagram type for which situation

---

## Skill 4: `technical-doc-review` (Satellite)

**File:** `skills/technical-doc-review/SKILL.md`
**Estimated size:** ~1500 words

```yaml
name: technical-doc-review
description: Review technical documentation against quality standards.
  Provides structured feedback with severity levels, quality scoring
  using the health formula, and actionable improvement recommendations.
```

### Content

Distilled from `writing-guide/08-review-process.md` + `writing-guide/09-metrics.md`:

- **Review checklist** — content quality, clarity, accuracy, grammar/style, accessibility
- **Review levels** — Small (1 peer), Medium (2 peers + SME), Large (multi-stakeholder)
- **Feedback format** — Must Fix / Should Fix / Nice to Have / Question
- **Quality metrics:**
  - Health score: (0.3 x Coverage) + (0.3 x Quality) + (0.2 x Usage) + (0.2 x Support)
  - Categories: 90-100 Excellent, 70-89 Good, 50-69 Fair, <50 Needs Improvement
- **Review output template** — structured markdown with score, findings, recommendations
- **Approval criteria** — all must-fix resolved, 80% should-fix resolved, at least one approval

---

## File Structure

```
technical-writing/
├── skills/
│   ├── technical-doc-writer/
│   │   └── SKILL.md              # Core skill (~3500 words)
│   ├── technical-doc-templates/
│   │   └── SKILL.md              # Templates satellite (~3000 words)
│   ├── technical-doc-diagrams/
│   │   └── SKILL.md              # Diagrams satellite (~1500 words)
│   └── technical-doc-review/
│       └── SKILL.md              # Review satellite (~1500 words)
```

---

## Scope Excluded (YAGNI)

| Excluded | Reason |
|----------|--------|
| Version control chapter (Ch.10) | Operational/team process, not agent doc generation |
| Metrics reporting templates (Ch.9) | Team reporting workflow, not doc writing |
| Full example reproduction | Agent documents target codebase; examples are human learning material |
| Diagram source files (.drawio, .puml) | Agent creates diagrams from rules, not by copying templates |
| Review meeting workflow | Human process detail, not agent-relevant |

---

## Success Criteria

- Agent with core skill can generate a complete README for a new project in one invocation
- Agent can detect whether to generate, improve, or review based on user prompt
- Each satellite is independently invocable for specialized tasks
- Generated documentation follows all style rules without manual correction
- Review output produces structured feedback with actionable severity levels
