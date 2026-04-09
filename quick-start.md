# Quick Start Guide

Get your project documented in 5 minutes.

## Core vs Extended Elements

Documentation should match your project's needs. Start with the core elements, then add extended sections as your project grows.

### Core Elements (Start Here)

Every README should include these 5 core sections:

| Section | Purpose | Time to Write |
|---------|---------|---------------|
| **Title & Description** | Project name and one-line description | 1 min |
| **Overview** | What it does and why it exists | 3 min |
| **Quick Start** | Fast path to get running | 5 min |
| **Usage** | Basic usage examples | 5 min |
| **Configuration** | Essential settings | 2 min |

**Total time: ~15-20 minutes for a complete basic README**

### Extended Elements (Add as Needed)

Add these sections when they provide value to your users:

| Section | When to Include | Purpose |
|----------|-----------------|---------|
| **Key Features** | Product has multiple capabilities | Highlight main selling points |
| **Architecture** | Complex system or design decisions | Explain how it's built |
| **Tech Stack** | Project has notable dependencies | List technologies and versions |
| **Testing** | Contributions accepted | How to run tests |
| **Deployment** | Deployable by users | Deployment instructions |
| **API Documentation** | Exposes API or library interface | API reference |
| **Performance & Scalability** | Performance is a concern | Performance characteristics |
| **FAQ** | Common questions from users | Pre-answer questions |
| **Contributing** | Open source or team project | Contribution guidelines |
| **License** | Publicly shared | License information |
| **Changelog** | Versioned releases | Version history |

## Choose Your Template

```
Is your project multi-module or distributed?
├── No → Use Simple Project Template
│   └── Single codebase, < 5 main files
└── Yes → Use Complex Project Template
    └── Multiple services, modules, or components
```

## Copy & Get Started

### Simple Project

```bash
# Copy the template
cp resources/templates/readme-simple.md your-project/README.md

# Edit the placeholders
# Replace [BRACKETED_TEXT] with your project details
```

### Complex Project

```bash
# Copy the template
cp resources/templates/readme-complex.md your-project/README.md

# Create docs directory structure
mkdir -p your-project/docs/{api,images}

# Copy additional templates as needed
cp resources/templates/*.md your-project/docs/
```

## Minimal README Example

For the quickest start, here's a minimal template:

```markdown
# [Project Name]

> [One-line description]

## Overview

[2-3 sentences explaining what the project does and its value]

## Quick Start

\`\`\`bash
# Install
[install command]

# Run
[start command]
\`\`\`

## Usage

[Basic usage example]

\`\`\`[language]
[code example]
\`\`\`

## Configuration

| Variable | Description | Default |
|----------|-------------|---------|
| [VAR_NAME] | [Description] | [default] |

---

**Project:** [Project Name]
**Documentation Last Updated:** [Date]
```

**Copy this, replace the placeholders, and you're done in 5 minutes.**

Add more sections from the extended elements list as your project grows.

## Diagram Tools Quick Reference

| Tool | Best For | File Format |
|------|----------|-------------|
| **draw.io** | Architecture, deployment, component diagrams | `.drawio` → `.png` |
| **draw.io** | Data flow, sequence, structural diagrams | `.drawio` → `.png` |

**Workflow:**
1. Create diagram in draw.io
2. Export as PNG (max width: 800px)
3. Commit both source (.drawio) and PNG to your repo
4. Reference in markdown: `![Architecture](docs/images/architecture.png)`

## Next Steps

- Read the [Complete Writing Guide](writing-guide/) for detailed guidelines
- Browse [Examples](examples/) to see standards in action
- Explore [Templates](resources/templates/) for more document types

---

**Need help?** Check the [Writing Guide](writing-guide/) or review the [Examples](examples/).
