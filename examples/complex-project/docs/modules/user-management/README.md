# User Management

> Authentication, profiles, and user preferences management

## Overview

The User Management module handles all user-related functionality including authentication, profile management, and user preferences. It provides secure JWT-based authentication with OAuth2 support for third-party login providers.

This module is responsible for user registration, login, password recovery, profile updates, and managing user preferences across the platform.

## Architecture

![User Flow](images/user-flow.png)

### Key Components

- **Authentication Service** - Handles login, registration, and token generation
- **Profile Service** - Manages user profile information and preferences
- **Address Book Service** - Manages user shipping addresses
- **Order History Service** - Retrieves user's past orders

## Tech Stack

- **Language:** Node.js 18.x
- **Framework:** Express
- **Database:** PostgreSQL 15
- **Authentication:** JWT + OAuth2
- **Password Hashing:** bcrypt

## API Documentation

### Core Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | /api/auth/register | User registration |
| POST | /api/auth/login | User login |
| POST | /api/auth/refresh | Refresh JWT token |
| GET | /api/users/profile | Get user profile |
| PUT | /api/users/profile | Update user profile |
| GET | /api/users/addresses | Get address book |
| POST | /api/users/addresses | Add new address |
| GET | /api/users/orders | Get order history |

For complete API reference, see [API Documentation](api/user-api.md)

## Configuration

### Environment Variables

| Variable | Required | Default | Description |
|----------|----------|---------|-------------|
| JWT_SECRET | Yes | - | JWT signing secret |
| JWT_EXPIRES_IN | No | 1d | Token expiration time |
| OAUTH_GOOGLE_CLIENT_ID | No | - | Google OAuth client ID |
| DB_HOST | Yes | localhost | Database host |
| DB_PORT | No | 5432 | Database port |
| DB_NAME | Yes | users | Database name |

## Development

### Prerequisites

- Node.js 18.x
- PostgreSQL 15
- Running database infrastructure

### Setup

```bash
# Navigate to module directory
cd modules/user-management

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

# Server runs on http://localhost:3001
```

### Common Tasks

```bash
# Run tests
npm test

# Run linting
npm run lint

# Run with coverage
npm run test:coverage
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

For deployment instructions, see [Deployment Guide](../../../docs/deployment.md#user-management)

## Troubleshooting

### Common Issues

**Issue:** JWT token expired
**Solution:** Use /api/auth/refresh endpoint to get a new token

**Issue:** Database connection failed
**Solution:** Verify DATABASE_URL is correct and database is running

**Issue:** OAuth callback URL mismatch
**Solution:** Ensure callback URL in OAuth provider dashboard matches redirect_uri

## FAQ

### How do I add a new OAuth provider?

Add provider configuration in `src/config/oauth.js` and update callback handler in `src/handlers/oauth.js`.

### How is password security handled?

Passwords are hashed using bcrypt with 12 salt rounds. Never store plain text passwords.

### Can users have multiple addresses?

Yes, users can store multiple shipping addresses. One can be marked as default.

## See Also

- [Main README](../../../README.md)
- [API Documentation](api/user-api.md)
- [Deployment Guide](../../../docs/deployment.md)
