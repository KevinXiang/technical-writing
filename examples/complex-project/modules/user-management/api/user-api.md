# User Management API

Complete API reference for the User Management module.

## Authentication

All endpoints require authentication unless noted otherwise. Include JWT token in Authorization header:

```
Authorization: Bearer <token>
```

## Endpoints

### Authentication Endpoints

#### POST /api/auth/register

Register a new user account.

**Request Body:**
```json
{
  "email": "user@example.com",
  "password": "SecurePass123!",
  "name": "John Doe"
}
```

**Response (201):**
```json
{
  "id": "uuid",
  "email": "user@example.com",
  "name": "John Doe",
  "token": "jwt_token"
}
```

#### POST /api/auth/login

Authenticate user and receive JWT token.

**Request Body:**
```json
{
  "email": "user@example.com",
  "password": "SecurePass123!"
}
```

**Response (200):**
```json
{
  "token": "jwt_token",
  "refreshToken": "refresh_token",
  "expiresIn": "1d"
}
```

#### POST /api/auth/refresh

Refresh expired JWT token.

**Request Body:**
```json
{
  "refreshToken": "refresh_token"
}
```

**Response (200):**
```json
{
  "token": "new_jwt_token",
  "expiresIn": "1d"
}
```

### User Profile Endpoints

#### GET /api/users/profile

Get current user profile.

**Response (200):**
```json
{
  "id": "uuid",
  "email": "user@example.com",
  "name": "John Doe",
  "avatar": "https://...",
  "preferences": {
    "language": "en",
    "currency": "USD"
  }
}
```

#### PUT /api/users/profile

Update user profile.

**Request Body:**
```json
{
  "name": "Jane Doe",
  "avatar": "https://..."
}
```

**Response (200):**
```json
{
  "id": "uuid",
  "email": "user@example.com",
  "name": "Jane Doe",
  "avatar": "https://..."
}
```

### Address Book Endpoints

#### GET /api/users/addresses

Get all user addresses.

**Response (200):**
```json
{
  "addresses": [
    {
      "id": "addr_id",
      "name": "Home",
      "street": "123 Main St",
      "city": "New York",
      "zip": "10001",
      "country": "US",
      "isDefault": true
    }
  ]
}
```

#### POST /api/users/addresses

Add new address.

**Request Body:**
```json
{
  "name": "Office",
  "street": "456 Work Ave",
  "city": "New York",
  "zip": "10002",
  "country": "US",
  "isDefault": false
}
```

**Response (201):**
```json
{
  "id": "new_addr_id",
  "name": "Office",
  "street": "456 Work Ave",
  "city": "New York",
  "zip": "10002",
  "country": "US",
  "isDefault": false
}
```

### Order History Endpoints

#### GET /api/users/orders

Get user's order history.

**Query Parameters:**
- `page` - Page number (default: 1)
- `limit` - Items per page (default: 10)

**Response (200):**
```json
{
  "orders": [
    {
      "id": "order_id",
      "createdAt": "2024-01-15T10:30:00Z",
      "status": "delivered",
      "total": 99.99
    }
  ],
  "pagination": {
    "page": 1,
    "limit": 10,
    "total": 25
  }
}
```

## Error Responses

All endpoints may return these error responses:

### 400 Bad Request
```json
{
  "error": "Validation Error",
  "details": ["email is required"]
}
```

### 401 Unauthorized
```json
{
  "error": "Unauthorized",
  "message": "Invalid or expired token"
}
```

### 409 Conflict
```json
{
  "error": "User already exists"
}
```

## See Also

- [Module README](../README.md)
