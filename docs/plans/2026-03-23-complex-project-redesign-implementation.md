# Complex-Project Reorganization Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Reorganize `examples/complex-project` to use modular documentation structure with entry README as high-level overview and detailed functional module documentation.

**Architecture:** Layered structure with entry README.md for high-level overview, docs/ for shared content, and modules/ for 4 functional modules (user-management, product-catalog, order-processing, payment-processing).

**Tech Stack:** Markdown documentation, draw.io for diagrams

---

### Task 1: Create Directory Structure

**Files:**
- Create: `examples/complex-project/docs/images/`
- Create: `examples/complex-project/modules/user-management/api/`
- Create: `examples/complex-project/modules/user-management/images/`
- Create: `examples/complex-project/modules/product-catalog/api/`
- Create: `examples/complex-project/modules/product-catalog/images/`
- Create: `examples/complex-project/modules/order-processing/api/`
- Create: `examples/complex-project/modules/order-processing/images/`
- Create: `examples/complex-project/modules/payment-processing/api/`
- Create: `examples/complex-project/modules/payment-processing/images/`

**Step 1: Create all directories**

```bash
cd examples/complex-project

# Create docs/images directory
mkdir -p docs/images

# Create module directories
mkdir -p modules/user-management/{api,images}
mkdir -p modules/product-catalog/{api,images}
mkdir -p modules/order-processing/{api,images}
mkdir -p modules/payment-processing/{api,images}
```

**Step 2: Verify structure**

Run: `ls -laR examples/complex-project/modules/`
Expected: All 4 module directories with api/ and images/ subdirectories

**Step 3: Create placeholder .gitkeep files**

```bash
touch docs/images/.gitkeep
touch modules/user-management/images/.gitkeep
touch modules/product-catalog/images/.gitkeep
touch modules/order-processing/images/.gitkeep
touch modules/payment-processing/images/.gitkeep
```

**Step 4: Commit**

```bash
git add examples/complex-project/
git commit -m "feat: create modular directory structure for complex-project

- Create docs/images/ for shared diagrams
- Create 4 module directories with api/ and images/ subdirectories
- Prepare for modular documentation approach"
```

---

### Task 2: Rewrite Entry README.md

**Files:**
- Modify: `examples/complex-project/README.md` (complete rewrite)

**Step 1: Backup original README**

```bash
cd examples/complex-project
cp README.md README.md.backup
```

**Step 2: Write new entry README**

Replace entire README.md with:

```markdown
# E-Commerce Platform

> A scalable microservices-based e-commerce platform

## Overview

E-Commerce Platform is a production-ready, scalable online marketplace built using a microservices architecture. The system handles thousands of concurrent users, processes millions of transactions, and provides real-time inventory management and order processing.

The platform is designed for high availability, with horizontal scaling capabilities, distributed data management, and event-driven communication between services.

## Architecture

The system follows an event-driven microservices architecture with API Gateway pattern:

![System Architecture](docs/images/architecture.png)

The system consists of 4 core functional modules:
- **User Management** - Authentication, authorization, and user profiles
- **Product Catalog** - Product search, categorization, and inventory display
- **Order Processing** - Cart, checkout, and order lifecycle management
- **Payment Processing** - Payment gateway integration and transaction handling

For detailed architecture documentation, see individual module documentation.

## Tech Stack

### Services
- **User Service:** Node.js 18.x + Express
- **Product Service:** Python 3.11 + FastAPI
- **Order Service:** Node.js 18.x + Express
- **Payment Service:** Node.js 18.x + Express
- **Inventory Service:** Go 1.21

### Infrastructure
- **API Gateway:** Kong 3.x
- **Message Queue:** RabbitMQ 3.12
- **Database:** PostgreSQL 15
- **Cache:** Redis 7
- **Search:** Elasticsearch 8

## Quick Start

### Prerequisites

- **Docker** - 20.x or higher
- **Docker Compose** - 2.x or higher
- **Node.js** - 18.x (for local development)
- **Python** - 3.11 (for local development)
- **Go** - 1.21 (for local development)

### Setup

```bash
# Clone the repository
git clone https://github.com/example/ecommerce-platform.git
cd ecommerce-platform

# Start infrastructure services
docker-compose up -d postgres redis rabbitmq elasticsearch

# Install dependencies for each service
make install-all

# Set up environment
cp .env.example .env

# Initialize databases
make db-migrate

# Start all services
make start-all
```

The application will be available at http://localhost:3000

## Functional Modules

| Module | Description | Documentation |
|--------|-------------|---------------|
| [User Management](modules/user-management/) | Authentication, profiles, preferences | [详细文档 →](modules/user-management/) |
| [Product Catalog](modules/product-catalog/) | Products, categories, search | [详细文档 →](modules/product-catalog/) |
| [Order Processing](modules/order-processing/) | Cart, checkout, orders | [详细文档 →](modules/order-processing/) |
| [Payment Processing](modules/payment-processing/) | Payment gateways, transactions | [详细文档 →](modules/payment-processing/) |

## Development

For development setup and common tasks, see individual module documentation.

## Deployment

For deployment instructions, see [Deployment Guide](docs/deployment.md)

## FAQ

### What happens when a service goes down?

The system is designed for high availability. If a service goes down:
- API Gateway returns 503 Service Unavailable
- Message queues buffer messages until service recovers
- Circuit breakers prevent cascading failures
- Health checks trigger automatic restarts

### How is data consistency maintained?

We use eventual consistency for distributed transactions:
- Sagas for order processing workflow
- Event sourcing for inventory updates
- Compensating transactions for rollbacks
- Periodic reconciliation jobs

### Can I add a new payment gateway?

Yes! See [Payment Processing Module](modules/payment-processing/) for details.

---

**Project:** E-Commerce Platform
**Documentation Last Updated:** 2026-03-23
```

**Step 3: Verify content**

Run: `head -50 examples/complex-project/README.md`
Expected: New high-level README with Overview, Architecture, Tech Stack, Quick Start sections

**Step 4: Commit**

```bash
git add examples/complex-project/README.md
git commit -m "docs: rewrite entry README as high-level overview

- Focus on product overview, architecture, tech stack, quick start
- Add functional modules table with links to detailed docs
- Remove detailed content, defer to module documentation
- Add FAQ section with common questions"
```

---

### Task 3: Create Shared Deployment Documentation

**Files:**
- Create: `examples/complex-project/docs/deployment.md`

**Step 1: Write deployment documentation**

Create `docs/deployment.md`:

```markdown
# Deployment Guide

> Deployment instructions for the E-Commerce Platform

## Overview

This guide covers deploying the E-Commerce Platform to different environments.

## Prerequisites

- Docker 20.x+
- Kubernetes cluster (for production)
- kubectl configured
- AWS/GCP/Azure account (for cloud deployment)

## Environment Setup

### Development Environment

The development environment runs locally using Docker Compose:

```bash
# Start all services
docker-compose up -d

