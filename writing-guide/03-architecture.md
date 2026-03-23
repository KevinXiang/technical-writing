# Architecture Documentation

Explaining how your system is built and why.

## Purpose

Architecture documentation helps:
- New developers understand the big picture
- Teams make consistent technical decisions
- Future maintainers understand design trade-offs
- Onboarding by providing mental models

## When to Write Architecture Docs

- New project or major feature
- Significant refactoring
- Distributed system design
- Microservices architecture
- Complex data flows

## Architecture Document Structure

### Overview Section

```markdown
# System Architecture

## Overview
[Project Name] is a [type] system that [what it does].

The system consists of [N] main components:
- [Component A] - [Purpose]
- [Component B] - [Purpose]
- [Component C] - [Purpose]
```

### System Diagram

```markdown
## System Overview

![System Overview](images/architecture.png)

The system follows a [pattern name] architecture:
```

### Component Descriptions

```markdown
## Components

### API Gateway
Routes incoming requests to appropriate services.
- **Technology:** Nginx
- **Responsibility:** Request routing, rate limiting, SSL termination

### Auth Service
Handles authentication and authorization.
- **Technology:** Node.js + Passport.js
- **Responsibility:** JWT token issuance, validation

### Data Service
Manages all data operations.
- **Technology:** Python + SQLAlchemy
- **Responsibility:** CRUD operations, data validation
```

### Data Flow

```markdown
## Data Flow

1. Client request → API Gateway
2. API Gateway validates token → Auth Service
3. Auth Service validates → API Gateway
4. API Gateway routes → Data Service
5. Data Service queries → Database
6. Data returns through same path

![Data Flow](images/data-flow.png)
```

### Technology Decisions

```markdown
## Technology Choices

### Why [Technology]?

**Reasons:**
- Reason 1
- Reason 2

**Alternatives Considered:**
- [Alternative A] - Not chosen because [reason]
- [Alternative B] - Not chosen because [reason]
```

## Diagram Best Practices

### ✅ Good Diagram Practices
0. **Drawio example-diagrams**
   - https://www.drawio.com/example-diagrams
1. **Show the right level of detail**
   - High-level overview: Show major components only
   - Detailed view: One component at a time

2. **Use consistent shapes and colors**
   - Same component type = same shape/color
   - Include a legend if needed

3. **Label everything clearly**
   - Component names (what)
   - Technology labels (how)
   - Data flow arrows (direction)

4. **Keep it readable**
   - Max width: 800px
   - Text size: readable at 100% zoom

### ❌ Common Diagram Mistakes

```markdown
# Bad: Too much detail in one diagram
[Diagram with 50 boxes showing every function call]

# Good: Separate concerns
[High-level architecture diagram with 5-6 boxes]
[Detailed sequence diagram for specific flow]
```

## Example: Simple Architecture Doc

```markdown
# Task Queue System Architecture

## Overview
A distributed task queue system that processes background jobs asynchronously.

## Architecture

![Architecture](images/architecture.png)

The system uses a producer-consumer pattern:
- **Producer API** - Accepts tasks from clients
- **Redis Queue** - Stores pending tasks
- **Worker Pool** - Processes tasks
- **Result Store** - Stores completed results

## Data Flow
1. Client submits task via Producer API
2. Task serialized and pushed to Redis queue
3. Workers poll Redis for available tasks
4. Worker processes task and pushes result
5. Client polls for result completion

## Scaling
- Horizontal scaling: Add more workers
- Vertical scaling: Increase worker concurrency
- Queue partitioning: Separate queues by priority

## Technology Choices
- **Redis:** Fast in-memory operations, Pub/Sub support
- **Considered:** RabbitMQ (more features but heavier)
```

## See Also

- [Creating Diagrams](06-diagrams.md) - How to create effective diagrams
- [README Documentation](02-readme.md) - Architecture section in README
- [Complex Project Example](../examples/complex-project/docs/architecture.md)
