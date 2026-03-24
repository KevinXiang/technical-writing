# Order Processing

> Shopping cart, checkout, and order lifecycle management

## Overview

The Order Processing module handles all order-related functionality including shopping cart management, checkout processing, and order lifecycle management. It integrates with payment processing and inventory services through message queue events.

This module is responsible for cart operations, checkout flow orchestration, order status tracking, and order history retrieval.

## Architecture

![Order Flow](images/order-flow.png)

### Key Components

- **Cart Service** - Manages shopping cart and cart items
- **Checkout Service** - Orchestrates checkout flow across services
- **Order Service** - Manages order lifecycle and status
- **Order History Service** - Retrieves user's past orders

## Tech Stack

- **Language:** Node.js 18.x
- **Framework:** Express
- **Database:** PostgreSQL 15
- **Message Queue:** RabbitMQ 3.12
- **State Machine:** Finite state machine for order states

## API Documentation

### Core Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | /api/cart | Get user's cart |
| POST | /api/cart/items | Add item to cart |
| PUT | /api/cart/items/:id | Update cart item |
| DELETE | /api/cart/items/:id | Remove cart item |
| POST | /api/checkout | Start checkout process |
| GET | /api/orders/:id | Get order details |
| GET | /api/orders | List user's orders |
| PUT | /api/orders/:id/cancel | Cancel order |

For complete API reference, see [API Documentation](api/order-api.md)

## Configuration

### Environment Variables

| Variable | Required | Default | Description |
|----------|----------|---------|-------------|
| JWT_SECRET | Yes | - | JWT signing secret |
| DB_HOST | Yes | localhost | Database host |
| DB_PORT | No | 5432 | Database port |
| DB_NAME | Yes | orders | Database name |
| RABBITMQ_URL | Yes | amqp://localhost | RabbitMQ connection URL |
| PAYMENT_SERVICE_URL | Yes | http://localhost:3004 | Payment service URL |
| INVENTORY_SERVICE_URL | Yes | http://localhost:3005 | Inventory service URL |

## Development

### Prerequisites

- Node.js 18.x
- PostgreSQL 15
- RabbitMQ 3.12
- Running User and Payment services

### Setup

```bash
# Navigate to module directory
cd modules/order-processing

# Install dependencies
npm install

# Set up environment
cp .env.example .env

# Run database migrations
npm run db:migrate

# Seed database
npm run db:seed
```

### Running Locally

```bash
# Start development server
npm run dev

# Server runs on http://localhost:3003
```

### Common Tasks

```bash
# Run tests
npm test

# Run linting
npm run lint

# Consume order events
npm run events:consume
```

## Testing

```bash
# Unit tests
npm run test:unit

# Integration tests
npm run test:integration

# E2E tests
npm run test:e2e

# All tests
npm test
```

## Deployment

For deployment instructions, see [Deployment Guide](../../deployment.md)

## Troubleshooting

### Common Issues

**Issue:** Cart not persisting
**Solution:** Check JWT token validity and user authentication

**Issue:** Checkout failing at payment step
**Solution:** Verify Payment Service is accessible and API key valid

**Issue:** Order status not updating
**Solution:** Check RabbitMQ connection and event consumer is running

## FAQ

### How are order states managed?

Orders use a finite state machine with states: pending → confirmed → processing → shipped → delivered / cancelled.

### What happens when payment fails?

Cart is preserved, payment failure event is emitted, and user can retry checkout.

### Can users modify orders after placement?

Orders can only be cancelled if in "pending" state. No modifications allowed after confirmation.

## See Also

- [Main README](../../../README.md)
- [API Documentation](api/order-api.md)
- [Deployment Guide](../../deployment.md)