# View logs
docker-compose logs -f

# Stop services
docker-compose down
```

### Staging Environment

```bash
# Deploy to staging
kubectl apply -f k8s/staging/

# Verify deployment
kubectl get pods -n staging

# Port forward to access
kubectl port-forward svc/api-gateway 3000:3000 -n staging
```

### Production Environment

```bash
# Deploy to production
kubectl apply -f k8s/production/

# Verify deployment
kubectl get pods -n production

# Check rollout status
kubectl rollout status deployment/api-gateway -n production
```

## Configuration

### Environment Variables

Each service requires specific environment variables:

| Variable | Description | Required |
|----------|-------------|----------|
| `DATABASE_URL` | PostgreSQL connection string | Yes |
| `REDIS_URL` | Redis connection string | Yes |
| `RABBITMQ_URL` | RabbitMQ connection string | Yes |
| `JWT_SECRET` | JWT signing secret | Yes |
| `PAYMENT_GATEWAY_KEY` | Payment gateway API key | Yes |

### Secrets Management

Use Kubernetes secrets for sensitive data:

```bash
# Create secret
kubectl create secret generic app-secrets \
  --from-literal=jwt-secret=YOUR_SECRET \
  --from-literal=database-url=YOUR_DB_URL \
  -n production
```

## Scaling

### Horizontal Scaling

```bash
# Scale specific service
kubectl scale deployment order-service --replicas=3 -n production

# Configure auto-scaling
kubectl autoscale deployment order-service \
  --cpu-percent=70 \
  --min=2 --max=10 -n production
```

### Vertical Scaling

Adjust resource requests in deployment manifests:

```yaml
resources:
  requests:
    cpu: "500m"
    memory: "512Mi"
  limits:
    cpu: "2000m"
    memory: "2Gi"
```

## Monitoring

### Health Checks

```bash
# Check service health
curl http://api-gateway/health

# Check all services
kubectl get pods -n production
```

### Logs

```bash
# View service logs
kubectl logs -f deployment/order-service -n production

# View all logs
kubectl logs -l app=ecommerce -n production --all-containers=true
```

## Rollback

```bash
# Rollback to previous version
kubectl rollout undo deployment/order-service -n production

# Rollback to specific revision
kubectl rollout undo deployment/order-service --to-revision=2 -n production
```

## Troubleshooting

### Pods Not Starting

```bash
# Check pod status
kubectl describe pod <pod-name> -n production

# Check logs
kubectl logs <pod-name> -n production
```

### Service Not Accessible

```bash
# Check service endpoints
kubectl get endpoints -n production

# Check ingress
kubectl get ingress -n production
```

## See Also

- [User Management Deployment](../modules/user-management/#deployment)
- [Product Catalog Deployment](../modules/product-catalog/#deployment)
- [Order Processing Deployment](../modules/order-processing/#deployment)
- [Payment Processing Deployment](../modules/payment-processing/#deployment)
```

**Step 2: Commit**

```bash
git add examples/complex-project/docs/deployment.md
git commit -m "docs: add shared deployment documentation

- Cover dev/staging/production environments
- Include scaling and monitoring guidance
- Add troubleshooting section"
```

---

### Task 4: Create User Management Module Documentation

**Files:**
- Create: `examples/complex-project/modules/user-management/README.md`
- Create: `examples/complex-project/modules/user-management/api/user-api.md`

**Step 1: Write User Management README**

Create `modules/user-management/README.md`:

```markdown
# User Management Module

> Authentication, authorization, and user profile management

## Overview

The User Management module handles all user-related functionality including authentication, authorization, user profiles, preferences, and address book management. It provides secure access control using JWT tokens and OAuth2 social login.

This module is responsible for:
- User registration and login
- JWT token generation and validation
- User profile management
- Address book CRUD operations
- User preferences and settings

## Architecture

![User Management Architecture](images/user-flow.png)

### Key Components

- **Auth Controller** - Handles authentication requests (login, register, logout)
- **User Controller** - Manages user profiles and preferences
- **Address Controller** - Manages address book operations
- **JWT Middleware** - Validates tokens and extracts user context
- **OAuth Service** - Handles social login integrations

## Tech Stack

- **Language:** Node.js 18.x
- **Framework:** Express 4.x
- **Database:** PostgreSQL 15
- **Cache:** Redis 7 (for active sessions)
- **Authentication:** JWT + Passport.js

## API Documentation

### Core Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | /api/v1/auth/register | Register new user |
| POST | /api/v1/auth/login | User login |
| POST | /api/v1/auth/refresh | Refresh JWT token |
| POST | /api/v1/auth/logout | User logout |
| GET | /api/v1/users/me | Get current user profile |
| PUT | /api/v1/users/me | Update user profile |
| GET | /api/v1/users/me/addresses | Get user addresses |
| POST | /api/v1/users/me/addresses | Add new address |

For complete API reference, see [API Documentation](api/user-api.md)

## Configuration

### Environment Variables

| Variable | Description | Default |
|----------|-------------|---------|
| `USER_SERVICE_PORT` | Service port | 3001 |
| `DATABASE_URL` | PostgreSQL connection | - |
| `REDIS_URL` | Redis connection | - |
| `JWT_SECRET` | JWT signing secret | - |
| `JWT_EXPIRES_IN` | Token expiration | 15m |
| `REFRESH_TOKEN_EXPIRES_IN` | Refresh token expiration | 30d |

### Configuration File

Configuration is loaded from `config/default.js` and `config/production.js`.

## Development

### Prerequisites

- Node.js 18.x or higher
- PostgreSQL 15
- Redis 7

### Setup

```bash
# Install dependencies
npm install

# Set up environment
cp .env.example .env
# Edit .env with your configuration

# Run database migrations
npm run db:migrate

# Seed development data
npm run db:seed
```

### Running Locally

```bash
# Start the service
npm start

# Start with hot reload
npm run dev

# Run in debug mode
npm run debug
```

The service will be available at http://localhost:3001

### Common Tasks

**Add a new authentication provider:**
1. Create strategy in `src/strategies/`
2. Register in `src/config/passport.js`
3. Add tests
4. Update documentation

**Add a new user field:**
1. Update database schema in `src/models/User.js`
2. Create migration
3. Update API validation
4. Add tests

## Testing

```bash
# Run all tests
npm test

# Run with coverage
npm run test:coverage

# Run integration tests
npm run test:integration

# Run specific test file
npm test -- tests/auth.controller.test.js
```

### Test Structure

```
tests/
├── unit/
│   ├── models/
│   └── services/
├── integration/
│   ├── auth.test.js
│   └── users.test.js
└── e2e/
    └── user-flows.test.js
