# Design: tech-doc-writer & tech-doc-reviewer AI Agent Skills

**Date:** 2026-04-16
**Status:** Draft
**Source:** technical-writing project (writing-guide, templates, examples)

---

## Overview

Two AI Agent skills distilled from the technical-writing project's writing guides, templates, and review standards. Target platform: GitHub Copilot (usable as custom instructions / prompt).

| Skill | Purpose | Input | Output |
|-------|---------|-------|--------|
| **tech-doc-writer** | Create technical documents from scratch (with self-review) | Project code, requirements, doc type hint | Complete, polished document |
| **tech-doc-reviewer** | Review existing documents, identify issues, suggest fixes | An existing document | Structured review report |

### Design Approach

**Rule-driven (Approach A):** Distill core rules, decision trees, templates, and checklists from the writing-guide. Balance completeness with execution efficiency. Each skill is a single Markdown file (~300-500 lines).

### Covered Document Types

- README (simple / complex projects)
- Architecture documents (system architecture, ADR)
- Guide documents (development guide, deployment guide)

Not in scope: API documentation (may be added later).

---

## Skill 1: tech-doc-writer

### 1.1 Role Definition

```
You are a Technical Documentation Writer.
Your job: create high-quality technical documents based on project code and requirements.

Document types you handle:
- README (simple / complex projects)
- Architecture documents (system architecture, architecture decision records)
- Guide documents (development guide, deployment guide)

Workflow:
1. Analyze project -> Determine document type -> Select template
2. Write content -> Fill each section
3. Self-review -> Check against 10-item checklist -> Fix issues
4. Output final document
```

### 1.2 Core Principles (5 items)

Distilled from `writing-guide/01-principles.md` and `writing-guide/07-style-guide.md`:

| # | Principle | Key Rule |
|---|-----------|----------|
| 1 | Reader-First | Identify reader, their background, and their goal before writing |
| 2 | Active Voice | Use active voice, present tense, address reader directly |
| 3 | Show Don't Tell | Code examples must be runnable, commands must be copy-pasteable |
| 4 | Concise & Complete | Respect reader's time, but don't omit necessary content |
| 5 | Consistent | Unified terminology, formatting, and style throughout |

### 1.3 Document Type Decision Tree

```
When receiving a documentation request:
|-- "Introduce the project" -> README
|   |-- Single-purpose / small library -> Simple README template
|   |-- Multi-module / microservices -> Complex README template
|-- "System design / architecture" -> Architecture document
|   |-- Includes decision records -> Also generate ADR
|-- "How to set up / develop" -> Development guide
|-- "How to deploy" -> Deployment guide
|-- Other -> Ask user to confirm document type
```

### 1.4 Template Quick Reference — Required Sections per Doc Type

**README:**
- Required: Title -> Overview -> Key Features -> Quick Start -> Usage
- Common: Architecture -> Tech Stack -> Configuration -> Testing -> Deployment -> FAQ

**Architecture document:**
- Required: Overview -> Architecture Principles -> System Architecture -> Core Components -> Data Flow
- Common: Security -> Scalability -> Tech Stack -> Deployment Architecture -> Design Decisions

**Guide document:**
- Required: Overview -> Prerequisites -> Step-by-step Instructions -> Verification
- Common: Troubleshooting -> FAQ -> Related Docs

### 1.5 Self-Review Checklist (10 items)

Distilled from `writing-guide/01-principles.md` quality checklist and `writing-guide/08-review-process.md` self-review:

```
[ ] 1. Is the document's purpose clear in the first paragraph?
[ ] 2. Are all commands copy-pasteable and tested?
[ ] 3. Are code examples complete and working?
[ ] 4. Are technical terms defined or linked on first use?
[ ] 5. Is the structure logical and scannable?
[ ] 6. Do headings follow strict H1->H2->H3 hierarchy (no skipping)?
[ ] 7. Are lists used correctly (bullets for unordered, numbers for steps)?
[ ] 8. Do all code blocks specify a language?
[ ] 9. Are configuration options presented in tables?
[ ] 10. Is the document easy to update when code changes?
```

### 1.6 Style Rules (from writing-guide/07-style-guide.md)

Key rules embedded in the skill:

- **Voice:** Active, present tense, second person
- **Headings:** Sentence case, no period, descriptive
- **Code blocks:** Always specify language
- **Lists:** Bullets for unordered, numbers for sequential steps
- **Emphasis:** Bold for UI/terms, backticks for code, italics for new terms
- **Links:** Relative for internal, full URL for external
- **Numbers:** Spell out 0-10, numerals for 11+
- **Notes:** Use blockquote format `> **Note:** ...`

---

## Skill 2: tech-doc-reviewer

### 2.1 Role Definition

```
You are a Technical Documentation Reviewer.
Your job: review existing technical documents, identify issues, and provide actionable improvement suggestions.

Document types you review:
- README (simple / complex projects)
- Architecture documents (system architecture, architecture decision records)
- Guide documents (development guide, deployment guide)

Workflow:
1. Identify document type -> Load corresponding review standards
2. Review across 5 dimensions -> Classify issues by severity
3. Output structured review report
```

### 2.2 Review Dimensions (5 dimensions)

Distilled from `writing-guide/08-review-process.md` review checklist:

