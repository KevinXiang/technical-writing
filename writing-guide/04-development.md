# Development Guides

Helping developers set up and contribute to your project.

## Purpose

Development guides help:
- **New team members get productive quickly** - Clear setup steps reduce onboarding time
- **Ensure consistent development environments** - Everyone works with the same tools and configurations
- **Document workflows and conventions** - Standardize how work gets done
- **Reduce "how do I..." questions** - Anticipate and answer common questions upfront

## Essential Sections

Every development guide should include these sections:

### 1. Prerequisites

What developers need before they start.

**✅ Good: Specific and Versioned**
```markdown
## Prerequisites

### Required Tools

- **Node.js:** 18.0.0 or higher
- **npm:** 8.0.0 or higher
- **Docker:** 20.10+ (for local database)
- **Git:** 2.30+

### Verify Your Setup

```bash
node --version   # Should be v18.x.x or higher
npm --version    # Should be 8.x.x or higher
docker --version # Should be 20.10.x or higher
git --version    # Should be 2.30.x or higher
```

### Install Prerequisites

**macOS:**
```bash
brew install node@18 docker git
```

**Windows:**
- Download Node.js from [nodejs.org](https://nodejs.org)
- Download Docker Desktop from [docker.com](https://www.docker.com/products/docker-desktop)
- Git is included with [Git for Windows](https://git-scm.com/download/win)

**Linux (Ubuntu):**
```bash
sudo apt update
sudo apt install nodejs npm docker.io git
```
```

**❌ Bad: Vague**
```markdown
## Prerequisites
- Node.js
- Docker
- Git
```

---

### 2. Quick Setup

Fast path to get the project running locally.

**✅ Good: Step-by-Step with Explanations**
```markdown
## Quick Setup

### Step 1: Clone the Repository

```bash
git clone https://github.com/team/project.git
cd project
```

### Step 2: Install Dependencies

```bash
npm install
```

This installs all required packages and sets up the development environment.

### Step 3: Set Up Environment Variables

```bash
cp .env.example .env
# Edit .env with your configuration values
```

Required environment variables:
- `DATABASE_URL` - PostgreSQL connection string
- `API_KEY` - Your API key for external services
- `PORT` - Server port (default: 3000)

### Step 4: Start Development Services

```bash
# Start the database (Docker)
docker-compose up -d

# Run database migrations
npm run db:migrate

# Start the development server
npm run dev
```

The application will be available at [http://localhost:3000](http://localhost:3000)

### Verify Setup

```bash
npm run health-check
# Expected output: ✅ All systems operational
```
```

---

### 3. Project Structure

Help developers understand how the code is organized.

**✅ Good: Clear Tree with Explanations**
```markdown
## Project Structure

```
project/
├── src/
│   ├── api/              # API endpoints and routes
│   │   ├── routes/       # Route definitions
│   │   ├── middleware/   # Express middleware
│   │   └── controllers/  # Request handlers
│   ├── services/         # Business logic layer
│   │   ├── user/         # User-related operations
│   │   ├── auth/         # Authentication logic
│   │   └── email/        # Email service
│   ├── models/           # Database models
│   ├── utils/            # Utility functions
│   └── config/           # Configuration files
├── tests/                # Test files
│   ├── unit/             # Unit tests
│   ├── integration/      # Integration tests
│   └── fixtures/         # Test data
├── docs/                 # Additional documentation
├── scripts/              # Utility scripts
├── .env.example          # Environment variables template
├── package.json          # Dependencies and scripts
└── README.md             # Project overview
```

### Key Directories Explained

- **`src/api/`** - All HTTP endpoints. Each route has its own file.
- **`src/services/`** - Business logic separated from API layer. Reusable across different endpoints.
- **`src/models/`** - Database schema and queries using our ORM.
- **`tests/`** - Mirrors the `src/` structure for easy navigation.
```

---

### 4. Development Workflow

How to work with the codebase day-to-day.

```markdown
## Development Workflow

### Running the Application

**Development Mode (with hot reload):**
```bash
npm run dev
```

**Production Mode (for testing):**
```bash
npm run build
npm start
```

**Watching for Changes:**
The dev server automatically restarts when you save files.

### Running Tests

```bash
# Run all tests
npm test

# Run tests in watch mode
npm run test:watch

# Run tests with coverage report
npm run test:coverage

# Run only unit tests
npm run test:unit

# Run only integration tests
npm run test:integration
```

### Code Quality

```bash
# Check code style
npm run lint

# Auto-fix linting issues
npm run lint:fix

# Format code
npm run format

# Run type checking (if TypeScript)
npm run type-check
```

### Pre-commit Checks

We use Husky to run checks before each commit:
- Linting
- Type checking
- Unit tests

To skip checks (not recommended):
```bash
git commit --no-verify -m "message"
```
```

---

### 5. Common Development Tasks

Step-by-step guides for frequent activities.

```markdown
## Common Tasks

### Adding a New API Endpoint

1. Create the route file in `src/api/routes/`
2. Add controller logic in `src/api/controllers/`
3. Add business logic in `src/services/`
4. Add tests in `tests/integration/`
5. Update API documentation

```bash
# Example: Add a user profile endpoint
npm run generate:endpoint user-profile
```

### Running Database Migrations

```bash
# Create a new migration
npm run db:migrate:create add_user_preferences