```

## Deployment

### Build

```bash
# Build Docker image
docker build -t user-service:latest .

# Or build locally
npm run build
```

### Deploy

For deployment instructions, see [Deployment Guide](../../docs/deployment.md).

### Environment-Specific Configuration

- **Development:** Local PostgreSQL, Redis
- **Staging:** Shared staging database
- **Production:** Managed RDS, ElastiCache

## Troubleshooting

### Common Issues

**Issue: JWT token invalid**

**Symptom:** API returns 401 Unauthorized with "Invalid token"

**Solution:**
1. Check JWT_SECRET is consistent across services
2. Verify token hasn't expired (default 15 minutes)
3. Check token format: `Authorization: Bearer <token>`

**Issue: User not found after registration**

**Symptom:** Login fails immediately after registration

**Solution:**
1. Check database connection
2. Verify migration ran successfully
3. Check for duplicate email constraint violation

**Issue: Social login redirect loop**

**Symptom:** OAuth flow never completes

**Solution:**
1. Verify OAuth callback URL matches provider configuration
2. Check session storage (Redis)
3. Review OAuth provider console for errors

### Debug Mode

```bash
# Enable debug logging
DEBUG=user-service:* npm start

# Check Redis connection
redis-cli ping

# Check database connection
psql $DATABASE_URL -c "SELECT 1"
```

## FAQ

### How do I change token expiration?

Edit `JWT_EXPIRES_IN` and `REFRESH_TOKEN_EXPIRES_IN` in `.env` or environment variables.

### Can I use multiple OAuth providers?

Yes! See `src/config/passport.js` for examples. Add new providers following the same pattern.

### How is password security handled?

Passwords are hashed using bcrypt with 12 salt rounds. Never store plain-text passwords.

### How do I add two-factor authentication?

See the implementation guide in `docs/2fa-setup.md` (not yet implemented).

## See Also

- [Main Platform README](../../README.md)
- [User API Documentation](api/user-api.md)
- [Platform Architecture](../../docs/architecture.md)
```

**Step 2: Write User API Documentation**

Create `modules/user-management/api/user-api.md`:

```markdown
# User Management API

> Complete API reference for the User Management module

## Base URL

```
http://localhost:3001/api/v1
```

## Authentication

All endpoints (except register and login) require a valid JWT token:

```
Authorization: Bearer <your-jwt-token>
```

## Endpoints

### Authentication

#### Register User

Register a new user account.

**Request**
```http
POST /auth/register
Content-Type: application/json

{
  "email": "user@example.com",
  "password": "SecurePass123!",
  "name": "John Doe"
}
```

**Response** 201 Created
```json
{
  "id": "user_123",
  "email": "user@example.com",
  "name": "John Doe",
  "createdAt": "2024-01-15T10:30:00Z"
}
```

**Error Responses**
- 400 Bad Request - Invalid input
- 409 Conflict - Email already exists

#### Login

Authenticate and receive JWT token.

**Request**
```http
POST /auth/login
Content-Type: application/json

{
  "email": "user@example.com",
  "password": "SecurePass123!"
}
```

**Response** 200 OK
```json
{
  "accessToken": "eyJhbGc...",
  "refreshToken": "eyJhbGc...",
  "expiresIn": 900
}
```

#### Refresh Token

Get a new access token using refresh token.

**Request**
```http
POST /auth/refresh
Content-Type: application/json

{
  "refreshToken": "eyJhbGc..."
}
```

**Response** 200 OK
```json
{
  "accessToken": "eyJhbGc...",
  "expiresIn": 900
}
```

#### Logout

Invalidate refresh token.

**Request**
```http
POST /auth/logout
Authorization: Bearer <token>
```

**Response** 204 No Content

### User Profile

#### Get Current User

Get authenticated user's profile.

**Request**
```http
GET /users/me
Authorization: Bearer <token>
```

**Response** 200 OK
```json
{
  "id": "user_123",
  "email": "user@example.com",
  "name": "John Doe",
  "avatar": "https://cdn.example.com/avatars/user_123.jpg",
  "preferences": {
    "language": "en",
    "currency": "USD",
    "newsletter": true
  },
  "createdAt": "2024-01-15T10:30:00Z",
  "updatedAt": "2024-01-20T15:45:00Z"
}
```

#### Update User Profile

Update authenticated user's profile.

**Request**
```http
PUT /users/me
Authorization: Bearer <token>
Content-Type: application/json

{
  "name": "John Smith",
  "avatar": "https://cdn.example.com/avatars/new.jpg"
}
```

**Response** 200 OK
```json
{
  "id": "user_123",
  "email": "user@example.com",
  "name": "John Smith",
  "avatar": "https://cdn.example.com/avatars/new.jpg",
  "updatedAt": "2024-01-25T10:00:00Z"
}
```

### Address Book

#### List Addresses

Get user's saved addresses.

**Request**
```http
GET /users/me/addresses
Authorization: Bearer <token>
```

**Response** 200 OK
```json
{
  "addresses": [
    {
      "id": "addr_1",
      "type": "home",
      "street": "123 Main St",
      "city": "New York",
      "state": "NY",
      "zip": "10001",
      "country": "USA",
      "isDefault": true
    }
  ]
}
```

#### Create Address

Add a new address.

**Request**
```http
POST /users/me/addresses
Authorization: Bearer <token>
Content-Type: application/json

{
  "type": "home",
  "street": "456 Oak Ave",
  "city": "Los Angeles",
  "state": "CA",
  "zip": "90001",
  "country": "USA",
  "isDefault": false
}
```

**Response** 201 Created
```json
{
  "id": "addr_2",
  "type": "home",
  "street": "456 Oak Ave",
  "city": "Los Angeles",
  "state": "CA",
  "zip": "90001",
  "country": "USA",
  "isDefault": false
}
```

#### Delete Address

Delete an address.

**Request**
```http
DELETE /users/me/addresses/{addressId}
Authorization: Bearer <token>
```

**Response** 204 No Content

## Error Responses

All endpoints may return these error responses:

### 400 Bad Request
```json
{
  "error": "Bad Request",
  "message": "Invalid email format",
  "code": "INVALID_EMAIL"
}
```

### 401 Unauthorized
```json
{
  "error": "Unauthorized",
  "message": "Invalid or expired token",
  "code": "INVALID_TOKEN"
}
```

### 404 Not Found
```json
{
  "error": "Not Found",
  "message": "User not found",
  "code": "USER_NOT_FOUND"
}
```

### 500 Internal Server Error
```json
{
  "error": "Internal Server Error",
  "message": "An unexpected error occurred",
  "code": "INTERNAL_ERROR"
}
```

## Rate Limiting

- Login endpoint: 5 requests per 15 minutes per IP
- Register endpoint: 3 requests per hour per IP
- All other endpoints: 100 requests per minute per user

## See Also

- [User Management Module](../README.md)
- [Platform README](../../README.md)
```

