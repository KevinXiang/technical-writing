# Quick Start Guide

Get your project documented in 5 minutes.

## Essential README Elements

Every README should include these 13 sections:

1. **Title & Description** - Project name and one-line description
2. **Overview** - Clear and concise project description (2-3 paragraphs)
3. **Key Features** - 3-5 bullet points of main capabilities
4. **Architecture** - System architecture and key components
5. **Tech Stack** - Technologies and tools used
6. **Quick Start** - Fast path to get running
7. **Usage** - Basic and advanced usage examples
8. **Configuration** - Environment variables and settings
9. **Testing** - How to run tests
10. **Deployment** - Deployment instructions
11. **API Documentation** - API reference (if applicable)
12. **Performance & Scalability** - Performance characteristics
13. **FAQ** - Common questions and answers

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

## Diagram Tools Quick Reference

| Tool | Best For | File Format |
|------|----------|-------------|
| **draw.io** | Architecture, deployment, component diagrams | `.drawio` → `.png` |
| **PlantUML** | Data flow, sequence, structural diagrams | `.puml` → `.png` |

**Workflow:**
1. Create diagram in draw.io or PlantUML
2. Export as PNG (max width: 800px)
3. Commit both source (.drawio/.puml) and PNG to your repo
4. Reference in markdown: `![Architecture](docs/images/architecture.png)`

## Next Steps

- Read the [Complete Writing Guide](writing-guide/) for detailed guidelines
- Browse [Examples](examples/) to see standards in action
- Explore [Templates](resources/templates/) for more document types

---

**Need help?** Check the [Writing Guide](writing-guide/) or review the [Examples](examples/).
