# Technical Documentation Skills — Usage Examples

Examples demonstrating how to use the technical documentation skills.

## Example 1: Generate README for Simple Project

**User Request:**
```
Write a README for my JavaScript utility library
```

**Agent Workflow:**
1. Invokes `technical-doc-writer` (detects Generate mode)
2. Analyzes codebase structure
3. Selects README Simple pattern
4. Generates documentation with all 13 sections
5. Fills in content based on code analysis

**Output:** Complete README.md following simple project template

## Example 2: Improve API Documentation

**User Request:**
```
Improve this API documentation, it's missing examples
```

**Agent Workflow:**
1. Invokes `technical-doc-writer` (detects Improve mode)
2. Reads existing API documentation
3. Compares against API Documentation pattern
4. Identifies missing: examples, error codes, pagination
5. Rewrites following standards, adds missing sections

**Output:** Improved API.md with examples, error handling, pagination

## Example 3: Create Architecture Diagram

**User Request:**
```
Create a system architecture diagram for my microservices app
```

**Agent Workflow:**
1. Invokes `technical-doc-diagrams`
2. Identifies diagram type: System Architecture
3. Selects appropriate template
4. Applies color-blind friendly palette
5. Generates .drawio and .png files
6. Includes accessibility-compliant alt text

**Output:** `docs/images/architecture.drawio` and `.png`

## Example 4: Review Documentation Quality

**User Request:**
```
Review my documentation and tell me what needs fixing
```

**Agent Workflow:**
1. Invokes `technical-doc-review`
2. Runs quality checklist
3. Calculates health score
4. Produces structured feedback with severity levels
5. Identifies must-fix, should-fix, and nice-to-have items

**Output:** Review report with health score, categorized issues, recommendations

## Example 5: Generate Architecture Document

**User Request:**
```
Create architecture documentation for this system
```

**Agent Workflow:**
1. Invokes `technical-doc-writer` (detects Generate mode)
2. Invokes `technical-doc-templates` for Architecture template
3. Analyzes codebase for components, data flow, tech stack
4. Fills in all 10 architecture sections
5. Creates diagrams using `technical-doc-diagrams`

**Output:** Complete `docs/architecture.md` with diagrams

## Example 6: Full Documentation Suite

**User Request:**
```
Document this entire project
```

**Agent Workflow:**
1. Analyzes project type (simple vs complex)
2. Generates README (main or complex pattern)
3. Creates architecture documentation
4. Generates API documentation (if applicable)
5. Creates development guide
6. Creates deployment guide
7. Generates diagrams for each document
8. Reviews all documentation

**Output:** Complete documentation suite:
- README.md
- docs/architecture.md
- docs/api/*.md
- docs/development.md
- docs/deployment.md
- docs/images/*.png (diagrams)
