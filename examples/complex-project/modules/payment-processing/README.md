# Payment Processing

> Payment gateway integration and transaction handling

## Overview

The Payment Processing module handles all payment-related functionality including payment gateway integration, payment processing, refund handling, and transaction history. It supports multiple payment providers including Stripe and PayPal.

This module is responsible for processing payments, managing payment methods, handling refunds, and maintaining transaction records.

## Architecture

![Payment Flow](images/payment-flow.png)

### Key Components

- **Payment Gateway Service** - Abstracts multiple payment providers (Stripe, PayPal)
- **Payment Service** - Orchestrates payment processing workflow
- **Refund Service** - Handles refund requests and processing
- **Transaction Service** - Manages transaction records and history

## Tech Stack

- **Language:** Node.js 18.x
- **Framework:** Express
- **Database:** PostgreSQL 15
- **Payment Gateways:** Stripe SDK, PayPal SDK
- **Webhooks:** Signature verification for payment events

## API Documentation

### Core Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | /api/payment-methods | List available payment methods |
| POST | /api/payments | Create payment intent |
| GET | /api/payments/:id | Get payment details |
| POST | /api/payments/:id/capture | Capture payment |
| POST | /api/refunds | Create refund |
| GET | /api/transactions | List transactions |

For complete API reference, see [API Documentation](api/payment-api.md)

## Configuration

### Environment Variables

| Variable | Required | Default | Description |
|----------|----------|---------|-------------|
| STRIPE_SECRET_KEY | Yes | - | Stripe API secret key |
| STRIPE_WEBHOOK_SECRET | Yes | - | Stripe webhook signing secret |
| PAYPAL_CLIENT_ID | Yes | - | PayPal client ID |
| PAYPAL_CLIENT_SECRET | Yes | - | PayPal client secret |
| DB_HOST | Yes | localhost | Database host |
| DB_PORT | No | 5432 | Database port |
| DB_NAME | Yes | payments | Database name |

## Development

### Prerequisites

- Node.js 18.x
- PostgreSQL 15
- Stripe test account (for development)
- PayPal test account (for development)

### Setup

```bash
# Navigate to module directory
cd modules/payment-processing

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

# Server runs on http://localhost:3004
```

### Common Tasks

```bash
# Run tests
npm test

# Run linting
npm run lint

# Start webhook listener
npm run webhooks:listen
```

## Testing

```bash
# Unit tests
npm run test:unit

# Integration tests
npm run test:integration

# E2E tests with Stripe test mode
npm run test:e2e

# All tests
npm test
```

## Deployment

For deployment instructions, see [Deployment Guide](../../docs/deployment.md)

## Troubleshooting

### Common Issues

**Issue:** Payment intent creation failing
**Solution:** Verify Stripe API key is valid and account is active

**Issue:** Webhook events not received
**Solution:** Check webhook secret matches Stripe dashboard configuration

**Issue:** Refund processing timeout
**Solution:** Payment provider may be experiencing issues, check status page

## FAQ

### How do I add a new payment gateway?

Implement the `PaymentGateway` interface in `src/gateways/` and register in `src/config/gateways.js`.

### How are webhook signatures verified?

Webhooks use HMAC signature verification. Each provider (Stripe, PayPal) provides a signing secret for validation.

### What happens when payment fails?

Payment failure event is emitted, order service is notified, and user can retry with different payment method.

## See Also

- [Main README](../../README.md)
- [API Documentation](api/payment-api.md)
- [Deployment Guide](../../docs/deployment.md)