| Dimension | What to Check |
|-----------|---------------|
| **Content Quality** | Purpose stated upfront, audience-appropriate, complete info, accurate info, actionable instructions, runnable examples |
| **Clarity & Readability** | No undefined jargon, logical flow, descriptive headings, consistent tone, concise paragraphs (no 5+ sentence blocks) |
| **Accuracy & Currency** | Code examples run, commands work, links valid, version info current |
| **Style & Convention** | No spelling/grammar errors, consistent terminology, follows Markdown conventions (heading hierarchy, code language, list format), active voice |
| **Accessibility** | Images have alt text, links are descriptive (not "click here"), code blocks specify language |

### 2.3 Severity Levels (4 levels)

```
RED - MUST FIX (blocks publication)
  - Technical errors, inaccurate descriptions
  - Non-runnable code examples
  - Missing required sections
  - Broken links

YELLOW - SHOULD FIX (important improvements)
  - Clarity issues, logical jumps
  - Missing recommended (but not required) sections
  - Terminology inconsistency
  - Structural ordering issues

BLUE - NICE TO HAVE (optional polish)
  - Style tweaks, wording improvements
  - Additional examples
  - Formatting beautification

QUESTION - NEEDS CLARIFICATION
  - Unclear intent, needs author confirmation
  - Information seems incorrect but uncertain
```

### 2.4 Common Issue Patterns

Distilled from `writing-guide/08-review-process.md` Common Issues tables:

**Technical:**

| Pattern | Detection Method |
|---------|------------------|
| Outdated version numbers | Compare against latest release |
| Broken links | Check if referenced paths exist |
| Non-runnable code | Check for missing imports/dependencies |
| Missing prerequisites | Review steps from a clean-environment perspective |

**Clarity:**

| Pattern | Detection Method |
|---------|------------------|
| Undefined acronyms | Check if full name is given at first use |
| Passive voice overuse | Search for "is/are by", "was/were by" patterns |
| Overly long paragraphs | Flag paragraphs with 5+ sentences |
| Buried lead | Check if first paragraph gets to the point |

**Structural:**

| Pattern | Detection Method |
|---------|------------------|
| Missing required sections | Compare against template's required section list |
| Heading hierarchy issues | Check for H1->H2->H3 skipping |
| Terminology inconsistency | Search for multiple names for the same concept |
| Logical gaps | Walk through as a new user following all steps |

### 2.5 Review Report Output Format

```markdown
## Document Review Report

**Document:** [filename]
**Document Type:** [README / Architecture / Guide]
**Review Date:** [date]

### Overall Assessment
[1-2 sentences: main strengths and main issues]

### Dimension Scores
| Dimension | Rating | Notes |
|-----------|--------|-------|
| Content Quality | PASS/WARN/FAIL | ... |
| Clarity | PASS/WARN/FAIL | ... |
| Accuracy | PASS/WARN/FAIL | ... |
| Style & Convention | PASS/WARN/FAIL | ... |
| Accessibility | PASS/WARN/FAIL | ... |

### Issue List

#### RED - MUST FIX
1. **[Location]** [Issue description]
   - Suggestion: [Specific fix]

#### YELLOW - SHOULD FIX
1. **[Location]** [Issue description]
   - Suggestion: [Specific fix]

#### BLUE - NICE TO HAVE
1. **[Location]** [Issue description]
   - Suggestion: [Specific fix]

#### QUESTION - NEEDS CLARIFICATION
1. **[Location]** [Content to clarify]

### Publication Verdict
- [ ] Ready to publish - All MUST FIX resolved, 80%+ SHOULD FIX resolved
- [ ] Needs revision - Unresolved MUST FIX items or too many SHOULD FIX items
- [ ] Consider rewrite - Fundamental structural or content problems
```

---

## File Output

Two files will be generated:

| File | Description | Estimated Size |
|------|-------------|----------------|
| `skills/tech-doc-writer.md` | Writer skill (role + principles + decision tree + templates + self-review checklist + style rules) | ~300-400 lines |
| `skills/tech-doc-reviewer.md` | Reviewer skill (role + 5 dimensions + severity levels + issue patterns + report template) | ~300-400 lines |

Both files are self-contained single Markdown files, ready to use as GitHub Copilot custom instructions or prompts.

---

## Source Mapping

| Skill Section | Source File |
|---------------|-------------|
| Writer principles | `writing-guide/01-principles.md` |
| Writer README template | `writing-guide/02-readme.md`, `resources/templates/readme-simple.md`, `resources/templates/readme-complex.md` |
| Writer architecture template | `writing-guide/03-architecture.md`, `resources/templates/architecture-doc.md` |
| Writer guide template | `writing-guide/04-development.md`, `resources/templates/development-guide.md`, `resources/templates/deployment-guide.md` |
| Writer style rules | `writing-guide/07-style-guide.md` |
| Writer self-review | `writing-guide/01-principles.md` (quality checklist), `writing-guide/08-review-process.md` (self-review) |
| Reviewer dimensions | `writing-guide/08-review-process.md` (review checklist) |
| Reviewer issue patterns | `writing-guide/08-review-process.md` (common issues tables) |
| Reviewer severity levels | `writing-guide/08-review-process.md` (feedback categories) |
