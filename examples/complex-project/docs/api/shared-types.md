# Shared Types and Data Models

> Common data types and models used across services

## Overview

This document defines the shared data types and models that are used across multiple services in the E-Commerce Platform.

## Common Types

### Pagination

```typescript
interface PaginationMeta {
  page: number;
  perPage: number;
  totalPages: number;
  totalItems: number;
}

interface PaginatedResponse<T> {
  data: T[];
  meta: PaginationMeta;
}
```

### Error Response

```typescript
interface ErrorResponse {
  errors: ErrorDetail[];
}

interface ErrorDetail {
  code: string;
  message: string;
  details?: Record<string, any>;
  field?: string;
}
```

### Timestamps

```typescript
interface Timestamps {
  createdAt: string; // ISO 8601 datetime
  updatedAt: string; // ISO 8601 datetime
  deletedAt?: string; // ISO 8601 datetime
}
```

### Money

```typescript
interface Money {
  amount: number;
  currency: string; // ISO 4217 currency code
}
```

### Address

```typescript
interface Address {
  street: string;
  street2?: string;
  city: string;
  state: string;
  zip: string;
  country: string;
  latitude?: number;
  longitude?: number;
}
```

## User Types

### User

```typescript
interface User {
  id: string;
  email: string;
  name: string;
  phone?: string;
  role: UserRole;
  emailVerified: boolean;
  avatar?: string;
  preferences?: UserPreferences;
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

### UserPreferences

```typescript
interface UserPreferences {
  language: string;
  currency: string;
  newsletter: boolean;
  smsNotifications: boolean;
}
```

## Product Types

### ProductReference

```typescript
interface ProductReference {
  id: string;
  name: string;
  slug: string;
  sku: string;
  price: number;
  image: string;
}
```

### CategoryReference

```typescript
interface CategoryReference {
  id: string;
  name: string;
  slug: string;
}
```

## Order Types

### OrderStatus

```typescript
enum OrderStatus {
  PENDING = 'pending',
  CONFIRMED = 'confirmed',
  PROCESSING = 'processing',
  SHIPPED = 'shipped',
  DELIVERED = 'delivered',
  CANCELLED = 'cancelled',
  REFUNDED = 'refunded'
}
```

### OrderItem

```typescript
interface OrderItem {
  id: string;
  product: ProductReference;
  quantity: number;
  unitPrice: number;
  totalPrice: number;
}
```

### Order

```typescript
interface Order {
  id: string;
  orderNumber: string;
  userId: string;
  status: OrderStatus;
  items: OrderItem[];
  subtotal: number;
  tax: number;
  shipping: number;
  total: number;
  shippingAddress: Address;
  billingAddress: Address;
  paymentMethod: string;
  createdAt: string;
  updatedAt: string;
}
```

## Payment Types

### PaymentStatus

```typescript
enum PaymentStatus {
  PENDING = 'pending',
  PROCESSING = 'processing',
  COMPLETED = 'completed',
  FAILED = 'failed',
  REFUNDED = 'refunded'
}
```

### PaymentMethod

```typescript
enum PaymentMethod {
  CREDIT_CARD = 'credit_card',
  DEBIT_CARD = 'debit_card',
  PAYPAL = 'paypal',
  APPLE_PAY = 'apple_pay',
  GOOGLE_PAY = 'google_pay'
}
```

### Payment

```typescript
interface Payment {
  id: string;
  orderId: string;
  amount: number;
  currency: string;
  status: PaymentStatus;
  method: PaymentMethod;
  gateway: string;
  gatewayTransactionId: string;
  createdAt: string;
  updatedAt: string;
}
```

## Inventory Types

### InventoryTransaction

```typescript
interface InventoryTransaction {
  id: string;
  sku: string;
  quantity: number;
  type: TransactionType;
  reason: string;
  orderId?: string;
  timestamp: string;
}

enum TransactionType {
  RESERVE = 'reserve',
  RELEASE = 'release',
  ADJUST = 'adjust',
  SALE = 'sale',
  RETURN = 'return'
}
```

## Event Types

### Event

```typescript
interface Event {
  id: string;
  type: string;
  version: number;
  source: string;
  data: Record<string, any>;
  timestamp: string;
  correlationId?: string;
}
```

### OrderCreated

```typescript
interface OrderCreated extends Event {
  type: 'OrderCreated';
  data: {
    orderId: string;
    userId: string;
    items: OrderItem[];
    total: number;
  };
}
```

### PaymentCompleted

```typescript
interface PaymentCompleted extends Event {
  type: 'PaymentCompleted';
  data: {
    paymentId: string;
    orderId: string;
    amount: number;
  };
}
```

### InventoryReserved

```typescript
interface InventoryReserved extends Event {
  type: 'InventoryReserved';
  data: {
    orderId: string;
    items: Array<{
      sku: string;
      quantity: number;
    }>;
  };
}
```

## Validation Rules

### Email Validation

```typescript
const EMAIL_REGEX = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
```

### Phone Validation

```typescript
const PHONE_REGEX = /^\+?[1-9]\d{1,14}$/;
```

### Password Requirements

- Minimum 8 characters
- At least one uppercase letter
- At least one lowercase letter
- At least one number
- At least one special character

### SKU Validation

```typescript
const SKU_REGEX = /^[A-Z0-9-]{3,50}$/;
```

## HTTP Status Codes

| Code | Description |
|------|-------------|
| 200 | Success |
| 201 | Created |
| 204 | No Content |
| 400 | Bad Request |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | Not Found |
| 409 | Conflict |
| 422 | Unprocessable Entity |
| 429 | Too Many Requests |
| 500 | Internal Server Error |
| 503 | Service Unavailable |

## Common Error Codes

| Code | Description |
|------|-------------|
| `VALIDATION_ERROR` | Request validation failed |
| `NOT_FOUND` | Resource not found |
| `UNAUTHORIZED` | Authentication required |
| `FORBIDDEN` | Insufficient permissions |
| `CONFLICT` | Resource conflict |
| `RATE_LIMIT_EXCEEDED` | Rate limit exceeded |
| `INTERNAL_ERROR` | Internal server error |

---

**Version:** 1.0.0
**Last Updated:** 2024-03-23
