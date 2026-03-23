# Development Guide

> Developer guide for the E-Commerce Platform

## Overview

This guide provides comprehensive information for developers working on the E-Commerce Platform, including setup, workflows, and best practices.

## Getting Started

### Prerequisites

- **Docker** - 20.x or higher
- **Docker Compose** - 2.x or higher
- **Node.js** - 18.x (for local Node.js services)
- **Python** - 3.11 (for local Python services)
- **Go** - 1.21 (for local Go services)
- **Make** - Build automation

### Initial Setup

```bash
# Clone the repository
git clone https://github.com/example/ecommerce-platform.git
cd ecommerce-platform

# Start infrastructure services
docker-compose up -d postgres redis rabbitmq elasticsearch

# Install dependencies for all services
make install-all

# Copy environment template
cp .env.example .env
# Edit .env with your configuration

# Initialize databases
make db-migrate

# Seed sample data
make db-seed

# Start all services
make start-all
```

### Verifying Setup

```bash
# Health check
curl http://localhost:3000/health

# Run smoke tests
make smoke-test

# Check service status
make status
```

## Development Environment

### Project Structure

```
ecommerce-platform/
├── modules/
│   ├── auth-service/       # Authentication service
│   ├── user-service/       # User management
│   ├── product-service/    # Product catalog
│   ├── order-service/      # Order processing
│   ├── inventory-service/  # Inventory management
│   ├── payment-service/    # Payment processing
│   └── notification-service/ # Notifications
├── docs/                   # Documentation
├── scripts/                # Utility scripts
├── docker-compose.yml      # Local infrastructure
├── Makefile               # Build automation
└── README.md              # This file
```

### Local Development

```bash
# Start all services
make start-all

# Start specific service
make start-service-auth

# View logs
make logs-service-auth

# Stop all services
make stop-all
```

## Testing

```bash
# Run all tests
make test-all

# Run unit tests
make test-unit

# Run integration tests
make test-integration

# Run E2E tests
make test-e2e

# Run with coverage
make test-coverage
```

## Common Tasks

### Adding a New Service

1. Create service directory under `modules/`
2. Implement service with health check endpoint
3. Add Dockerfile
4. Update docker-compose.yml
5. Add Make targets
6. Update API documentation

### Database Migration

```bash
# Create migration
make db-migrate-create service=name

# Run migrations
make db-migrate

# Rollback migration
make db-rollback
```

## Troubleshooting

See [Troubleshooting Guide](troubleshooting.md) for detailed troubleshooting information.

---

**Last Updated:** 2024-03-23
