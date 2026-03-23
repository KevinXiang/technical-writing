# Development Guides

Helping developers set up and contribute to your project.

## Purpose

Development guides help:
- **New team members get productive quickly** - Clear setup steps reduce onboarding time
- **Ensure consistent development environments** - Everyone works with the same tools and configurations
- **Document workflows and conventions** - Standardize how work gets done
- **Reduce "how do I..." questions** - Anticipate and answer common questions upfront

## Essential Sections

Every development guide should include these 7 sections:

1. **Prerequisites** - Required tools and how to install them
2. **Quick Setup** - Step-by-step guide to get running
3. **Project Structure** - How the code is organized
4. **Development Workflow** - Day-to-day commands and operations
5. **Common Development Tasks** - Frequently used procedures
6. **Code Style & Conventions** - Naming, commits, branch patterns
7. **Troubleshooting** - Common problems and solutions

---

## 1. Prerequisites

What developers need before they start.

### Structure

- **Required Tools** - List with specific versions
- **Verify Your Setup** - Commands to check versions
- **Install Prerequisites** - OS-specific instructions

### Example

> ## Prerequisites
>
> ### Required Tools
> - **Node.js:** 18.0.0 or higher
> - **npm:** 8.0.0 or higher
> - **Docker:** 20.10+ (for local database)
> - **Git:** 2.30+
>
> ### Verify Your Setup
> ```bash
> node --version   # Should be v18.x.x or higher
> npm --version    # Should be 8.x.x or higher
> docker --version # Should be 20.10.x or higher
> git --version    # Should be 2.30.x or higher
> ```
>
> ### Install Prerequisites
>
> **macOS:**
> ```bash
> brew install node@18 docker git
> ```
>
> **Windows:**
> - Download Node.js from [nodejs.org](https://nodejs.org)
> - Download Docker Desktop from [docker.com](https://www.docker.com/products/docker-desktop)
> - Git is included with [Git for Windows](https://git-scm.com/download/win)

### Key Points
- Always specify version numbers
- Provide verification commands
- Include installation instructions for multiple OSs

---

## 2. Quick Setup

Fast path to get the project running locally.

### Structure

- **Step-by-step numbered instructions**
- **Explanation for each step** (what it does, why it's needed)
- **Expected output/verification**

### Example

> ## Quick Setup
>
> ### Step 1: Clone the Repository
> ```bash
> git clone https://github.com/team/project.git
> cd project
> ```
>
> ### Step 2: Install Dependencies
> ```bash
> npm install
> ```
> This installs all required packages and sets up the development environment.
>
> ### Step 3: Set Up Environment Variables
> ```bash
> cp .env.example .env
> # Edit .env with your configuration values
> ```
>
> ### Step 4: Start Development Services
> ```bash
> docker-compose up -d
> npm run db:migrate
> npm run dev
> ```
>
> The application will be available at http://localhost:3000

### Key Points
- Number each step for clarity
- Explain what commands do
- Include verification step

---

## 3. Project Structure

Help developers understand how the code is organized.

### Structure

- **Directory tree** with inline comments
- **Key directories explained** with their purposes

### Example

> ## Project Structure
>
> ```
> project/
> ├── src/
> │   ├── api/              # API endpoints and routes
> │   ├── services/         # Business logic layer
> │   ├── models/           # Database models
> │   └── utils/            # Utility functions
> ├── tests/                # Test files
> ├── docs/                 # Additional documentation
> └── package.json          # Dependencies and scripts
> ```
>
> ### Key Directories
> - **`src/api/`** - All HTTP endpoints. Each route has its own file.
> - **`src/services/`** - Business logic separated from API layer.
> - **`tests/`** - Mirrors the `src/` structure for easy navigation.

### Key Points
- Use inline comments for directory purposes
- Explain the most important directories
- Keep tree concise (don't show every file)

---

## 4. Development Workflow

How to work with the codebase day-to-day.

### Structure

- **Running the application** (dev mode, production mode)
- **Running tests** (all, watch, coverage)
- **Code quality tools** (lint, format, type-check)
- **Pre-commit checks** (what runs automatically)

### Example

> ## Development Workflow
>
> ### Running the Application
> ```bash
> # Development mode (with hot reload)
> npm run dev
>
> # Production mode (for testing)
> npm run build && npm start
> ```
>
> ### Running Tests
> ```bash
> npm test              # All tests
> npm run test:watch    # Watch mode
> npm run test:coverage # Coverage report
> ```
>
> ### Code Quality
> ```bash
> npm run lint      # Check code style
> npm run lint:fix  # Auto-fix issues
> npm run format    # Format code
> ```

### Key Points
- Group commands by purpose
- Include watch/coverage variants
- Explain pre-commit hooks

---

## 5. Common Development Tasks

Step-by-step guides for frequent activities.

### Structure

- **Named tasks** with clear headings
- **Numbered steps** or commands
- **Explanations** for what each step does

### Example

> ## Common Tasks
>
> ### Adding a New API Endpoint
>
> 1. Create the route file in `src/api/routes/`
> 2. Add controller logic in `src/api/controllers/`
> 3. Add business logic in `src/services/`
> 4. Add tests in `tests/integration/`
> 5. Update API documentation
>
> ### Running Database Migrations
> ```bash
> npm run db:migrate:create add_user_preferences
> npm run db:migrate
> npm run db:migrate:rollback
> ```
>
> ### Debugging
> ```bash
> npm run debug      # Start with debugger
> npm run test:debug # Debug tests
> ```

### Key Points
- Include tasks developers do frequently
- Provide both steps and commands
- Keep descriptions concise

---

## 6. Code Style & Conventions

Keep code consistent across the team.

### Structure

- **Naming conventions** (files, variables, classes)
- **Commit message format** with examples
- **Branch naming** patterns
- **Pull request guidelines**

### Example

> ## Code Style & Conventions
>
> ### Naming Conventions
> - **Files:** `kebab-case.js` (e.g., `user-service.js`)
> - **Variables:** `camelCase` (e.g., `userId`)
> - **Constants:** `UPPER_SNAKE_CASE` (e.g., `API_BASE_URL`)
> - **Classes:** `PascalCase` (e.g., `UserService`)
>
> ### Commit Messages
>
> Follow [Conventional Commits](https://www.conventionalcommits.org/):
>
> ```
> feat: add user authentication
> fix: resolve login timeout issue
> docs: update API documentation
> ```
>
> ### Branch Naming
> ```
> feature/description  # New features
> bugfix/description   # Bug fixes
> hotfix/description   # Urgent fixes
> ```
>
> ### Pull Request Guidelines
> 1. Title: Use conventional commit format
> 2. Description: Explain what and why
> 3. Link issues: Include `Fixes #123`
> 4. Tests: All tests must pass

### Key Points
- Provide concrete examples for each convention
- Link to external standards
- Keep rules simple and memorable

---

## 7. Troubleshooting

Solutions to common problems.

### Structure

- **Problem/Solution format**
- **Symptoms** clearly described
- **OS-specific solutions** when applicable
- **Getting help** section

### Example

> ## Troubleshooting
>
> ### "Module not found" Errors
>
> **Symptom:** `Error: Cannot find module 'xyz'`
>
> **Solution:**
> ```bash
> rm -rf node_modules package-lock.json
> npm install
> ```
>
> ---
>
> ### Port Already in Use
>
> **Symptom:** `Error: listen EADDRINUSE: address already in use :::3000`
>
> **Solution:**
>
> **macOS/Linux:**
> ```bash
> lsof -ti:3000 | xargs kill -9
> ```
>
> **Windows:**
> ```bash
> netstat -ano | findstr :3000
> taskkill /PID <PID> /F
> ```
>
> ### Database Connection Fails
>
> **Symptom:** `Connection refused`
>
> **Solution:**
> 1. Check Docker is running: `docker ps`
> 2. Verify `.env` variables
> 3. Check database logs: `docker-compose logs db`
> 4. Restart database: `docker-compose restart db`

### Key Points
- Group by problem type
- Include clear symptoms
- Provide step-by-step solutions
- Add "Getting More Help" section

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
