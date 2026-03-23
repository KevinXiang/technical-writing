# Creating Diagrams with draw.io

When and how to use diagrams effectively in software development.

## Purpose

Diagrams help when:
- Explaining complex relationships
- Showing system architecture
- Illustrating data flows
- Documenting deployment topologies
- Onboarding new developers

## When to Use Diagrams

### Use Diagrams For

- System architecture (high-level overview)
- Data flow through components
- Sequence of operations
- Network topology
- Deployment architecture

### Don't Use For

- Simple linear processes (text is clearer)
- Implementation details (code comments are better)
- Temporary workarounds (fix the code instead)
- Obsolete architectures (update the diagram)

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


## Diagram Standards

### File Naming

Use kebab-case matching the diagram subject:
```
images/
├── system-overview.png
├── data-flow.png
├── deployment-architecture.png
└── api-sequence.png
```

### File Format

| Format | Purpose |
|--------|---------|
| .drawio | Source file (always commit) |
| .png | Documentation (max width: 800px) |

**Always commit both .drawio and .png files.**

### Markdown Syntax

```markdown
![Alt text](relative/path/to/image.png)
```

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

## See Also

- [draw.io Examples](https://www.drawio.com/example-diagrams) - Browse by category
- [Architecture Documentation](03-architecture.md) - Documenting system design
