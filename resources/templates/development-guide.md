# Development Guide

> [Project Name] - Developer Guide
<!-- Tip: Keep this document focused on development workflow. User-facing documentation belongs in README.md -->

## Overview

This guide provides comprehensive information for developers working on [Project Name], including setup, workflows, coding standards, and best practices.

**Audience:** New developers joining the project, contributors, and maintainers.
<!-- Tip: Specifying the audience helps set appropriate detail level -->

## Table of Contents

1. [Getting Started](#getting-started)
2. [Development Environment](#development-environment)
3. [Project Structure](#project-structure)
4. [Development Workflow](#development-workflow)
5. [Coding Standards](#coding-standards)
6. [Testing](#testing)
7. [Debugging](#debugging)
8. [Common Tasks](#common-tasks)
9. [Troubleshooting](#troubleshooting)

## Getting Started

### Prerequisites

Before you begin, ensure you have the following installed:

- **[Language/Runtime]** - [Version] ([Download link])
- **[Package Manager]** - [Version] ([Download link])
- **[Database]** - [Version] ([Download link])
- **[Docker]** - [Version] (for local infrastructure) ([Download link])
- **[IDE/Editor]** - [Recommended IDE] ([Download link])

<!-- Tip: Providing specific versions helps developers avoid compatibility issues -->

### Initial Setup

```bash
# 1. Clone the repository
git clone [repository-url]
cd [project-name]

# 2. Install dependencies
[npm install | pip install -r requirements.txt | cargo install]

# 3. Copy environment template
cp .env.example .env

# 4. Edit .env with your configuration
# Set required environment variables

# 5. Start infrastructure services (if using Docker)
docker-compose up -d

# 6. Run database migrations (if applicable)
[npm run db:migrate | python manage.py migrate | cargo db migrate]

# 7. Seed database (optional)
[npm run db:seed | python manage.py seed | cargo db seed]

# 8. Start the development server
[npm run dev | python run.py | cargo run]

# 9. Verify installation
# Open http://localhost:3000 in your browser
```

<!-- Tip: Numbered steps make the setup process easier to follow.
     Consider adding estimated time for each step. -->

### Verifying Setup

```bash
# Run health check
curl http://localhost:3000/health

# Run smoke tests
npm run test:smoke

# Check database connection
npm run db:check
```

## Development Environment

### Recommended Tools

- **IDE:** [Recommended IDE with configuration]
- **API Client:** [Postman/Insomnia/cURL]
- **Database Client:** [TablePlus/DBeaver/pgAdmin]
- **Git Client:** [GitKraken/SourceTree/CLI]

### IDE Configuration

#### VS Code

Install recommended extensions:
- [Extension 1] - [Purpose]
- [Extension 2] - [Purpose]
- [Extension 3] - [Purpose]

Workspace settings are included in `.vscode/settings.json`.

### Environment Variables

Create a `.env` file in the project root:

```bash
# Required
DATABASE_URL=postgresql://user:password@localhost:5432/dbname
API_KEY=your-api-key

# Optional
LOG_LEVEL=debug
PORT=3000
```

See `.env.example` for all available variables.

### Services Configuration

#### Database

- **Host:** localhost
- **Port:** 5432
- **Database:** [database-name]
- **User:** [username]
- **Password:** [password]

#### Redis (if applicable)

- **Host:** localhost
- **Port:** 6379

#### Message Queue (if applicable)

- **Host:** localhost
- **Port:** 5672

## Project Structure

```
project-name/
├── docs/                  # Documentation
├── src/                   # Source code
│   ├── api/              # API handlers/controllers
│   ├── models/           # Data models
│   ├── services/         # Business logic
│   ├── utils/            # Utility functions
│   └── index.js          # Entry point
├── tests/                 # Test files
│   ├── unit/             # Unit tests
│   ├── integration/      # Integration tests
│   └── e2e/              # End-to-end tests
├── scripts/               # Build and deployment scripts
├── config/                # Configuration files
├── .github/               # GitHub-specific files
│   └── workflows/        # CI/CD workflows
├── .env.example           # Environment variables template
├── .gitignore            # Git ignore rules
├── package.json          # Dependencies and scripts
└── README.md             # Project documentation
```

### Key Directories

#### `src/api/`
API endpoint handlers and route definitions.

#### `src/models/`
Data models and schemas.

#### `src/services/`
Business logic and service layer.

#### `src/utils/`
Helper functions and utilities.

#### `tests/`
All test files organized by type.

## Development Workflow

### Git Workflow

We use a [feature branch/Git flow/fork] workflow:

```bash
# 1. Create a feature branch from main
git checkout main
git pull origin main
git checkout -b feature/your-feature-name

# 2. Make your changes
# Commit frequently with clear messages

# 3. Push to remote
git push origin feature/your-feature-name

# 4. Create a pull request
# Use the GitHub UI to create a PR

# 5. Address review feedback
# Make changes and push updates

# 6. After approval, merge and delete branch
```

### Commit Message Format

Follow conventional commits:

```
type(scope): subject

body

footer
```

**Types:** `feat`, `fix`, `docs`, `style`, `refactor`, `test`, `chore`

**Examples:**
```
feat(auth): add OAuth2 login support

Implement OAuth2 authentication flow with Google
and GitHub providers.

Closes #123
```

### Code Review Process

1. Create pull request with clear description
2. Request review from [number] reviewers
3. Address all review comments
4. Ensure all checks pass
5. Squash commits if needed
6. Merge after approval

## Coding Standards

### Code Style

We follow [style guide: Airbnb/Google/Standard/etc.]

**Formatting:**
- Use [Prettier/Black/gofmt] for automatic formatting
- Run before committing: `npm run format`

**Linting:**
- Linter: [ESLint/Pylint/golint]
- Configuration: `.eslintrc.js` / `.pylintrc` / `.golangci.yml`
- Run: `npm run lint`

### Naming Conventions

#### Variables and Functions
- Use `camelCase` for variables and functions
- Use descriptive names that indicate purpose

#### Classes
- Use `PascalCase` for class names
- Use descriptive nouns

#### Constants
- Use `UPPER_SNAKE_CASE` for constants

#### Files
- Use `kebab-case` for file names
- Match file name to primary export

### Code Organization

#### Module Structure

```javascript
// 1. Imports
import { foo } from 'foo';
import { bar } from 'bar';

// 2. Constants
const MAX_ITEMS = 100;

// 3. Types/Interfaces
interface Item {
  id: string;
  name: string;
}

// 4. Functions
function getItem(id: string): Item {
  // implementation
}

// 5. Exports
export { getItem };
```

### Documentation

#### Function Documentation

```javascript
/**
 * Retrieves a user by ID
 *
 * @param {string} userId - The unique user identifier
 * @param {Object} options - Additional options
 * @param {boolean} options.includeProfile - Include user profile
 * @returns {Promise<User>} The user object
 * @throws {NotFoundError} If user not found
 *
 * @example
 * const user = await getUser('123', { includeProfile: true });
 */
async function getUser(userId, options = {}) {
  // implementation
}
```

#### Inline Comments

- Use comments to explain **why**, not **what**
- Keep comments up-to-date with code changes
- Use TODO/FIXME comments for temporary notes

## Testing

### Test Structure

```
tests/
├── unit/
│   └── [module].test.js
├── integration/
│   └── [feature].test.js
└── e2e/
    └── [scenario].test.js
```

### Writing Tests

#### Unit Tests

```javascript
describe('UserService', () => {
  describe('getUser', () => {
    it('should return user when found', async () => {
      const user = await userService.getUser('123');
      expect(user).toBeDefined();
      expect(user.id).toBe('123');
    });

    it('should throw NotFoundError when not found', async () => {
      await expect(userService.getUser('999'))
        .rejects.toThrow(NotFoundError);
    });
  });
});
```

#### Integration Tests

```javascript
describe('API Integration', () => {
  it('should create user via API', async () => {
    const response = await request(app)
      .post('/api/users')
      .send({ name: 'John Doe' });

    expect(response.status).toBe(201);
    expect(response.body.name).toBe('John Doe');
  });
});
```

### Running Tests

```bash
# Run all tests
npm test

# Run unit tests only
npm run test:unit

# Run integration tests only
npm run test:integration

# Run with coverage
npm run test:coverage

# Run watch mode
npm run test:watch

# Run specific test file
npm test -- path/to/test.test.js
```

### Test Coverage

- Target: [X]% coverage
- Minimum per file: [Y]%
- Check coverage report: `open coverage/index.html`

## Debugging

### Local Debugging

#### Using VS Code Debugger

Configuration included in `.vscode/launch.json`:

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "type": "node",
      "request": "launch",
      "name": "Launch Program",
      "program": "${workspaceFolder}/src/index.js"
    }
  ]
}
```

#### Debugging Tests

```bash
# Run tests in debug mode
npm run test:debug

# Run specific test in debug mode
npm test -- --debug path/to/test.test.js
```

### Logging

#### Log Levels

- **ERROR:** Application errors
- **WARN:** Warning messages
- **INFO:** General information
- **DEBUG:** Detailed debugging info

#### Log Format

```json
{
  "timestamp": "2024-01-01T00:00:00Z",
  "level": "INFO",
  "message": "User created",
  "context": {
    "userId": "123",
    "action": "create"
  }
}
```

#### Viewing Logs

```bash
# View all logs
npm run logs

# View error logs
npm run logs:error

# Tail logs
npm run logs:tail
```

## Common Tasks

### Adding a New Feature

1. Create feature branch
2. Implement feature following coding standards
3. Write tests
4. Update documentation
5. Create pull request

### Adding a New API Endpoint

1. Define route in `src/api/routes/`
2. Create handler in `src/api/handlers/`
3. Add request/response validation
4. Write integration tests
5. Update API documentation

### Database Migration

```bash
# Create migration
npm run db:migrate:create -- migration-name

# Run migration
npm run db:migrate

# Rollback migration
npm run db:migrate:rollback
```

### Updating Dependencies

```bash
# Check for updates
npm outdated

# Update a package
npm update package-name

# Update all packages
npm update

# Audit for vulnerabilities
npm audit
npm audit fix
```

## Troubleshooting

### Common Issues

#### Issue: Database connection fails

**Solution:**
```bash
# Check if database is running
docker-compose ps

# Restart database
docker-compose restart db

# Check connection string
echo $DATABASE_URL
```

#### Issue: Tests fail with timeout

**Solution:**
```bash
# Increase test timeout
# In test configuration:
jest: {
  testTimeout: 10000
}
```

#### Issue: Port already in use

**Solution:**
```bash
# Find process using port
lsof -i :3000

# Kill process
kill -9 [PID]

# Or use different port
PORT=3001 npm start
```

### Getting Help

- Check existing [GitHub Issues](link to issues)
- Create new issue with:
  - Description of problem
  - Steps to reproduce
  - Expected vs actual behavior
  - Environment details
- Contact: [email/slack channel]

## Related Documentation

- [Architecture Documentation](architecture.md)
- [API Documentation](api.md)
- [Deployment Guide](deployment.md)
- [Contributing Guide](contributing.md)

---

**Document Version:** 1.0
**Last Updated:** [Date]
**Maintained by:** [Team name]