**Step 3: Commit**

```bash
git add examples/complex-project/modules/user-management/
git commit -m "docs: add user management module documentation

- Complete README with overview, architecture, API, config, development
- Detailed API documentation in api/user-api.md
- Include testing, deployment, troubleshooting sections"
```

---

### Task 5: Create Product Catalog Module Documentation

**Files:**
- Create: `examples/complex-project/modules/product-catalog/README.md`
- Create: `examples/complex-project/modules/product-catalog/api/product-api.md`

**Step 1: Write Product Catalog README**

Create `modules/product-catalog/README.md`:

```markdown
# Product Catalog Module

> Product management, search, and categorization

## Overview

The Product Catalog module handles all product-related functionality including product CRUD operations, category management, product search, and inventory display integration. It provides fast product search using Elasticsearch and integrates with the Inventory Service for real-time stock levels.

This module is responsible for:
- Product creation and management
- Category hierarchy management
- Product search and filtering
- Pricing management
- Product image handling

## Architecture

![Product Catalog Architecture](images/product-flow.png)

### Key Components

- **Product Controller** - Handles product CRUD operations
- **Category Controller** - Manages category hierarchy
- **Search Service** - Elasticsearch integration for product search
- **Image Service** - Handles product image uploads to S3
- **Inventory Client** - Fetches real-time inventory levels

## Tech Stack

- **Language:** Python 3.11
- **Framework:** FastAPI 0.100
- **Database:** PostgreSQL 15
- **Search:** Elasticsearch 8
- **Storage:** AWS S3
- **ORM:** SQLAlchemy 2.0

## API Documentation

### Core Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | /api/v1/products | List products with filters |
| GET | /api/v1/products/:id | Get product details |
| POST | /api/v1/products | Create product |
| PUT | /api/v1/products/:id | Update product |
| DELETE | /api/v1/products/:id | Delete product |
| GET | /api/v1/products/search | Search products |
| GET | /api/v1/categories | List categories |
| POST | /api/v1/categories | Create category |

For complete API reference, see [API Documentation](api/product-api.md)

## Configuration

### Environment Variables

| Variable | Description | Default |
|----------|-------------|---------|
| `PRODUCT_SERVICE_PORT` | Service port | 3002 |
| `DATABASE_URL` | PostgreSQL connection | - |
| `ELASTICSEARCH_URL` | Elasticsearch URL | http://localhost:9200 |
| `S3_BUCKET` | S3 bucket for images | - |
| `AWS_ACCESS_KEY_ID` | AWS access key | - |
| `AWS_SECRET_ACCESS_KEY` | AWS secret key | - |

## Development

### Prerequisites

- Python 3.11+
- PostgreSQL 15
- Elasticsearch 8

### Setup

```bash
# Install dependencies
pip install -r requirements.txt

# Set up environment
cp .env.example .env

# Run database migrations
alembic upgrade head

# Create Elasticsearch index
python scripts/create_search_index.py

# Seed development data
python scripts/seed_data.py
```

### Running Locally

```bash
# Start the service
uvicorn main:app --reload

# Or using make
make run
```

The service will be available at http://localhost:3002

### Common Tasks

**Add a new product field:**
1. Update SQLAlchemy model in `models/product.py`
2. Create Alembic migration
3. Update Pydantic schemas
4. Update Elasticsearch mapping

**Add a new filter option:**
1. Add filter in `services/search_service.py`
2. Update API endpoint parameters
3. Add tests

## Testing

```bash
# Run all tests
pytest

# Run with coverage
pytest --cov=src --cov-report=html

# Run integration tests
pytest tests/integration/

# Run specific test
pytest tests/test_products.py -v
```

## Deployment

See [Deployment Guide](../../docs/deployment.md)

## Troubleshooting

### Common Issues

**Issue: Products not appearing in search**

**Symptom:** Search returns empty results

**Solution:**
1. Check Elasticsearch is running: `curl http://localhost:9200/_cluster/health`
2. Verify index exists: `curl http://localhost:9200/products`
3. Reindex products: `python scripts/reindex_products.py`

**Issue: Images not uploading**

**Symptom:** Image upload returns 500 error

**Solution:**
1. Verify S3 credentials in `.env`
2. Check S3 bucket permissions
3. Verify file size under limit (5MB)

## FAQ

### How do I bulk import products?

Use the bulk import script: `python scripts/bulk_import.py --file products.csv`

### Can I have multiple category levels?

Yes, categories support unlimited nesting using the `parent_id` field.

## See Also

- [Main Platform README](../../README.md)
- [Product API Documentation](api/product-api.md)
```

**Step 2: Write Product API Documentation**

Create `modules/product-catalog/api/product-api.md`:

```markdown
# Product Catalog API

> Complete API reference for the Product Catalog module

## Base URL

```
http://localhost:3002/api/v1
```

## Endpoints

### Products

#### List Products

Get paginated list of products with optional filtering.

**Request**
```http
GET /products?page=1&limit=20&category=electronics&sort=price_asc
```

**Parameters**
| Parameter | Type | Description |
|-----------|------|-------------|
| page | integer | Page number (default: 1) |
| limit | integer | Items per page (default: 20, max: 100) |
| category | string | Filter by category slug |
| search | string | Search in name/description |
| min_price | number | Minimum price |
| max_price | number | Maximum price |
| sort | string | Sort order: price_asc, price_desc, name_asc, name_desc, newest |

**Response** 200 OK
```json
{
  "products": [
    {
      "id": "prod_123",
      "name": "Wireless Headphones",
      "description": "High-quality wireless headphones",
      "price": 79.99,
      "sku": "WH-001",
      "category": {
        "id": "cat_1",
        "name": "Electronics",
        "slug": "electronics"
      },
      "images": ["https://cdn.example.com/products/wh-001.jpg"],
      "inventory": 45,
      "createdAt": "2024-01-15T10:30:00Z"
    }
  ],
  "pagination": {
    "page": 1,
    "limit": 20,
    "total": 156,
    "totalPages": 8
  }
}
```

#### Get Product

Get product details by ID.

**Request**
```http
GET /products/:id
```

