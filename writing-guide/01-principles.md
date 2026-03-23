# Documentation Principles

The foundation of good technical documentation.

## Why Documentation Matters

Good documentation:
- **Reduces onboarding time** - New developers become productive faster
- **Reduces support burden** - Questions are answered before they're asked
- **Captures institutional knowledge** - Knowledge isn't lost when people leave
- **Enables scalability** - Teams can grow without constant hand-holding
- **Improves code quality** - Documentation forces clear thinking

## Reader-First Mindset

Always write with your reader in mind:

### ✅ Good: Reader-focused
```markdown
To deploy the application, run `npm run deploy` from the project root.
```

### ❌ Bad: Writer-focused
```markdown
I usually deploy by running npm run deploy.
```

### Questions to ask before writing:
1. Who is reading this? (New developer? Experienced teammate? External user?)
2. What do they already know?
3. What do they need to accomplish?
4. What might confuse them?

## What to Document

### Must Document:

**Essential Sections (every README):**
- **Title & Description** - What is this project?
- **Overview** - Clear explanation of purpose and value
- **Quick Start** - How to get it running immediately
- **Usage** - How to use it

**Common Sections (include when applicable):**
- **Key Features** - Main capabilities (3-5 bullet points)
- **Architecture** - System design and components
- **Tech Stack** - Technologies and tools used
- **Configuration** - Environment variables and settings
- **Testing** - How to run tests
- **Deployment** - Deployment instructions
- **API Documentation** - API reference (for libraries/APIs)
- **Performance & Scalability** - Performance characteristics
- **FAQ** - Common questions and answers

**Optional Sections:**
- **Contributing** - Contribution guidelines
- **License** - License information
- **Changelog** - Version history

**Guideline:** Start with the essential sections, then add common sections based on your project's needs.

### Don't Document:
- Implementation details obvious from code
- Comments that repeat what code does (not why)
- Temporary workarounds (fix the code instead)
- Obsolete features (remove them instead)

## The Documentation Pyramid

```
                 │
      Overview ───┤── 1 page, high level
                 │
      Guides ─────┤── 5-10 pages, how-to
                 │
      Reference ──┤── Detailed, lookup
                 │
─────────────────┴─────────────────
```

Start with overview documents, add guides as needed, reference material comes last.

## Common Pitfalls

### ❌ Assuming too much knowledge
```markdown
# Bad: Assumes reader knows your jargon
Connect to the FLDB and query the UM.
```

```markdown
# Good: Explains terms
Connect to the Feature Database (FLDB) and query the User Management (UM) table.
```

### ❌ Writing novels
```markdown
# Bad: 50 paragraphs before any useful information
The history of authentication begins in ancient times...
```

```markdown
# Good: Gets to the point immediately
## Authentication
This API uses JWT tokens for authentication. Include your token in the Authorization header.
```

### ❌ Being too brief
```markdown
# Bad: One-line command with no context
npm install
```

```markdown
# Good: Explains what and why
# Install dependencies
npm install

This installs all required packages listed in package.json.
```

## Documentation Quality Checklist

Before publishing documentation, verify:
- [ ] Is the purpose clear in the first paragraph?
- [ ] Are all commands copy-pasteable and tested?
- [ ] Are code examples complete and working?
- [ ] Is technical terminology explained or linked?
- [ ] Is the structure logical and scannable?
- [ ] Will this be accurate in 6 months?

## See Also

- [Style Guide](07-style-guide.md) - Writing conventions
- [README Documentation](02-readme.md) - README-specific guidelines
