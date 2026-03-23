# Rewrite 06-diagrams.md Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Rewrite `writing-guide/06-diagrams.md` to focus exclusively on draw.io with practical guidance for common software diagrams.

**Architecture:** Replace current content (draw.io + PlantUML) with concise, draw.io-only guide. Merge similar diagram types, add step-by-step instructions for each.

**Tech Stack:** Markdown documentation, draw.io as diagramming tool

---

### Task 1: Rewrite Header Sections (Purpose + When to Use)

**Files:**
- Modify: `writing-guide/06-diagrams.md:1-30`

**Step 1: Update title and add subtitle**

Replace first 3 lines with:
```markdown
# Creating Diagrams with draw.io

When and how to use diagrams effectively in software development.

## Purpose
```

**Step 2: Keep Purpose section unchanged**

The Purpose section (lines 5-12) remains the same.

**Step 3: Update When to Use section title**

Change line 14 from `## When to Use Diagrams` to:
```markdown
## When to Use Diagrams
```

**Step 4: Simplify Use/Don't Use bullets**

Remove emoji from lines 16, 24. Change from:
```markdown
### ✅ Use Diagrams For:
```
To:
```markdown
### Use Diagrams For
```

And from:
```markdown
### ❌ Don't Use For:
```
To:
```markdown
### Don't Use For
```

**Step 5: Verify changes**

Run: `head -30 writing-guide/06-diagrams.md`
Expected: Clean header sections without emojis

---

### Task 2: Replace draw.io Section with Quick Start

**Files:**
- Modify: `writing-guide/06-diagrams.md:31-79`

**Step 1: Delete PlantUML section entirely**

Remove lines 49-79 (PlantUML content starting from `### PlantUML` through the code block).

**Step 2: Rename and expand draw.io section**

Change `## Diagram Tools` to `## draw.io Quick Start` and replace content with:

```markdown
## draw.io Quick Start

### What is draw.io?

draw.io (diagrams.net) is a free, web-based diagramming tool.

**Key advantages:**
- Free and no installation required
- Export to PNG, SVG, PDF
- Version control friendly (.drawio is XML)
- Large library of built-in shapes

### Getting Started

1. Go to [diagrams.net](https://diagrams.net) or [drawio.com](https://www.drawio.com)
2. Click "Create New Diagram"
3. Select a template or start blank
4. Drag shapes from left panel to canvas
5. Double-click shapes to add text
6. Connect shapes with arrows
7. File → Export as → PNG

### Interface Overview

```
+----------------------------------+
|  File  Edit  View  Arrange        |
+----------------------------------+
| Shapes  |    Canvas              |
| General |                        |
| Arrows  |                        |
|         |                        |
+----------------------------------+
```
```

**Step 3: Verify changes**

Run: `sed -n '31,70p' writing-guide/06-diagrams.md`
Expected: draw.io Quick Start section with numbered steps

---

### Task 3: Simplify Diagram Standards Section

**Files:**
- Modify: `writing-guide/06-diagrams.md:81-114`

**Step 1: Update File Format table**

Replace the File Format table (lines 95-101) with:

```markdown
### File Format

| Format | Purpose |
|--------|---------|
| .drawio | Source file (always commit) |
| .png | Documentation (max width: 800px) |

**Always commit both .drawio and .png files.**
```

**Step 2: Remove redundant content**

Keep only:
- File Naming (lines 84-92)
- File Format (updated above)
- Markdown Syntax (lines 111-114)

Remove: Image Size section (lines 104-108)

**Step 3: Verify changes**

Run: `sed -n '81,110p' writing-guide/06-diagrams.md`
Expected: Clean standards section with 3 subsections

---

### Task 4: Rewrite Common Diagram Types (Part 1 - Architecture & Deployment)

**Files:**
- Modify: `writing-guide/06-diagrams.md:116-217`

**Step 1: Delete all existing PlantUML diagram examples**

Remove lines 116-217 (all PlantUML code blocks for System Architecture, Data Flow, Sequence, Deployment).

**Step 2: Write merged Architecture & Deployment section**

```markdown
## Common Diagram Types

### System Architecture & Deployment

**Use when:** High-level system overview, infrastructure layout

**Key elements:**
- Rectangles for services/components
- Cylinders for databases
- Cloud shapes for external services
- Arrows for data flow

**Steps:**
1. Identify major components
2. Group related components (Arrange → Group)
3. Draw arrows showing relationships
4. Label each component clearly
5. Add legend if using colors

**Best practices:**
- One idea per diagram
- Consistent shapes for same component types
- Label arrows with relationship types

**Common mistakes:**
- Too many components (keep under 10)
- Mixing abstraction levels
- Unlabeled arrows

---

### Sequence & Flow Diagrams

**Use when:** API flows, authentication, multi-step processes

**Key elements:**
- Vertical lifelines for participants
- Horizontal arrows for messages
- Diamonds for decisions (flowcharts)
- Time flows downward

**Steps:**
1. List all participants (left to right)
2. Draw vertical lifelines
3. Add arrows for each message/step
4. Label each arrow with action
5. Number steps if complex

**Best practices:**
- Participants at top (left-to-right = call order)
- Label every arrow
- Show error paths if important

**Common mistakes:**
- Too many lifelines (keep under 6)
- Missing return messages
- Unclear step order
```