**Response** 200 OK
```json
{
  "id": "prod_123",
  "name": "Wireless Headphones",
  "description": "High-quality wireless headphones with noise cancellation",
  "price": 79.99,
  "sku": "WH-001",
  "category": {
    "id": "cat_1",
    "name": "Electronics",
    "slug": "electronics"
  },
  "images": [
    "https://cdn.example.com/products/wh-001-1.jpg",
    "https://cdn.example.com/products/wh-001-2.jpg"
  ],
  "attributes": {
    "color": "Black",
    "weight": "250g",
    "battery": "30 hours"
  },
  "inventory": 45,
  "reviews": {
    "average": 4.5,
    "count": 128
  },
  "createdAt": "2024-01-15T10:30:00Z",
  "updatedAt": "2024-01-20T15:45:00Z"
}
```

**Error Responses**
- 404 Not Found - Product not found

#### Create Product

Create a new product (requires admin role).

**Request**
```http
POST /products
Authorization: Bearer <admin-token>
Content-Type: application/json

{
  "name": "Wireless Headphones",
  "description": "High-quality wireless headphones",
  "price": 79.99,
  "sku": "WH-001",
  "categoryId": "cat_1",
  "images": ["https://cdn.example.com/products/wh-001.jpg"],
  "attributes": {
    "color": "Black",
    "weight": "250g"
  }
}
```

**Response** 201 Created
```json
{
  "id": "prod_123",
  "name": "Wireless Headphones",
  "price": 79.99,
  "sku": "WH-001",
  "createdAt": "2024-01-25T10:00:00Z"
}
```

#### Update Product

Update product details.

**Request**
```http
PUT /products/:id
Authorization: Bearer <admin-token>
Content-Type: application/json

{
  "name": "Wireless Headphones Pro",
  "price": 99.99
}
```

**Response** 200 OK

#### Delete Product

Delete a product.

**Request**
```http
DELETE /products/:id
Authorization: Bearer <admin-token>
```

**Response** 204 No Content

### Search

#### Search Products

Full-text product search.

**Request**
```http
POST /products/search
Content-Type: application/json

{
  "query": "wireless headphones",
  "filters": {
    "category": "electronics",
    "minPrice": 50,
    "maxPrice": 100
  },
  "sort": "relevance",
  "page": 1,
  "limit": 20
}
```

**Response** 200 OK
```json
{
  "products": [...],
  "pagination": {...},
  "facets": {
    "categories": [
      {"name": "Electronics", "count": 45},
      {"name": "Accessories", "count": 12}
    ],
    "priceRanges": [
      {"range": "0-50", "count": 20},
      {"range": "50-100", "count": 35}
    ]
  }
}
```

### Categories

#### List Categories

Get category tree.

**Request**
```http
GET /categories
```

**Response** 200 OK
```json
{
  "categories": [
    {
      "id": "cat_1",
      "name": "Electronics",
      "slug": "electronics",
      "parentId": null,
      "children": [
        {
          "id": "cat_2",
          "name": "Audio",
          "slug": "audio",
          "parentId": "cat_1",
          "children": []
        }
      ]
    }
  ]
}
```

#### Create Category

Create a new category.

**Request**
```http
POST /categories
Authorization: Bearer <admin-token>
Content-Type: application/json

{
  "name": "Audio",
  "slug": "audio",
  "parentId": "cat_1"
}
```

**Response** 201 Created

## See Also

- [Product Catalog Module](../README.md)
- [Platform README](../../README.md)
```

**Step 3: Commit**

```bash
git add examples/complex-project/modules/product-catalog/
git commit -m "docs: add product catalog module documentation

- Complete README with overview, architecture, API, config
- Detailed API documentation with search endpoints
- Include testing, deployment, troubleshooting sections"
```

---

### Task 6: Create Order Processing Module Documentation

**Files:**
- Create: `examples/complex-project/modules/order-processing/README.md`
- Create: `examples/complex-project/modules/order-processing/api/order-api.md`

**Step 1: Write Order Processing README**

Create `modules/order-processing/README.md`:

```markdown
# Order Processing Module

> Shopping cart, checkout, and order lifecycle management

## Overview

The Order Processing module handles the complete order workflow from shopping cart to order completion. It implements the Saga pattern for distributed transaction management across inventory and payment services.

This module is responsible for:
- Shopping cart management
- Checkout flow orchestration
- Order creation and status tracking
- Order history and retrieval
- Cart abandonment handling

## Architecture

![Order Processing Architecture](images/order-flow.png)

### Key Components

- **Cart Controller** - Shopping cart CRUD operations
- **Order Controller** - Order management and lifecycle
- **Saga Orchestrator** - Coordinates distributed transactions
- **Event Publisher** - Publishes domain events
- **Event Consumer** - Handles events from other services

### Saga Pattern - Order Processing

```
1. Create Order (PENDING)
   ├─ Success → Publish OrderCreated
   └─ Failure → Return error to client

2. Reserve Inventory
   ├─ Success → Publish InventoryReserved
   └─ Failure → Compensate: Cancel Order

3. Process Payment
   ├─ Success → Publish PaymentCompleted
   └─ Failure → Compensate: Release Inventory, Cancel Order

4. Confirm Order
   ├─ Success → Publish OrderConfirmed
   └─ Failure → Compensate: Refund Payment
```

## Tech Stack

- **Language:** Node.js 18.x
- **Framework:** Express 4.x
- **Database:** PostgreSQL 15
- **Cache:** Redis 7 (for shopping carts)
- **Message Queue:** RabbitMQ 3.12

## API Documentation

### Core Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | /api/v1/cart | Get user's cart |
| POST | /api/v1/cart/items | Add item to cart |
| PUT | /api/v1/cart/items/:itemId | Update cart item |
| DELETE | /api/v1/cart/items/:itemId | Remove from cart |
| POST | /api/v1/checkout | Start checkout |
| GET | /api/v1/orders | List user's orders |
| GET | /api/v1/orders/:orderId | Get order details |
| POST | /api/v1/orders/:orderId/cancel | Cancel order |

For complete API reference, see [API Documentation](api/order-api.md)

## Configuration

### Environment Variables

| Variable | Description | Default |
|----------|-------------|---------|
| `ORDER_SERVICE_PORT` | Service port | 3003 |
| `DATABASE_URL` | PostgreSQL connection | - |
| `REDIS_URL` | Redis connection | - |
| `RABBITMQ_URL` | RabbitMQ connection | - |

## Development

### Prerequisites

- Node.js 18.x
- PostgreSQL 15
- Redis 7
- RabbitMQ 3.12

### Setup

```bash
# Install dependencies
npm install

# Set up environment
cp .env.example .env

# Run database migrations
npm run db:migrate

# Start the service
npm start
```

### Running Locally

```bash
# Start all services (including RabbitMQ)
docker-compose up -d

# Start order service
npm run dev
```

### Common Tasks

**Add a new order status:**
1. Update status enum in `models/Order.js`
2. Add state transition logic
3. Update saga orchestrator
4. Add tests

