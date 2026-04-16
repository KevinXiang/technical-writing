# Technical Documentation Skills

A set of ECC-format AI skills for generating, improving, and reviewing technical documentation.

## Skills

### Core Skill

**[`technical-doc-writer`](technical-doc-writer/SKILL.md)**
- Purpose: Main entry point for all documentation tasks
- Detects task type (generate/improve/review)
- Applies style rules and document patterns
- References satellite skills for specialized tasks

### Satellite Skills

**[`technical-doc-templates`](technical-doc-templates/SKILL.md)**
- Purpose: Ready-to-use templates for common document types
- Templates: README (simple/complex), API, Architecture, Dev Guide, Deployment, ADR
- Includes inline comments and placeholders

**[`technical-doc-diagrams`](technical-doc-diagrams/SKILL.md)**
- Purpose: Create accessible technical diagrams
- Covers: diagram types, accessibility, tooling, workflow
- Color-blind friendly palettes included

**[`technical-doc-review`](technical-doc-review/SKILL.md)**
- Purpose: Review and score documentation quality
- Includes: checklist, feedback format, health score formula
- Structured output with severity levels

## Usage

### For AI Agents

Invoke the appropriate skill based on task:
- **Generate documentation:** `technical-doc-writer` (auto-selects templates)
- **Create diagram:** `technical-doc-diagrams`
- **Review documentation:** `technical-doc-review`

### For Humans

Skills serve as reference documentation for:
- Writing guidelines and best practices
- Template structures and patterns
- Review standards and scoring
- Diagram creation and accessibility

## Development

Source material distilled from:
- `writing-guide/` — 10 chapters of documentation standards
- `resources/templates/` — 7 ready-to-use templates
- `examples/` — Complete examples demonstrating standards

## License

Same as parent project.
