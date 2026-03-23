# README Documentation

Your project's front door. Make it welcoming.

## Purpose

The README is often the first thing people see. It should answer:
- What is this project?
- Why does it exist?
- How do I get started?
- Where can I learn more?

## Standard README Structure

### For Simple Projects

```markdown
# [Project Name]

[One-line description]

## Overview
[2-3 sentences explaining what the project does and its value]

## Key Features
- Feature 1
- Feature 2
- Feature 3

## Architecture
[Brief description of how it's structured]

## Tech Stack
- Language/Framework
- Key libraries

## Quick Start
\`\`\`bash
git clone repo
cd project
npm install
npm start
\`\`\`

## Usage
[Basic usage examples]

## Configuration
[Key configuration options]

## Testing
\`\`\`bash
npm test
\`\`\`

## Deployment
[Deployment instructions]

## API Documentation
[Link to detailed API docs if applicable]

## Performance & Scalability
[Performance characteristics, limitations]

## FAQ
**Q: Common question?**
A: Answer
```

### For Complex Projects

Same structure as simple projects, with these additions:
- Link to sub-documentation for detailed sections
- Architecture section links to `docs/architecture.md`
- API section links to `docs/api/` directory
- Include contributor guidelines

## README Best Practices

### ✅ Good Examples

#### Clear Title and Description
```markdown
# ImageCompressor

A lightweight image compression library that reduces file size by 80% while maintaining visual quality.
```

#### Specific, Actionable Features
```markdown
## Key Features
- Compress JPEG, PNG, and WebP formats
- Batch processing support
- Configurable quality levels (1-100)
- Preserve metadata (EXIF, IPTC)
- CLI and programmatic API
```

#### Working Quick Start
```markdown
## Quick Start
\`\`\`bash
# Install
npm install @team/imagecompressor

# Use in code
const compressor = require('@team/imagecompressor');
await compressor.compress('input.jpg', 'output.jpg', { quality: 80 });
\`\`\`
```

### ❌ Bad Examples

#### Vague Description
```markdown
# MyProject

A project that does things.
```

#### Generic Features
```markdown
## Features
- Easy to use
- Fast
- Great performance
```

#### Non-Working Quick Start
```markdown
## Quick Start
npm install
node app.js
# (Note: you need to set up API keys first)
```

## Badges

Use badges sparingly and purposefully:

```markdown
# ProjectName

![Build Status](https://img.shields.io/github/actions/workflow/status/user/repo/build.yml)
![Version](https://img.shields.io/npm/v/package)
![License](https://img.shields.io/npm/l/package)
```

**Only include badges that:**
- Provide actionable information (build status, version)
- Are relevant to your users (not just vanity metrics)
- Stay current (remove broken badges immediately)

## Screenshots and Demos

For user-facing projects, add a screenshot or GIF after the description:

```markdown
# ProjectName

Brief description.

![Demo](docs/images/demo.gif)

## Key Features
...
```

## See Also

- [Architecture Documentation](03-architecture.md) - Documenting system design
- [Style Guide](07-style-guide.md) - Writing conventions
- [Simple Project Example](../examples/simple-project/README.md)
- [Complex Project Example](../examples/complex-project/README.md)