## Testing

```bash
# Run all tests
npm test

# Run integration tests (requires RabbitMQ)
npm run test:integration

# Run E2E tests
npm run test:e2e
```

## Deployment

See [Deployment Guide](../../docs/deployment.md)

## Troubleshooting

### Common Issues

**Issue: Cart items lost after update**

**Symptom:** Cart shows incorrect items after product update

**Solution:**
1. Check Redis connection
2. Verify cart session ID is consistent
3. Check for race conditions in cart updates

**Issue: Order stuck in PENDING status**

**Symptom:** Order never progresses from PENDING

**Solution:**
1. Check RabbitMQ: `docker-compose exec rabbitmq rabbitmqctl list_queues`
2. Verify event consumers are running
3. Check order service logs for errors

## FAQ

### How long are shopping carts retained?

Carts are retained for 30 days of inactivity, then automatically cleaned up.

### Can users modify pending orders?

Orders can only be cancelled within 1 hour of creation. Modifications are not supported.

## See Also

- [Main Platform README](../../README.md)
- [Order API Documentation](api/order-api.md)
```

**Step 2: Write Order API Documentation**

Create `modules/order-processing/api/order-api.md`:

```markdown
# Order Processing API

> Complete API reference for the Order Processing module

## Base URL

```
http://localhost:3003/api/v1
```

## Endpoints

### Shopping Cart

#### Get Cart

Get authenticated user's shopping cart.

**Request**
```http
GET /cart
Authorization: Bearer <token>
```

**Response** 200 OK
```json
{
  "id": "cart_123",
  "items": [
    {
      "productId": "prod_123",
      "name": "Wireless Headphones",
      "price": 79.99,
      "quantity": 2,
      "subtotal": 159.98
    }
  ],
  "totals": {
    "subtotal": 159.98,
    "tax": 16.00,
    "shipping": 5.99,
    "total": 181.97
  }
}
```

#### Add Item to Cart

Add a product to the cart.

**Request**
```http
POST /cart/items
Authorization: Bearer <token>
Content-Type: application/json

{
  "productId": "prod_123",
  "quantity": 2
}
```

**Response** 201 Created
```json
{
  "itemId": "item_456",
  "productId": "prod_123",
  "quantity": 2
}
```

#### Update Cart Item

Update item quantity.

**Request**
```http
PUT /cart/items/:itemId
Authorization: Bearer <token>
Content-Type: application/json

{
  "quantity": 3
}
```

**Response** 200 OK

#### Remove from Cart

Remove item from cart.

**Request**
```http
DELETE /cart/items/:itemId
Authorization: Bearer <token>
```

**Response** 204 No Content

### Checkout

#### Start Checkout

Initiate checkout process.

**Request**
```http
POST /checkout
Authorization: Bearer <token>
Content-Type: application/json

{
  "shippingAddress": {
    "name": "John Doe",
    "street": "123 Main St",
    "city": "New York",
    "state": "NY",
    "zip": "10001",
    "country": "USA"
  },
  "billingAddress": {
    "name": "John Doe",
    "street": "123 Main St",
    "city": "New York",
    "state": "NY",
    "zip": "10001",
    "country": "USA"
  },
  "paymentMethod": "stripe"
}
```

**Response** 200 OK
```json
{
  "orderId": "order_789",
  "status": "PENDING",
  "clientSecret": "pi_123..."
}
```

### Orders

#### List Orders

Get user's order history.

**Request**
```http
GET /orders?page=1&limit=20
Authorization: Bearer <token>
```

**Response** 200 OK
```json
{
  "orders": [
    {
      "id": "order_789",
      "status": "CONFIRMED",
      "total": 181.97,
      "createdAt": "2024-01-25T10:00:00Z",
      "items": [...]
    }
  ],
  "pagination": {
    "page": 1,
    "limit": 20,
    "total": 15
  }
}
```

#### Get Order

Get order details.

**Request**
```http
GET /orders/:orderId
Authorization: Bearer <token>
```

**Response** 200 OK
```json
{
  "id": "order_789",
  "status": "CONFIRMED",
  "items": [...],
  "shippingAddress": {...},
  "totals": {
    "subtotal": 159.98,
    "tax": 16.00,
    "shipping": 5.99,
    "total": 181.97
  },
  "timeline": [
    {
      "status": "PENDING",
      "timestamp": "2024-01-25T10:00:00Z"
    },
    {
      "status": "CONFIRMED",
      "timestamp": "2024-01-25T10:02:00Z"
    }
  ],
  "createdAt": "2024-01-25T10:00:00Z",
  "updatedAt": "2024-01-25T10:02:00Z"
}
```

#### Cancel Order

Cancel a pending order.

**Request**
```http
POST /orders/:orderId/cancel
Authorization: Bearer <token>
```

**Response** 200 OK
```json
{
  "id": "order_789",
  "status": "CANCELLED",
  "cancelledAt": "2024-01-25T10:05:00Z"
}
```

**Error Responses**
- 400 Bad Request - Order cannot be cancelled (not in PENDING status)
- 404 Not Found - Order not found

## See Also

- [Order Processing Module](../README.md)
- [Platform README](../../README.md)
```

**Step 3: Commit**

```bash
git add examples/complex-project/modules/order-processing/
git commit -m "docs: add order processing module documentation

- Complete README with cart, checkout, order management
- Detailed API documentation for all endpoints
- Include saga pattern explanation and troubleshooting"
```

---

### Task 7: Create Payment Processing Module Documentation

**Files:**
- Create: `examples/complex-project/modules/payment-processing/README.md`
- Create: `examples/complex-project/modules/payment-processing/api/payment-api.md`

**Step 1: Write Payment Processing README**

Create `modules/payment-processing/README.md`:

```markdown
# Payment Processing Module

> Payment gateway integration and transaction management

## Overview

The Payment Processing module handles all payment-related functionality including payment gateway integration, payment processing, refund handling, and transaction history. It supports multiple payment gateways (Stripe, PayPal) and provides secure payment processing.

This module is responsible for:
- Payment gateway integration
- Payment processing and validation
- Refund and partial refund handling
- Payment method management
- Transaction history and reporting

## Architecture

![Payment Processing Architecture](images/payment-flow.png)

### Key Components

- **Payment Controller** - Handles payment requests
- **Gateway Factory** - Creates appropriate gateway instances
- **Stripe Gateway** - Stripe integration
- **PayPal Gateway** - PayPal integration
- **Refund Service** - Handles refund operations
- **Event Publisher** - Publishes payment events

## Tech Stack

- **Language:** Node.js 18.x
- **Framework:** Express 4.x
- **Database:** PostgreSQL 15
- **Payment SDKs:** Stripe SDK, PayPal REST SDK

## API Documentation