**Step 3: Verify structure**

Run: `grep -n "###" writing-guide/06-diagrams.md | head -10`
Expected: Clean headings for merged diagram types

---

### Task 5: Add ER Diagram & C4 Model Sections

**Files:**
- Modify: `writing-guide/06-diagrams.md` (append after Sequence & Flow section)

**Step 1: Add ER Diagram & Data Model section**

```markdown

### ER Diagram & Data Model

**Use when:** Database schema, data relationships

**Key elements:**
- Rectangles for entities (tables)
- Ovals for attributes
- Lines for relationships
- Crow's foot for cardinality

**Steps:**
1. List all entities
2. Identify relationships (1:1, 1:N, N:M)
3. Draw entities with attributes
4. Connect with relationship lines
5. Add cardinality markers

**Best practices:**
- Show primary keys clearly
- Indicate foreign key relationships
- Group related entities

**Common mistakes:**
- Missing cardinality
- Too many attributes in one diagram
- Inconsistent notation

---

### C4 Model

**Use when:** Software architecture documentation

**Levels:**
1. **Context:** System boundaries + external actors
2. **Container:** Applications and data stores
3. **Component:** Internal structure (rarely needed)

**Best practices:**
- Use color to distinguish system types
- Show key interactions only
- Reference detailed docs for specifics
```

**Step 2: Verify all diagram types are present**

Run: `grep "^###" writing-guide/06-diagrams.md`
Expected: 4 main diagram type headings

---

### Task 6: Rewrite Best Practices Section

**Files:**
- Modify: `writing-guide/06-diagrams.md:218-248`

**Step 1: Simplify Best Practices heading**

Change from `## Best Practices` to keep it, update content:

```markdown
## Best Practices

### Good Diagram Practices

1. **One idea per diagram** - Don't overcrowd
2. **Consistent styling** - Same shape = same component type
3. **Clear labels** - Label every component and arrow
4. **Appropriate detail** - High-level: major components only

### Common Mistakes

```markdown
# Bad: Too much detail
[Diagram with 50 boxes showing every detail]

# Good: Focused diagrams
[High-level architecture diagram]
[Separate diagram for specific flow]
```
```

**Step 2: Remove Example section**

Delete the "Example: Documenting with Diagrams" section (lines 250-274).

**Step 3: Update See Also section**

Replace See Also (lines 276-279) with:

```markdown
## See Also

- [draw.io Examples](https://www.drawio.com/example-diagrams) - Browse by category
- [Architecture Documentation](03-architecture.md) - Documenting system design
```

**Step 4: Verify file completion**

Run: `wc -l writing-guide/06-diagrams.md`
Expected: Approximately 150-180 lines (reduced from 280)

---

### Task 7: Final Verification and Commit

**Files:**
- Modify: `writing-guide/06-diagrams.md`

**Step 1: Verify no PlantUML content remains**

Run: `grep -i "plantuml\|puml" writing-guide/06-diagrams.md`
Expected: No matches (exit code 1)

**Step 2: Verify draw.io focus**

Run: `grep -c "draw.io\|drawio" writing-guide/06-diagrams.md`
Expected: At least 5 mentions

**Step 3: Check all required sections exist**

Run: `grep "^##" writing-guide/06-diagrams.md`
Expected output:
```markdown
## Purpose
## When to Use Diagrams
## draw.io Quick Start
## Diagram Standards
## Common Diagram Types
## Best Practices
## See Also
```

**Step 4: View final file for manual review**

Run: `cat writing-guide/06-diagrams.md`
Action: Review the complete file to ensure flow and consistency

**Step 5: Commit changes**

```bash
git add writing-guide/06-diagrams.md
git commit -m "docs: rewrite 06-diagrams.md to focus on draw.io only

- Remove all PlantUML content
- Add draw.io quick start guide
- Merge diagram types (Architecture & Deployment, Sequence & Flow)
- Add ER Diagram and C4 Model sections
- Include step-by-step instructions for each diagram type
- Add specific best practices and common mistakes per type
- Link to drawio.com/example-diagrams"
```

**Step 6: Verify commit**

Run: `git log -1 --stat`
Expected: Shows modification to writing-guide/06-diagrams.md

---

## Completion Criteria

- [ ] No PlantUML content remains
- [ ] All sections follow approved design
- [ ] File length is ~150-180 lines
- [ ] All 4 diagram types have: Use case, Key elements, Steps, Best practices, Common mistakes
- [ ] See Also links to drawio.com/example-diagrams
- [ ] Committed with descriptive message