# Run pending migrations
npm run db:migrate

# Rollback last migration
npm run db:migrate:rollback

# View migration status
npm run db:migrate:status
```

### Seeding Test Data

```bash
# Seed database with sample data
npm run db:seed

# Reset database (drop, create, migrate, seed)
npm run db:reset
```

### Debugging

```bash
# Start with debugger enabled
npm run debug

# Debug tests
npm run test:debug
```

Then attach your debugger (VS Code, Chrome DevTools, etc.) to the specified port.

### Updating Dependencies

```bash
# Check for outdated packages
npm outdated

# Update packages (interactive)
npm update -i

# Update to latest versions
npm update --latest
```
```

---

### 6. Code Style & Conventions

Keep code consistent across the team.

```markdown
## Code Style & Conventions

### Naming Conventions

- **Files:** `kebab-case.js` (e.g., `user-service.js`)
- **Variables:** `camelCase` (e.g., `userId`)
- **Constants:** `UPPER_SNAKE_CASE` (e.g., `API_BASE_URL`)
- **Classes:** `PascalCase` (e.g., `UserService`)
- **Private properties:** Leading underscore (e.g., `_internalMethod`)

### Commit Messages

We follow [Conventional Commits](https://www.conventionalcommits.org/):

```
feat: add user authentication
fix: resolve login timeout issue
docs: update API documentation
refactor: simplify user service logic
test: add integration tests for checkout
chore: update dependencies
```

**Format:** `type: subject`

**Types:** `feat`, `fix`, `docs`, `style`, `refactor`, `test`, `chore`

### Branch Naming

```
feature/description       # New features
bugfix/description        # Bug fixes
hotfix/description        # Urgent production fixes
refactor/description      # Code refactoring
docs/description          # Documentation updates
```

**Example:** `feature/add-oauth-login`

### Pull Request Guidelines

1. **Title:** Use conventional commit format
2. **Description:** Explain what and why, not how
3. **Link issues:** Include `Fixes #123` or `Closes #456`
4. **Tests:** All tests must pass
5. **Review:** At least one approval required

**PR Template:**
```markdown
## What
Brief description of changes

## Why
Reason for this change

## How
Implementation approach (brief)

## Testing
How this was tested

## Checklist
- [ ] Tests added/updated
- [ ] Documentation updated
- [ ] All tests passing
```
```

---

### 7. Troubleshooting

Solutions to common problems.

```markdown
## Troubleshooting

### "Module not found" Errors

**Symptom:** `Error: Cannot find module 'xyz'`

**Solution:**
```bash
rm -rf node_modules package-lock.json
npm install
```

---

### Port Already in Use

**Symptom:** `Error: listen EADDRINUSE: address already in use :::3000`

**Solution:**

**macOS/Linux:**
```bash
lsof -ti:3000 | xargs kill -9
```

**Windows:**
```bash
netstat -ano | findstr :3000
taskkill /PID <PID> /F
```

**Or use a different port:**
```bash
PORT=3001 npm run dev
```

---

### Database Connection Fails

**Symptom:** `Connection refused` or `ECONNREFUSED`

**Solution:**

1. Check Docker is running:
```bash
docker ps
```

2. Verify environment variables in `.env`:
```bash
cat .env | grep DATABASE_URL
```

3. Check database logs:
```bash
docker-compose logs db
```

4. Restart database:
```bash
docker-compose restart db
```

---

### Tests Pass Locally but Fail in CI

**Common causes:**

1. **Node.js version mismatch**
```bash
node --version  # Should match CI version
```

2. **Environment variables missing**
```bash
cp .env.example .env
```

3. **Cached test data**
```bash
npm run test:clean
npm test
```

---

### Docker Container Exits Immediately

**Symptom:** Docker container starts and stops immediately

**Solution:**

1. Check container logs:
```bash
docker-compose logs <service-name>
```

2. Check for syntax errors in config files:
```bash
docker-compose config
```

3. Rebuild the container:
```bash
docker-compose up -d --build
```

---

### Getting More Help

If none of these solutions work:

1. Check existing [GitHub Issues](https://github.com/team/project/issues)
2. Search internal documentation
3. Ask in team Slack channel: `#dev-help`
4. Create a new issue with:
   - Error message (full stack trace)
   - Steps to reproduce
   - Your environment (OS, Node version, etc.)
```

---

## When to Include Additional Sections

Depending on your project, you may also need:

### For API Projects
- Authentication/authorization setup
- API key generation
- Rate limiting configuration
- Mocking external services

### For Frontend Projects
- Component development workflow
- Storybook setup
- Design system usage
- Browser testing

### For Data Projects
- Sample data acquisition
- Data pipeline setup
- Jupyter notebook usage
- Model training procedures

### For DevOps/Infrastructure
- Local Kubernetes setup
- Terraform development
- CI/CD pipeline testing
- Environment-specific configurations

---

## See Also

- [README Documentation](02-readme.md) - Quick Start section
- [API Documentation](05-api-docs.md) - API development guidelines
- [Architecture Documentation](03-architecture.md) - System design overview
- [Simple Project Example](../examples/simple-project/docs/development.md)
- [Complex Project Example](../examples/complex-project/docs/development.md)