### Core Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | /api/v1/payments/intent | Create payment intent |
| POST | /api/v1/payments/confirm | Confirm payment |
| GET | /api/v1/payments/:paymentId | Get payment details |
| POST | /api/v1/payments/:paymentId/refund | Refund payment |
| GET | /api/v1/payment-methods | List saved payment methods |
| POST | /api/v1/payment-methods | Add payment method |

For complete API reference, see [API Documentation](api/payment-api.md)

## Configuration

### Environment Variables

| Variable | Description | Default |
|----------|-------------|---------|
| `PAYMENT_SERVICE_PORT` | Service port | 3004 |
| `DATABASE_URL` | PostgreSQL connection | - |
| `STRIPE_SECRET_KEY` | Stripe API key | - |
| `PAYPAL_CLIENT_ID` | PayPal client ID | - |
| `PAYPAL_CLIENT_SECRET` | PayPal secret | - |
| `WEBHOOK_SECRET` | Webhook signing secret | - |

### Supported Gateways

- **Stripe** - Credit/debit cards, Apple Pay, Google Pay
- **PayPal** - PayPal wallet, Venmo

## Development

### Prerequisites

- Node.js 18.x
- PostgreSQL 15
- Stripe test account
- PayPal test account

### Setup

```bash
# Install dependencies
npm install

# Set up environment
cp .env.example .env

# Add test API keys
# Edit .env with your test credentials

# Run database migrations
npm run db:migrate

# Start the service
npm start
```

### Running Locally

```bash
# Start with hot reload
npm run dev

# Start Stripe CLI for webhooks
stripe listen --forward-to localhost:3004/webhooks
```

### Common Tasks

**Add a new payment gateway:**
1. Implement `PaymentGateway` interface in `gateways/`
2. Register in `GatewayFactory`
3. Add configuration
4. Add tests

**Add webhook handler:**
1. Create handler in `webhooks/handlers.js`
2. Register in `webhooks/index.js`
3. Verify webhook signature
4. Add tests

## Testing

```bash
# Run all tests
npm test

# Run with Stripe test mode
STRIPE_TEST_MODE=true npm test

# Run integration tests
npm run test:integration
```

### Test Cards

Use Stripe test cards for testing:
- **Success:** 4242 4242 4242 4242
- **Decline:** 4000 0000 0000 0002
- **Insufficient Funds:** 4000 0000 0000 9995

## Deployment

See [Deployment Guide](../../docs/deployment.md)

### Environment Configuration

- **Development:** Test mode, test API keys
- **Staging:** Test mode with staging credentials
- **Production:** Live mode with production credentials

## Troubleshooting

### Common Issues

**Issue: Payment fails with "card_declined"**

**Symptom:** Payment rejected by gateway

**Solution:**
1. Verify card details are correct
2. Check Stripe dashboard for decline reason
3. Ensure test cards are used in development

**Issue: Webhook not received**

**Symptom:** Payment status not updating

**Solution:**
1. Verify webhook endpoint is accessible
2. Check webhook signing secret matches
3. Review Stripe CLI or webhook logs

### Debug Mode

```bash
# Enable debug logging
DEBUG=payment-service:* npm start

# Check Stripe webhook events
stripe logs

# Test webhook delivery
stripe tests trigger webhook.payment_intent.succeeded
```

## FAQ

### How do I add a new payment method?

See the gateway implementation guide in `docs/gateway-guide.md`.

### Are payment details stored?

No, payment details are tokenized by the payment gateway. We only store tokens and last 4 digits.

### How are partial refunds handled?

Use the refund API with an `amount` parameter. Multiple refunds can be processed up to the original payment amount.

## See Also

