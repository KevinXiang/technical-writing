# Auth Service API Documentation

> Authentication and authorization service API

## Overview

The Auth Service handles user authentication, JWT token generation, and authorization for the E-Commerce Platform.

## Base URL

```
[Production] https://api.example.com/auth/v1
[Staging] https://api-staging.example.com/auth/v1
[Development] http://localhost:3001/v1
```

## Authentication

This service uses JWT (JSON Web Tokens) for authentication.

### Token Format

```javascript
{
  "sub": "user-id",
  "email": "user@example.com",
  "role": "customer",
  "iat": 1640995200,
  "exp": 1640996100
}
```

### Headers

```http
Authorization: Bearer <access-token>
Content-Type: application/json
```

## Endpoints

### Register User

Register a new user account.

```http
POST /auth/register
```

**Request Body:**

```json
{
  "email": "user@example.com",
  "password": "SecurePass123!",
  "name": "John Doe",
  "phone": "+1234567890"
}
```

**Response:** `201 Created`

```json
{
  "data": {
    "id": "user-123",
    "email": "user@example.com",
    "name": "John Doe",
    "createdAt": "2024-01-01T00:00:00Z"
  },
  "tokens": {
    "accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "refreshToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "expiresIn": 900
  }
}
```

### Login

Authenticate user and receive tokens.

```http
POST /auth/login
```

**Request Body:**

```json
{
  "email": "user@example.com",
  "password": "SecurePass123!"
}
```

**Response:** `200 OK`

```json
{
  "data": {
    "id": "user-123",
    "email": "user@example.com",
    "name": "John Doe"
  },
  "tokens": {
    "accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "refreshToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "expiresIn": 900
  }
}
```

### Refresh Token

Refresh access token using refresh token.

```http
POST /auth/refresh
```

**Request Body:**

```json
{
  "refreshToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}
```

**Response:** `200 OK`

```json
{
  "tokens": {
    "accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "refreshToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "expiresIn": 900
  }
}
```

### Logout

Invalidate refresh token.

```http
POST /auth/logout
```

**Request Body:**

```json
{
  "refreshToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}
```

**Response:** `204 No Content`

### Request Password Reset

Initiate password reset flow.

```http
POST /auth/reset-password/request
```

**Request Body:**

```json
{
  "email": "user@example.com"
}
```

**Response:** `200 OK`

```json
{
  "message": "Password reset email sent"
}
```

### Reset Password

Complete password reset with token.

```http
POST /auth/reset-password/confirm
```

**Request Body:**

```json
{
  "token": "reset-token-123",
  "newPassword": "NewSecurePass123!"
}
```

**Response:** `200 OK`

```json
{
  "message": "Password reset successful"
}
```

### Social Login

Authenticate with OAuth2 provider.

```http
POST /auth/social/{provider}
```

**Providers:** `google`, `facebook`, `apple`

**Request Body:**

```json
{
  "code": "oauth2-code-from-provider",
  "redirectUri": "https://example.com/callback"
}
```

**Response:** `200 OK`

```json
{
  "data": {
    "id": "user-123",
    "email": "user@example.com",
    "name": "John Doe",
    "provider": "google",
    "providerId": "google-123"
  },
  "tokens": {
    "accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "refreshToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "expiresIn": 900
  }
}
```

## Data Models

### User

```typescript
interface User {
  id: string;
  email: string;
  name: string;
  phone?: string;
  role: UserRole;
  emailVerified: boolean;
  createdAt: string;
  updatedAt: string;
}
```

### UserRole

```typescript
enum UserRole {
  CUSTOMER = 'customer',
  ADMIN = 'admin',
  SUPPORT = 'support',
  MANAGER = 'manager'
}
```

### Tokens

```typescript
interface Tokens {
  accessToken: string;
  refreshToken: string;
  expiresIn: number; // seconds
}
```

## Error Codes

| Code | Description |
|------|-------------|
| `AUTH_INVALID_CREDENTIALS` | Invalid email or password |
| `AUTH_USER_EXISTS` | User already exists |
| `AUTH_USER_NOT_FOUND` | User not found |
| `AUTH_INVALID_TOKEN` | Invalid or expired token |
| `AUTH_WEAK_PASSWORD` | Password does not meet requirements |
| `AUTH_EMAIL_NOT_VERIFIED` | Email not verified |

## Examples

### Complete Authentication Flow

```javascript
// 1. Register
const registerResponse = await fetch('https://api.example.com/auth/v1/register', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({
    email: 'user@example.com',
    password: 'SecurePass123!',
    name: 'John Doe'
  })
});

const { data, tokens } = await registerResponse.json();

// 2. Use access token
const profileResponse = await fetch('https://api.example.com/user/v1/profile', {
  headers: {
    'Authorization': `Bearer ${tokens.accessToken}`
  }
});

// 3. Refresh token when expired
const refreshResponse = await fetch('https://api.example.com/auth/v1/refresh', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({ refreshToken: tokens.refreshToken })
});

const newTokens = await refreshResponse.json();
```

---

**API Version:** 1.0.0
**Last Updated:** 2024-03-23
