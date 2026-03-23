# Design: Reorganize Complex-Project Example

**Date:** 2026-03-23
**Status:** Approved
**Author:** Claude + User collaboration

## Overview

Reorganize `examples/complex-project` to use a modular documentation structure where the entry README provides high-level overview and links to detailed functional module documentation.

## Goals

1. **Entry README** - High-level overview only (Product, Architecture, Tech Stack, Quick Start)
2. **Functional Modules** - 4 core modules with complete documentation each
3. **Layered Structure** - modules/ for functional modules, docs/ for shared content
4. **Complete Module Docs** - Each module has full content (Overview, API, Development, Testing, Deployment, Troubleshooting)

## Directory Structure

```
examples/complex-project/
├── README.md                    # Entry point (High Level)
├── docs/
│   ├── images/
│   │   ├── architecture.png    # System architecture diagram
│   │   └── deployment.png      # Deployment architecture diagram
│   └── deployment.md           # Shared deployment documentation
├── modules/
│   ├── user-management/        # User Management module
│   │   ├── README.md           # Complete module documentation
│   │   ├── api/
│   │   │   └── user-api.md     # API details
│   │   └── images/
│   │       └── user-flow.png   # Module-specific diagrams
│   ├── product-catalog/        # Product Catalog module
│   │   ├── README.md
│   │   ├── api/
│   │   │   └── product-api.md
│   │   └── images/
│   ├── order-processing/       # Order Processing module
│   │   ├── README.md
│   │   ├── api/
│   │   │   └── order-api.md
│   │   └── images/
│   └── payment-processing/     # Payment Processing module
│       ├── README.md
│       ├── api/
│       │   └── payment-api.md
│       └── images/
```

## Entry README Structure

```markdown
# E-Commerce Platform

> A scalable microservices-based e-commerce platform

## Overview
[2-3 paragraphs describing product positioning and value]

## Architecture
[High-level architecture description + diagram link]

![System Architecture](docs/images/architecture.png)

The system consists of 4 core functional modules:
- **User Management** - Authentication, authorization, user profiles
- **Product Catalog** - Product search, categorization, inventory display
- **Order Processing** - Cart, checkout, order lifecycle
- **Payment Processing** - Payment gateway integration, transactions

## Tech Stack
### Services
- Node.js 18.x (User, Order, Payment)
- Python 3.11 (Product)
- Go 1.21 (Inventory)

### Infrastructure
- API Gateway: Kong 3.x
- Message Queue: RabbitMQ 3.12
- Database: PostgreSQL 15
- Cache: Redis 7

## Quick Start
[Prerequisites + setup commands]

## Functional Modules
| Module | Description | Documentation |
|--------|-------------|---------------|
| [User Management](modules/user-management/) | Authentication, profiles | [详细文档 →](modules/user-management/) |
| [Product Catalog](modules/product-catalog/) | Products, categories, search | [详细文档 →](modules/product-catalog/) |
| [Order Processing](modules/order-processing/) | Cart, checkout, orders | [详细文档 →](modules/order-processing/) |
| [Payment Processing](modules/payment-processing/) | Payment gateways | [详细文档 →](modules/payment-processing/) |

## Development
## Deployment
## FAQ
```

## Module README Structure (Unified)

Each module's README.md follows this structure:

```markdown
# [Module Name]

> [One-line description]

## Overview
[Module overview - 2-3 paragraphs]

## Architecture
[Module architecture + diagrams]

### Key Components
- Component A - Description
- Component B - Description

## Tech Stack
[Language, Framework, Database, Dependencies]

## API Documentation
### Core Endpoints
[Table of main endpoints]

For complete API reference, see [API Documentation](api/xxx-api.md)

## Configuration
### Environment Variables
[Table of variables]

## Development
### Prerequisites
### Setup
### Running Locally
### Common Tasks

## Testing
[Unit, Integration, E2E test commands]

## Deployment
[Reference to shared deployment guide]

## Troubleshooting
### Common Issues
[Issue + Solution format]

## FAQ
[Module-specific FAQ]

## See Also
[Links to main README and related docs]
```

## Functional Modules

### 1. User Management
- Authentication (JWT, OAuth2)
- User profiles and preferences
- Address book management
- Order history

### 2. Product Catalog
- Product CRUD
- Category management
- Product search and filtering
- Pricing management

### 3. Order Processing
- Shopping cart
- Checkout flow
- Order lifecycle management
- Order history

### 4. Payment Processing
- Payment gateway integration
- Payment processing
- Refund handling
- Transaction history

## Rewrite Scope

### Create/Rewrite
| File | Status | Description |
|------|--------|-------------|
| `README.md` | Rewrite | Entry document, High Level |
| `docs/deployment.md` | Rewrite | Shared deployment guide |
| `docs/images/architecture.png` | Create | System architecture diagram |

### Delete
| File | Description |
|------|------|
| `docs/architecture.md` | Content merged into entry README and module docs |
| `docs/development.md` | Content distributed to module docs |
| `docs/troubleshooting.md` | Content distributed to module docs |
| `docs/contributing.md` | Delete or simplify |
| `docs/api/` | API docs moved to module api/ directories |

### Create New - Modules
Each module gets:
- `README.md` - Complete documentation (5+ pages)
- `api/xxx-api.md` - Detailed API documentation
- `images/` - Module-specific diagrams

## Implementation Phases

### Phase 1: Base Structure
1. Create directory structure
2. Rewrite entry README.md
3. Create docs/deployment.md

### Phase 2: Module Documentation
4. Create user-management module docs
5. Create product-catalog module docs
6. Create order-processing module docs
7. Create payment-processing module docs

### Phase 3: Diagrams & Cleanup
8. Create architecture diagrams
9. Delete old documentation
10. Final review and adjustments

## Success Criteria

- [ ] Entry README is concise (~100-150 lines) with high-level overview
- [ ] All 4 functional modules have complete README documentation
- [ ] Each module has API documentation in api/ subdirectory
- [ ] Old monolithic docs are removed or consolidated
- [ ] All internal links work correctly
- [ ] Diagrams are created and properly referenced