- [Main Platform README](../../README.md)
- [Payment API Documentation](api/payment-api.md)
- [Stripe Documentation](https://stripe.com/docs)
```

**Step 2: Write Payment API Documentation**

Create `modules/payment-processing/api/payment-api.md`:

```markdown
# Payment Processing API

> Complete API reference for the Payment Processing module

## Base URL

```
http://localhost:3004/api/v1
```

## Authentication

All endpoints require a valid JWT token:
```
Authorization: Bearer <your-jwt-token>
```

## Endpoints

### Payment Intents

#### Create Payment Intent

Create a payment intent for processing payment.

**Request**
```http
POST /payments/intent
Authorization: Bearer <token>
Content-Type: application/json

{
  "orderId": "order_789",
  "amount": 18197,
  "currency": "usd",
  "gateway": "stripe",
  "method": "card"
}
```

**Response** 201 Created
```json
{
  "paymentIntentId": "pi_123...",
  "clientSecret": "pi_123..._secret_...",
  "amount": 18197,
  "currency": "usd",
  "status": "requires_payment_method"
}
```

#### Confirm Payment

Confirm and process the payment.

**Request**
```http
POST /payments/confirm
Authorization: Bearer <token>
Content-Type: application/json

{
  "paymentIntentId": "pi_123...",
  "paymentMethodId": "pm_123..."
}
```

**Response** 200 OK
```json
{
  "paymentId": "pay_456",
  "status": "succeeded",
  "amount": 18197,
  "currency": "usd",
  "gateway": "stripe",
  "gatewayTransactionId": "ch_123...",
  "createdAt": "2024-01-25T10:05:00Z"
}
```

**Error Responses**
- 400 Bad Request - Invalid payment method
- 402 Payment Required - Payment declined
- 404 Not Found - Payment intent not found

### Payments

#### Get Payment

Get payment details by ID.

**Request**
```http
GET /payments/:paymentId
Authorization: Bearer <token>
```

**Response** 200 OK
```json
{
  "id": "pay_456",
  "orderId": "order_789",
  "status": "succeeded",
  "amount": 18197,
  "currency": "usd",
  "gateway": "stripe",
  "gatewayTransactionId": "ch_123...",
  "paymentMethod": {
    "type": "card",
    "last4": "4242",
    "brand": "Visa"
  },
  "timeline": [
    {
      "status": "created",
      "timestamp": "2024-01-25T10:00:00Z"
    },
    {
      "status": "succeeded",
      "timestamp": "2024-01-25T10:05:00Z"
    }
  ],
  "createdAt": "2024-01-25T10:00:00Z",
  "updatedAt": "2024-01-25T10:05:00Z"
}
```

#### Refund Payment

Refund a payment (full or partial).

**Request**
```http
POST /payments/:paymentId/refund
Authorization: Bearer <token>
Content-Type: application/json

{
  "amount": 5000,
  "reason": "customer_request"
}
```

**Parameters**
| Parameter | Type | Description |
|-----------|------|-------------|
| amount | integer | Refund amount in cents (optional, defaults to full refund) |
| reason | string | Reason for refund |

**Response** 200 OK
```json
{
  "refundId": "re_789...",
  "paymentId": "pay_456",
  "amount": 5000,
  "currency": "usd",
  "status": "succeeded",
  "gatewayRefundId": "re_123...",
  "createdAt": "2024-01-25T11:00:00Z"
}
```

### Payment Methods

#### List Payment Methods

Get user's saved payment methods.

**Request**
```http
GET /payment-methods
Authorization: Bearer <token>
```

**Response** 200 OK
```json
{
  "paymentMethods": [
    {
      "id": "pm_123...",
      "type": "card",
      "card": {
        "last4": "4242",
        "brand": "Visa",
        "expiryMonth": 12,
        "expiryYear": 2025
      },
      "isDefault": true
    }
  ]
}
```

#### Add Payment Method

Add a new payment method for future use.

**Request**
```http
POST /payment-methods
Authorization: Bearer <token>
Content-Type: application/json

{
  "paymentMethod": "pm_123...",
  "setAsDefault": true
}
```

**Response** 201 Created

## Webhooks

### Payment Webhooks

The service receives webhook notifications from payment gateways:

- `payment_intent.succeeded` - Payment completed successfully
- `payment_intent.failed` - Payment failed
- `payment_intent.requires_action` - Requires 3D Secure authentication
- `charge.refunded` - Refund processed

Webhook endpoints:
- Stripe: `POST /webhooks/stripe`
- PayPal: `POST /webhooks/paypal`

## Error Responses

### 402 Payment Required
```json
{
  "error": "Payment Required",
  "message": "Card was declined",
  "code": "CARD_DECLINED",
  "declineCode": "insufficient_funds"
}
```

### 400 Bad Request
```json
{
  "error": "Bad Request",
  "message": "Invalid payment amount",
  "code": "INVALID_AMOUNT"
}
```

## See Also

- [Payment Processing Module](../README.md)
- [Platform README](../../README.md)
- [Stripe API Docs](https://stripe.com/docs/api)
```

**Step 3: Commit**

```bash
git add examples/complex-project/modules/payment-processing/
git commit -m "docs: add payment processing module documentation

- Complete README with gateway integration, webhooks
- Detailed API documentation for payments and refunds
- Include testing guide with test cards"
```

---

### Task 8: Create System Architecture Diagram

**Files:**
- Create: `examples/complex-project/docs/images/architecture.png`

**Step 1: Create architecture diagram using draw.io**

1. Go to [diagrams.net](https://diagrams.net)
2. Create a new diagram
3. Draw the system architecture showing:
   - API Gateway at top
   - 4 functional modules (User Management, Product Catalog, Order Processing, Payment Processing)
   - Supporting services (Inventory, Notification)
   - Infrastructure layer (PostgreSQL, Redis, RabbitMQ, Elasticsearch)
4. Export as PNG: `architecture.png`
5. Save source as `architecture.drawio`

**Step 2: Add files to project**

```bash
# Copy files to project
cp architecture.png examples/complex-project/docs/images/
cp architecture.drawio examples/complex-project/docs/images/
```

**Step 3: Commit**

```bash
git add examples/complex-project/docs/images/
git commit -m "docs: add system architecture diagram

- High-level system architecture showing 4 core modules
- Infrastructure layer visualization
- Include drawio source file for future edits"
```

---

### Task 9: Delete Old Documentation

**Files:**
- Delete: `examples/complex-project/docs/architecture.md`
- Delete: `examples/complex-project/docs/development.md`
- Delete: `examples/complex-project/docs/troubleshooting.md`
- Delete: `examples/complex-project/docs/contributing.md`
- Delete: `examples/complex-project/docs/api/` directory

**Step 1: Delete old documentation files**

```bash
cd examples/complex-project/docs

# Delete old monolithic documentation
rm -f architecture.md development.md troubleshooting.md contributing.md

# Delete old API directory
rm -rf api/

# Delete old module directories
rm -rf modules/service-a/
rm -rf modules/service-b/
rm -rf modules/shared/
```

**Step 2: Verify deletion**

Run: `ls -la examples/complex-project/docs/`
Expected: Only `deployment.md`, `images/` remain

**Step 3: Commit**

```bash
git add examples/complex-project/docs/
git add examples/complex-project/modules/
git commit -m "docs: remove old monolithic documentation

- Delete architecture.md, development.md, troubleshooting.md
- Delete old api/ directory
- Delete old module directories (service-a, service-b, shared)
- Content now in modular documentation structure"
```

---

### Task 10: Final Review and Verification

**Files:**
- Verify: All new files created
- Verify: All old files deleted
- Verify: Internal links work

**Step 1: Verify directory structure**

```bash
# Verify structure
tree examples/complex-project/ -L 3 -I 'node_modules'
```

Expected output:
```
examples/complex-project/
├── README.md
├── docs/
│   ├── images/
│   │   ├── architecture.drawio
│   │   ├── architecture.png
│   │   └── .gitkeep
│   └── deployment.md
└── modules/
    ├── user-management/
    │   ├── README.md
    │   ├── api/
    │   │   └── user-api.md
    │   └── images/
    │       └── .gitkeep
    ├── product-catalog/
    │   ├── README.md
    │   ├── api/
    │   │   └── product-api.md
    │   └── images/
    │       └── .gitkeep
    ├── order-processing/
    │   ├── README.md
    │   ├── api/
    │   │   └── order-api.md
    │   └── images/
    │       └── .gitkeep
    └── payment-processing/
        ├── README.md
        ├── api/
        │   └── payment-api.md
        └── images/
            └── .gitkeep
```

**Step 2: Verify README content**

```bash
# Check entry README length (should be ~100-150 lines)
wc -l examples/complex-project/README.md

# Verify module READMEs are complete (should be ~200+ lines each)
wc -l examples/complex-project/modules/*/README.md
```

**Step 3: Check for broken internal links**

```bash
# Check for markdown link syntax errors
grep -r "\](.*\.md)" examples/complex-project/README.md
grep -r "\](.*\.md)" examples/complex-project/modules/
```

**Step 4: Final commit if any adjustments needed**

```bash
git add examples/complex-project/
git commit -m "docs: final adjustments to complex-project reorganization

- Verify all internal links work
- Ensure consistent formatting across modules
- Complete reorganization"
```

---

## Completion Criteria

- [ ] Entry README is concise (~100-150 lines) with high-level overview only
- [ ] All 4 functional modules have complete README documentation (~200+ lines each)
- [ ] Each module has API documentation in api/ subdirectory
- [ ] Old monolithic docs are removed (architecture.md, development.md, troubleshooting.md, api/)
- [ ] All internal links work correctly
- [ ] Architecture diagram created and properly referenced
- [ ] Directory structure matches approved design
